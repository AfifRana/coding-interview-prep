# SQL Problem Solving Ideas

This guide explains SQL interview thinking with practical patterns, examples, and common pitfalls.

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
SELECT customer_id, COUNT(*) AS order_count
FROM orders
GROUP BY customer_id
HAVING COUNT(*) >= 5;
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
SELECT c.customer_id, c.name, COUNT(o.order_id) AS total_orders
FROM customers c
LEFT JOIN orders o ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.name;
```

### Pitfalls
- Join key mistakes causing row explosion.
- `COUNT(*)` with left join may count non-null left rows unexpectedly; use `COUNT(o.order_id)`.

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
SELECT DATE_TRUNC('month', order_date) AS month,
       SUM(amount) AS revenue
FROM orders
GROUP BY 1
ORDER BY 1;
```

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
SELECT email, COUNT(*) AS cnt
FROM users
GROUP BY email
HAVING COUNT(*) > 1;
```

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

### B) Customers with no orders
```sql
SELECT c.customer_id, c.name
FROM customers c
LEFT JOIN orders o ON c.customer_id = o.customer_id
WHERE o.order_id IS NULL;
```

### C) Gaps and islands (consecutive dates)
Use `date - row_number` grouping trick after ordering dates.

---

## Performance mindset (important)

1. Filter early (`WHERE`) before joins/aggregations.
2. Select only needed columns.
3. Ensure join keys are indexed (or logically optimized).
4. Avoid unnecessary nested subqueries.
5. Use window functions where they simplify logic and reduce self-joins.

---

## SQL problem-solving checklist

1. Clarify expected output columns.
2. Identify required tables and join keys.
3. Decide row-level vs grouped vs window-level result.
4. Build query step-by-step (CTE if needed).
5. Validate edge cases (nulls, ties, duplicates, no matches).
6. Explain complexity and performance trade-offs.

## Matching practice problems

| Guide example | Problem set exercise |
|---|---|
| Filter employees by department | [Employees in a Department](sql-problem-set.md#easy-1-employees-in-a-department) |
| Aggregate orders and filter with `HAVING` | [Customers with at Least Five Orders](sql-problem-set.md#hard-6-customers-with-at-least-five-orders) |
| Include customers with zero orders | [Order Count per Customer](sql-problem-set.md#easy-2-order-count-per-customer) |
| Compare product prices with the average | [Products Above Average Price](sql-problem-set.md#easy-4-products-above-average-price) |
| Find the top-spending customer | [Top Customer by Total Spend](sql-problem-set.md#hard-7-top-customer-by-total-spend) |
| Rank salary within departments | [Salary Rank within Each Department](sql-problem-set.md#hard-8-salary-rank-within-each-department) |
| Compute a running revenue total | [Running Order Revenue](sql-problem-set.md#hard-9-running-order-revenue) |
| Group revenue by month | [Monthly Revenue for Twelve Calendar Months](sql-problem-set.md#medium-3-monthly-revenue-for-twelve-calendar-months) |
| Segment orders with `CASE` | [Order Amount Segments](sql-problem-set.md#hard-10-order-amount-segments) |
| Detect duplicate email addresses | [Duplicate Email Addresses](sql-problem-set.md#medium-5-duplicate-email-addresses) |
| Keep the latest activity per user | [Latest Activity per User](sql-problem-set.md#hard-11-latest-activity-per-user) |
| Find the second-highest distinct salary | [Second Highest Distinct Salary](sql-problem-set.md#medium-1-second-highest-distinct-salary) |
| Find customers without orders | [Customers Without Orders](sql-problem-set.md#medium-2-customers-without-orders) |
| Group consecutive dates into streaks | [Consecutive Activity Streaks](sql-problem-set.md#hard-4-consecutive-activity-streaks) |
