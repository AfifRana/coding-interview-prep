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
SELECT employee_id, name
FROM employees
WHERE department = 'Engineering'
ORDER BY employee_id;
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
SELECT product_id, product_name, price
FROM products
WHERE price > (SELECT AVG(price) FROM products);
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
  SELECT customer_id, SUM(amount) AS total_spend
  FROM orders
  GROUP BY customer_id
)
SELECT customer_id, total_spend
FROM customer_spend
ORDER BY total_spend DESC
LIMIT 1;
```

### Pitfalls
- Overusing CTEs for tiny tasks can reduce clarity.

---

## 6) Window Functions

### Goal
Compute analytics without collapsing rows.

### Example: Rank salary within department
```sql
SELECT employee_id, department, salary,
       DENSE_RANK() OVER (PARTITION BY department ORDER BY salary DESC) AS dept_rank
FROM employees;
```

### Example: Running total by date
```sql
SELECT order_date,
       amount,
       SUM(amount) OVER (ORDER BY order_date) AS running_revenue
FROM daily_sales;
```

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
SELECT order_id,
       amount,
       CASE
         WHEN amount < 50 THEN 'small'
         WHEN amount < 200 THEN 'medium'
         ELSE 'large'
       END AS order_size
FROM orders;
```

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
  SELECT *,
         ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY updated_at DESC) AS rn
  FROM user_events
)
SELECT *
FROM ranked
WHERE rn = 1;
```

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
