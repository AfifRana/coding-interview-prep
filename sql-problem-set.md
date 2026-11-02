# SQL Problem Set

Each problem has a statement, a sample solution (Postgres-flavoured standard SQL; notes for dialect differences), and the key idea. Try writing the query yourself first. See [sql-problem-solving-ideas.md](sql-problem-solving-ideas.md) for the underlying patterns.

## Sample Schema

```sql
employees(id, name, salary, department_id, manager_id, hire_date)
departments(id, name)
customers(id, name, email, city)
orders(id, customer_id, order_date, amount)
products(id, name, category, price)
order_items(order_id, product_id, quantity)
user_activity(user_id, activity_date)
```

---

## Easy

### 1. Employees in a department
**Problem:** List all employees in the `Engineering` department.

```sql
SELECT e.id, e.name
FROM employees e
JOIN departments d ON d.id = e.department_id
WHERE d.name = 'Engineering';
```
**Idea:** `JOIN` + filter on the joined table.

### 2. Orders per customer
**Problem:** Count orders per customer, including customers with zero orders.

```sql
SELECT c.id, c.name, COUNT(o.id) AS order_count
FROM customers c
LEFT JOIN orders o ON o.customer_id = c.id
GROUP BY c.id, c.name;
```
**Idea:** `LEFT JOIN` keeps customers without orders; `COUNT(o.id)` ignores NULLs so they get 0 (`COUNT(*)` would give 1).

### 3. Top 5 highest-paid employees
```sql
SELECT id, name, salary
FROM employees
ORDER BY salary DESC
LIMIT 5;  -- SQL Server: SELECT TOP 5; Oracle: FETCH FIRST 5 ROWS ONLY
```
**Idea:** `ORDER BY ... DESC` + limit. Use `RANK()`/`DENSE_RANK()` instead if ties must be included.

### 4. Products priced above average
```sql
SELECT id, name, price
FROM products
WHERE price > (SELECT AVG(price) FROM products);
```
**Idea:** Scalar subquery in `WHERE`.

### 5. Unique cities
```sql
SELECT DISTINCT city
FROM customers
WHERE city IS NOT NULL;
```
**Idea:** `DISTINCT`; decide explicitly how to treat NULLs.

---

## Medium

### 1. Second highest salary
**Problem:** Return the second highest distinct salary, or `NULL` if none.

```sql
SELECT (
  SELECT DISTINCT salary
  FROM employees
  ORDER BY salary DESC
  LIMIT 1 OFFSET 1
) AS second_highest_salary;
```
Alternative with a window function:
```sql
SELECT MAX(salary) AS second_highest_salary
FROM (
  SELECT salary, DENSE_RANK() OVER (ORDER BY salary DESC) AS rnk
  FROM employees
) t
WHERE rnk = 2;
```
**Idea:** Wrapping in a scalar subquery (or `MAX`) makes the result `NULL` rather than an empty set. Use `DENSE_RANK` for distinct values.

### 2. Customers with no orders
```sql
SELECT c.id, c.name
FROM customers c
LEFT JOIN orders o ON o.customer_id = c.id
WHERE o.id IS NULL;
```
Or `WHERE NOT EXISTS (SELECT 1 FROM orders o WHERE o.customer_id = c.id)`.

**Idea:** Anti-join. Avoid `NOT IN` with a nullable subquery column (a single NULL makes it return nothing).

### 3. Monthly revenue for the last 12 months
```sql
SELECT DATE_TRUNC('month', order_date) AS month,
       SUM(amount) AS revenue
FROM orders
WHERE order_date >= DATE_TRUNC('month', CURRENT_DATE) - INTERVAL '11 months'
GROUP BY 1
ORDER BY 1;
```
**Idea:** Truncate dates to the month and aggregate. (MySQL uses `DATE_FORMAT`, SQL Server `DATETRUNC`/`EOMONTH`.) To show months with zero revenue, left join a generated month calendar.

### 4. Top 3 products per category by revenue
```sql
WITH revenue AS (
  SELECT p.category, p.id AS product_id, p.name,
         SUM(oi.quantity * p.price) AS total
  FROM products p
  JOIN order_items oi ON oi.product_id = p.id
  GROUP BY p.category, p.id, p.name
)
SELECT category, name, total
FROM (
  SELECT *, ROW_NUMBER() OVER (PARTITION BY category ORDER BY total DESC) AS rn
  FROM revenue
) r
WHERE rn <= 3;
```
**Idea:** Aggregate first, then rank within each group using `ROW_NUMBER() OVER (PARTITION BY ...)`. Use `RANK` if ties should all be kept.

