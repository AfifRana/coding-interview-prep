# SQL Problem Solving Ideas

This guide explains SQL interview thinking with practical patterns, examples, and common pitfalls.
It covers common interview patterns, not every SQL dialect or database-specific feature.

---

## 1) Filtering and Projection

### Goal
Return only required columns and rows.

### Core clauses
- `SELECT`, `WHERE`, `ORDER BY`, `LIMIT`

### Example
```sql
SELECT e.id AS employee_id, e.name AS employee_name
FROM employees e
JOIN departments d ON d.id = e.department_id
WHERE d.name = 'Engineering'
ORDER BY e.id;
```
Practice: [Employees in a Department](sql-problem-set.md#easy-1-employees-in-a-department).

### Pitfalls
- Using `SELECT *` unnecessarily.
- Forgetting deterministic order when using `LIMIT`.

---

## 2) Aggregation (`GROUP BY`, `HAVING`)

### Goal
Summarize data by category.

### Example
Count orders per customer, only customers with at least 5 orders:
```sql
SELECT c.id AS customer_id, COUNT(o.id) AS order_count
FROM customers c
JOIN orders o ON o.customer_id = c.id
GROUP BY c.id
HAVING COUNT(o.id) >= 5
ORDER BY c.id;
```
Practice: [Customers with at Least Five Orders](sql-problem-set.md#hard-6-customers-with-at-least-five-orders).

### Pitfalls
- Filtering aggregated results in `WHERE` instead of `HAVING`.

---

## 3) Joins

### Goal
Combine data from multiple tables.

### Common join intent
- `INNER JOIN`: only matches
- `LEFT JOIN`: keep all rows from left side

### Example
Customers and their order count (including zero):
```sql
SELECT c.id AS customer_id, c.name AS customer_name, COUNT(o.id) AS order_count
FROM customers c
LEFT JOIN orders o ON c.id = o.customer_id
GROUP BY c.id, c.name
ORDER BY c.id;
```
Practice: [Order Count per Customer](sql-problem-set.md#easy-2-order-count-per-customer).

### Pitfalls
- Join key mistakes causing row explosion.
- `COUNT(*)` with a left join counts the preserved left row even without a match; count the non-NULL right-side key instead, such as `COUNT(o.id)`.

---

## 4) Subqueries

### Goal
Use query results inside another query.

### Example
Find products priced above average:
```sql
SELECT id AS product_id, name AS product_name, price
FROM products
WHERE price > (SELECT AVG(price) FROM products)
ORDER BY id;
```
Practice: [Products Above Average Price](sql-problem-set.md#easy-4-products-above-average-price).

### Pitfalls
- Correlated subqueries can be slow if not optimized.

---

## 5) CTE (Common Table Expression)

### Goal
Split complex query into readable steps.

### Example
Top customer by total spend:
```sql
WITH customer_spend AS (
  SELECT customer_id, SUM(amount) AS total_spend,
         RANK() OVER (ORDER BY SUM(amount) DESC) AS spend_rank
  FROM orders
  WHERE customer_id IS NOT NULL
  GROUP BY customer_id
)
SELECT customer_id, total_spend
FROM customer_spend
WHERE spend_rank = 1
ORDER BY customer_id;
```
Practice: [Top Customer by Total Spend](sql-problem-set.md#hard-7-top-customer-by-total-spend).

### Pitfalls
- Overusing CTEs for tiny tasks can reduce clarity.

---

## 6) Window Functions

### Goal
Compute analytics without collapsing rows.

### Example: Rank salary within department
```sql
SELECT id AS employee_id, department_id, salary,
       DENSE_RANK() OVER (PARTITION BY department_id ORDER BY salary DESC) AS dept_rank
FROM employees
WHERE department_id IS NOT NULL
ORDER BY department_id, dept_rank, employee_id;
```
Practice: [Salary Rank within Each Department](sql-problem-set.md#hard-8-salary-rank-within-each-department).

### Example: Running total by date
```sql
SELECT id AS order_id, order_date, amount,
       SUM(amount) OVER (
         ORDER BY order_date, id
         ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
       ) AS running_revenue
FROM orders
ORDER BY order_date, id;
```
Practice: [Running Order Revenue](sql-problem-set.md#hard-9-running-order-revenue).

### Pitfalls
- Confusing `ROW_NUMBER`, `RANK`, `DENSE_RANK` behavior with ties.

---

## 7) Date/Time Analysis

### Goal
Analyze trends over time.

### Example
Monthly revenue:
```sql
SELECT strftime('%Y-%m', order_date) AS month,
       SUM(amount) AS revenue
FROM orders
WHERE order_date >= date('2025-03-15', 'start of month', '-11 months')
  AND order_date < date('2025-03-15', 'start of month', '+1 month')
GROUP BY strftime('%Y-%m', order_date)
ORDER BY month;
```
This example uses SQLite syntax and a fixed reference date; other SQL dialects have different date functions.
Practice: [Monthly Revenue for Twelve Calendar Months](sql-problem-set.md#medium-3-monthly-revenue-for-twelve-calendar-months).

### Pitfalls
- Timezone/date boundary misunderstandings.

---

## 8) Conditional Logic (`CASE WHEN`)

### Goal
Create derived categories.

### Example
Segment orders by amount:
```sql
SELECT id AS order_id,
       amount,
       CASE
         WHEN amount < 50 THEN 'small'
         WHEN amount < 200 THEN 'medium'
         ELSE 'large'
       END AS order_size
FROM orders
ORDER BY id;
```
Practice: [Order Amount Segments](sql-problem-set.md#hard-10-order-amount-segments).

---

## 9) Deduplication Patterns

### Goal
Identify/remove duplicate records.

### Example: Detect duplicates by email
```sql
SELECT email, COUNT(*) AS customer_count
FROM customers
WHERE email IS NOT NULL
GROUP BY email
HAVING COUNT(*) > 1
ORDER BY email COLLATE BINARY;
```
Practice: [Duplicate Email Addresses](sql-problem-set.md#medium-5-duplicate-email-addresses).

### Example: Keep latest record per user
```sql
WITH ranked AS (
  SELECT user_id, activity_date,
         ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY activity_date DESC) AS rn
  FROM user_activity
)
SELECT user_id, activity_date AS latest_activity_date
FROM ranked
WHERE rn = 1
ORDER BY user_id;
```
Practice: [Latest Activity per User](sql-problem-set.md#hard-11-latest-activity-per-user).

---

## 10) Interview-Common Advanced Patterns

### A) Second highest salary
```sql
SELECT MAX(salary) AS second_highest
FROM employees
WHERE salary < (SELECT MAX(salary) FROM employees);
```
Practice: [Second Highest Distinct Salary](sql-problem-set.md#medium-1-second-highest-distinct-salary).

### B) Customers with no orders
```sql
SELECT c.id AS customer_id, c.name AS customer_name
FROM customers c
LEFT JOIN orders o ON c.id = o.customer_id
WHERE o.id IS NULL
ORDER BY c.id;
```
Practice: [Customers Without Orders](sql-problem-set.md#medium-2-customers-without-orders).

### C) Gaps and islands (consecutive dates)
For SQLite dates, assign each user's activity dates a row number and group rows by `date(activity_date, printf('-%d days', rn))`. Consecutive dates share the same shifted date.
```sql
WITH numbered AS (
  SELECT user_id, activity_date,
         ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY activity_date) AS rn
  FROM user_activity
),
grouped AS (
  SELECT user_id, activity_date,
         date(activity_date, printf('-%d days', rn)) AS group_day
  FROM numbered
)
SELECT user_id, MIN(activity_date) AS streak_start,
       MAX(activity_date) AS streak_end, COUNT(*) AS streak_days
FROM grouped
GROUP BY user_id, group_day
ORDER BY user_id, streak_start;
```
Practice: [Consecutive Activity Streaks](sql-problem-set.md#hard-4-consecutive-activity-streaks).

---

## 11) NULL Semantics

Comparisons with `NULL` evaluate to unknown, so `column = NULL` and `column <> NULL` do not match rows. Use `IS NULL` / `IS NOT NULL`. `COUNT(*)` counts rows, while `COUNT(column)` skips NULL values.

For anti-joins, prefer `NOT EXISTS` when the subquery can contain NULLs; `NOT IN` can return no rows if its input includes NULL.

```sql
SELECT c.id AS customer_id, c.name AS customer_name
FROM customers c
WHERE NOT EXISTS (
  SELECT 1
  FROM orders o
  WHERE o.customer_id = c.id
)
ORDER BY c.id;
```
Practice: [Customers Without Orders](sql-problem-set.md#medium-2-customers-without-orders).

## 12) Set Operations

`UNION` removes duplicates; `UNION ALL` keeps them. `INTERSECT` returns rows present in both query results, and `EXCEPT` returns rows from the first result that are absent from the second. Both sides must return the same number of type-compatible columns.

```sql
SELECT customer_id FROM orders
WHERE customer_id IS NOT NULL AND amount >= 100
INTERSECT
SELECT customer_id FROM orders
WHERE customer_id IS NOT NULL AND order_date >= '2025-01-01';
```

In SQLite, `EXCEPT` is supported; other databases may use a different equivalent such as `MINUS`.
Practice: [Customers with Orders in Both Periods](sql-problem-set.md#medium-6-customers-with-orders-in-both-periods).

## 13) Recursive CTEs

Use recursive CTEs to traverse hierarchies such as employee-manager relationships. The anchor query selects the root; the recursive query joins each discovered row to its children.

```sql
WITH RECURSIVE org AS (
  SELECT id, name, manager_id, 1 AS level, name AS path
  FROM employees
  WHERE manager_id IS NULL
  UNION ALL
  SELECT e.id, e.name, e.manager_id, org.level + 1,
         org.path || ' > ' || e.name
  FROM employees e
  JOIN org ON e.manager_id = org.id
)
SELECT id AS employee_id, name AS employee_name, level, path
FROM org
ORDER BY path COLLATE BINARY;
```
Practice: [Employee Org Chart](sql-problem-set.md#hard-5-employee-org-chart).

## Performance mindset (important)

1. Filter early (`WHERE`) before joins/aggregations.
2. Select only needed columns.
3. Ensure join keys are indexed (or logically optimized).
4. Avoid unnecessary nested subqueries.
5. Use window functions where they simplify logic and reduce self-joins.
6. Inspect query plans with `EXPLAIN` or the database's equivalent; add indexes based on the actual filters, joins, and ordering.

---

## SQL problem-solving checklist

1. Clarify expected output columns.
2. Identify required tables and join keys.
3. Decide row-level vs grouped vs window-level result.
4. Build query step-by-step (CTE if needed).
5. Validate edge cases (nulls, ties, duplicates, no matches).
6. Explain complexity and performance trade-offs.