### 5. Duplicate emails
```sql
SELECT email, COUNT(*) AS cnt
FROM customers
GROUP BY email
HAVING COUNT(*) > 1;
```
**Idea:** Filter groups with `HAVING`, not `WHERE`.

---

## Hard

### 1. Rolling 7-day active users
**Problem:** For each date, count distinct users active in the 7 days ending on that date.

```sql
WITH days AS (
  SELECT DISTINCT activity_date AS d FROM user_activity
)
SELECT days.d,
       COUNT(DISTINCT a.user_id) AS active_7d
FROM days
JOIN user_activity a
  ON a.activity_date BETWEEN days.d - INTERVAL '6 days' AND days.d
GROUP BY days.d
ORDER BY days.d;
```
**Idea:** A window frame cannot do `COUNT(DISTINCT)` in most engines, so use a range self-join against a date list. Make sure the date list has no gaps if zero days must appear.

### 2. Retention cohorts
**Problem:** Group users by the month of their first activity and report, for each cohort, how many are still active N months later.

```sql
WITH first_seen AS (
  SELECT user_id, DATE_TRUNC('month', MIN(activity_date)) AS cohort
  FROM user_activity
  GROUP BY user_id
),
activity AS (
  SELECT DISTINCT user_id, DATE_TRUNC('month', activity_date) AS m
  FROM user_activity
)
SELECT f.cohort,
       (EXTRACT(YEAR FROM a.m) - EXTRACT(YEAR FROM f.cohort)) * 12
         + EXTRACT(MONTH FROM a.m) - EXTRACT(MONTH FROM f.cohort) AS month_number,
       COUNT(DISTINCT a.user_id) AS users
FROM first_seen f
JOIN activity a USING (user_id)
GROUP BY f.cohort, month_number
ORDER BY f.cohort, month_number;
```
**Idea:** Compute each user's cohort with `MIN`, join to later activity, and compute the period offset. Divide `users` by the `month_number = 0` count for retention %.

### 3. Median salary per department
```sql
SELECT department_id,
       PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY salary) AS median_salary
FROM employees
GROUP BY department_id;
```
Portable version (no percentile function):
```sql
SELECT department_id, AVG(salary) AS median_salary
FROM (
  SELECT department_id, salary,
         ROW_NUMBER() OVER (PARTITION BY department_id ORDER BY salary) AS rn,
         COUNT(*)     OVER (PARTITION BY department_id)                 AS cnt
  FROM employees
) t
WHERE rn IN ((cnt + 1) / 2, (cnt + 2) / 2)
GROUP BY department_id;
```
**Idea:** The median is the middle row (odd count) or the average of the two middle rows (even count). Integer division picks both cases.

### 4. Gaps and islands (consecutive date streaks)
**Problem:** For each user, find streaks of consecutive active days.

```sql
WITH d AS (
  SELECT DISTINCT user_id, activity_date FROM user_activity
),
grouped AS (
  SELECT user_id, activity_date,
         activity_date - (ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY activity_date))::int AS grp
  FROM d
)
SELECT user_id,
       MIN(activity_date) AS streak_start,
       MAX(activity_date) AS streak_end,
       COUNT(*)           AS streak_days
FROM grouped
GROUP BY user_id, grp
ORDER BY user_id, streak_start;
```
**Idea:** Within a streak, date minus row number stays constant, so that constant becomes the island's group key. Dedupe dates first. (In Postgres, `date - integer` subtracts days; other engines use `DATEADD`/`DATE_SUB`.)

### 5. Org chart with a recursive CTE
**Problem:** List every employee with their management level and path from the CEO (`manager_id IS NULL`).

```sql
WITH RECURSIVE org AS (
  SELECT id, name, manager_id, 1 AS level, name::text AS path
  FROM employees
  WHERE manager_id IS NULL
  UNION ALL
  SELECT e.id, e.name, e.manager_id, o.level + 1, o.path || ' > ' || e.name
  FROM employees e
  JOIN org o ON e.manager_id = o.id
)
SELECT * FROM org ORDER BY path;
```
**Idea:** Anchor member (roots) + recursive member (children join to previous level). Guard against cycles in dirty data with a depth limit. (SQL Server omits the `RECURSIVE` keyword.)

---

## Tips
- State assumptions: NULL handling, ties, duplicates, time zones.
- Check `JOIN` cardinality before aggregating to avoid inflated sums.
- Prefer `EXISTS` over `IN` for correlated checks on nullable columns.
- Read `EXPLAIN` output and confirm the filters can use indexes.
