# SQL Problem Set

Write a SQL query for each prompt using the shared schema and exact output contract. Every fixture is independent and starts from empty tables. Expected results are shown for each example and listed test case.

## How to use

Each exercise is a query-writing problem, not a request to reproduce a provided solution. Create the shared schema below, load one fixture at a time into a fresh database, and write a query that returns exactly the requested columns and rows. Unless stated otherwise, compare values exactly and order by the columns specified in the problem. The displayed fixtures and expected results are executable tests; they are not the only valid database contents.

The schema uses SQLite-compatible types and syntax for fixtures. Problem statements describe behavior rather than requiring a particular SQL dialect. Tables are empty at the start of each fixture.

## Shared schema

```sql
CREATE TABLE departments (
  id INTEGER PRIMARY KEY,
  name TEXT NOT NULL UNIQUE
);
CREATE TABLE employees (
  id INTEGER PRIMARY KEY,
  name TEXT NOT NULL,
  salary INTEGER NOT NULL,
  department_id INTEGER REFERENCES departments(id),
  manager_id INTEGER REFERENCES employees(id),
  hire_date TEXT
);
CREATE TABLE customers (
  id INTEGER PRIMARY KEY,
  name TEXT NOT NULL,
  email TEXT,
  city TEXT
);
CREATE TABLE orders (
  id INTEGER PRIMARY KEY,
  customer_id INTEGER REFERENCES customers(id),
  order_date TEXT NOT NULL,
  amount NUMERIC NOT NULL
);
CREATE TABLE products (
  id INTEGER PRIMARY KEY,
  name TEXT NOT NULL,
  category TEXT NOT NULL,
  price NUMERIC NOT NULL
);
CREATE TABLE order_items (
  order_id INTEGER REFERENCES orders(id),
  product_id INTEGER REFERENCES products(id),
  quantity INTEGER NOT NULL,
  PRIMARY KEY (order_id, product_id)
);
CREATE TABLE user_activity (
  user_id INTEGER NOT NULL,
  activity_date TEXT NOT NULL,
  PRIMARY KEY (user_id, activity_date)
);
```

Dates in fixtures use ISO `YYYY-MM-DD`. Each test-case fixture is loaded independently after creating the shared schema. SQL result tables show column names and row order.

## Table of Contents

- [Easy](#easy)
  - [Employees in a Department](#easy-1-employees-in-a-department)
  - [Order Count per Customer](#easy-2-order-count-per-customer)
  - [Top Five Salaries](#easy-3-top-five-salaries)
  - [Products Above Average Price](#easy-4-products-above-average-price)
  - [Unique Customer Cities](#easy-5-unique-customer-cities)
- [Medium](#medium)
  - [Second Highest Distinct Salary](#medium-1-second-highest-distinct-salary)
  - [Customers Without Orders](#medium-2-customers-without-orders)
  - [Monthly Revenue for Twelve Calendar Months](#medium-3-monthly-revenue-for-twelve-calendar-months)
  - [Top Three Products per Category](#medium-4-top-three-products-per-category)
  - [Duplicate Email Addresses](#medium-5-duplicate-email-addresses)
- [Hard](#hard)
  - [Rolling Seven-Day Active Users](#hard-1-rolling-seven-day-active-users)
  - [Monthly Retention Cohorts](#hard-2-monthly-retention-cohorts)
  - [Median Salary per Department](#hard-3-median-salary-per-department)
  - [Consecutive Activity Streaks](#hard-4-consecutive-activity-streaks)
  - [Employee Org Chart](#hard-5-employee-org-chart)

---

## Easy

### Easy 1. Employees in a Department

Return `employee_id` and `employee_name` for employees in the department named `Engineering`. Include every matching employee exactly once. Order by `employee_id` ascending.

**Schema:** `employees(id, name, salary, department_id)` and `departments(id, name)`.

**Example fixture:**
```sql
INSERT INTO departments VALUES (1, 'Engineering'), (2, 'Sales');
INSERT INTO employees (id,name,salary,department_id) VALUES
  (1,'Ari',100,1),(2,'Bo',90,2),(3,'Cy',110,1);
```
**Expected result:**
| employee_id | employee_name |
|---:|---|
| 1 | Ari |
| 3 | Cy |

**Test cases:**
1. **Department has no employees**
   ```sql
   INSERT INTO departments VALUES (1,'Engineering');
   INSERT INTO employees (id,name,salary,department_id) VALUES (1,'Ari',100,NULL);
   ```
   Expected columns: `employee_id, employee_name`; rows: *(empty)*.
2. **No department with requested name**
   ```sql
   INSERT INTO departments VALUES (1,'Sales');
   INSERT INTO employees (id,name,salary,department_id) VALUES (1,'Ari',100,1);
   ```
   Expected columns: `employee_id, employee_name`; rows: *(empty)*.

### Easy 2. Order Count per Customer

Return one row for every customer with `customer_id`, `customer_name`, and `order_count`. Customers with no orders must appear with a count of zero. Order by customer ID ascending.

**Schema:** `customers(id, name)` and `orders(id, customer_id, order_date, amount)`.

**Example fixture:**
```sql
INSERT INTO customers (id,name) VALUES (1,'Ari'),(2,'Bo'),(3,'Cy');
INSERT INTO orders (id,customer_id,order_date,amount) VALUES
  (10,1,'2025-01-01',20),(11,1,'2025-01-02',30),(12,3,'2025-01-03',10);
```
**Expected result:**
| customer_id | customer_name | order_count |
|---:|---|---:|
| 1 | Ari | 2 |
| 2 | Bo | 0 |
| 3 | Cy | 1 |

**Test cases:**
1. **No customers**
   ```sql
   INSERT INTO orders VALUES (1,NULL,'2025-01-01',10);
   ```
   Expected columns: `customer_id, customer_name, order_count`; rows: *(empty)*.
2. **Several customers without orders**
   ```sql
   INSERT INTO customers (id,name) VALUES (1,'Ari'),(2,'Bo');
   ```
   Expected rows: `(1,'Ari',0)`, `(2,'Bo',0)`.
3. **Order with NULL customer is not counted for any customer**
   ```sql
   INSERT INTO customers (id,name) VALUES (1,'Ari');
   INSERT INTO orders VALUES (1,NULL,'2025-01-01',99);
   ```
   Expected rows: `(1,'Ari',0)`.

### Easy 3. Top Five Salaries

Return at most five employees with the highest salary. Return `employee_id`, `employee_name`, and `salary`, ordered by salary descending and employee ID ascending to break ties. If fewer than five employees exist, return all of them.

**Schema:** `employees(id, name, salary)`.

**Example fixture:**
```sql
INSERT INTO employees (id,name,salary) VALUES
  (4,'Dee',90),(2,'Bo',100),(3,'Cy',100),(1,'Ari',80),(5,'Eli',70),(6,'Fox',60);
```
**Expected result:**
| employee_id | employee_name | salary |
|---:|---|---:|
| 2 | Bo | 100 |
| 3 | Cy | 100 |
| 4 | Dee | 90 |
| 1 | Ari | 80 |
| 5 | Eli | 70 |

**Test cases:**
1. **Empty table:** Expected columns `employee_id, employee_name, salary`; rows *(empty)*.
2. **Fewer than five rows**
   ```sql
   INSERT INTO employees (id,name,salary) VALUES (3,'Cy',50),(1,'Ari',50);
   ```
   Expected rows: `(1,'Ari',50)`, `(3,'Cy',50)`.
3. **Ties at cutoff**
   ```sql
   INSERT INTO employees (id,name,salary) VALUES
     (1,'A',100),(2,'B',90),(3,'C',80),(4,'D',70),(5,'E',60),(6,'F',60);
   ```
   Expected rows: `(1,'A',100)`, `(2,'B',90)`, `(3,'C',80)`, `(4,'D',70)`, `(5,'E',60)`.

### Easy 4. Products Above Average Price

Return `product_id`, `product_name`, and `price` for products whose price is strictly greater than the average price of all rows in `products`. Order by product ID ascending. The table contains no NULL prices.

**Schema:** `products(id, name, category, price)`.

**Example fixture:**
```sql
INSERT INTO products VALUES (1,'Pen','Office',2),(2,'Book','Office',8),(3,'Bag','Travel',20);
```
Average price is 10.
**Expected result:**
| product_id | product_name | price |
|---:|---|---:|
| 3 | Bag | 20 |

**Test cases:**
1. **Empty table:** Expected columns `product_id, product_name, price`; rows *(empty)*.
2. **Equal-to-average product is excluded**
   ```sql
   INSERT INTO products VALUES (1,'A','x',2),(2,'B','x',4),(3,'C','x',6);
   ```
   Expected rows: `(3,'C',6)` because the average is 4.
3. **Single product**
   ```sql
   INSERT INTO products VALUES (7,'Only','x',99);
   ```
   Expected rows: *(empty)*.

### Easy 5. Unique Customer Cities

Return each non-NULL city represented by at least one customer exactly once, ordered alphabetically using case-sensitive binary ordering.

**Schema:** `customers(id, name, city)`.

**Example fixture:**
```sql
INSERT INTO customers (id,name,city) VALUES
  (1,'Ari','Oslo'),(2,'Bo','Lima'),(3,'Cy','Oslo'),(4,'Dee',NULL);
```
**Expected result:**
| city |
|---|
| Lima |
| Oslo |

**Test cases:**
1. **No customers:** Expected column `city`; rows *(empty)*.
2. **Only NULL cities**
   ```sql
   INSERT INTO customers (id,name,city) VALUES (1,'A',NULL),(2,'B',NULL);
   ```
   Expected rows: *(empty)*.
3. **Case-sensitive distinct values**
   ```sql
   INSERT INTO customers (id,name,city) VALUES (1,'A','rome'),(2,'B','Rome'),(3,'C','rome');
   ```
   Expected rows: `('Rome')`, `('rome')`.

## Medium

### Medium 1. Second Highest Distinct Salary

Return one row with column `second_highest_salary`: the second greatest distinct salary in `employees`. Return SQL `NULL` if there are fewer than two distinct salaries.

**Schema:** `employees(id, name, salary)`.

**Example fixture:**
```sql
INSERT INTO employees (id,name,salary) VALUES (1,'A',100),(2,'B',100),(3,'C',80);
```
**Expected result:**
| second_highest_salary |
|---:|
| 80 |

**Test cases:**
1. **Empty table:** Expected one row `(NULL)`.
2. **One distinct salary**
   ```sql
   INSERT INTO employees (id,name,salary) VALUES (1,'A',100),(2,'B',100);
   ```
   Expected one row `(NULL)`.
3. **Several distinct salaries**
   ```sql
   INSERT INTO employees (id,name,salary) VALUES (1,'A',10),(2,'B',30),(3,'C',20);
   ```
   Expected one row `(20)`.

### Medium 2. Customers Without Orders

Return `customer_id` and `customer_name` for every customer that has no matching order. Order by customer ID ascending. A NULL `orders.customer_id` does not match any customer.

**Schema:** `customers(id, name)` and `orders(id, customer_id, order_date, amount)`.

**Example fixture:**
```sql
INSERT INTO customers (id,name) VALUES (1,'Ari'),(2,'Bo'),(3,'Cy');
INSERT INTO orders VALUES (10,1,'2025-01-01',20),(11,NULL,'2025-01-02',5);
```
**Expected result:**
| customer_id | customer_name |
|---:|---|
| 2 | Bo |
| 3 | Cy |

**Test cases:**
1. **No customers:** Expected columns `customer_id, customer_name`; rows *(empty)*.
2. **Every customer has an order**
   ```sql
   INSERT INTO customers (id,name) VALUES (1,'A'),(2,'B');
   INSERT INTO orders VALUES (1,1,'2025-01-01',1),(2,2,'2025-01-02',1);
   ```
   Expected rows: *(empty)*.
3. **NULL order customer does not hide unmatched customer**
   ```sql
   INSERT INTO customers (id,name) VALUES (1,'A');
   INSERT INTO orders VALUES (1,NULL,'2025-01-01',1);
   ```
   Expected rows: `(1,'A')`.

### Medium 3. Monthly Revenue for Twelve Calendar Months

Given a reference date, report revenue by calendar month for the 12-month period beginning at the first day of the month 11 months before the reference date's month and ending at the first day of the following month. Include only months with at least one order. Return `month` as `YYYY-MM` and `revenue` as the sum of order amounts. Ignore orders outside the half-open date range. Order chronologically. The reference date is supplied as a parameter and must not be replaced with the database's current date.

**Schema:** `orders(id, order_date, amount)`. Reference date for the example: `2025-03-15`.

**Example fixture:**
```sql
INSERT INTO orders (id,order_date,amount) VALUES
  (1,'2024-04-01',10),(2,'2024-04-30',5),(3,'2025-03-15',7),
  (4,'2024-03-31',100),(5,'2025-04-01',200);
```
**Expected result:**
| month | revenue |
|---|---:|
| 2024-04 | 15 |
| 2025-03 | 7 |

**Test cases (reference date `2025-03-15`):**
1. **Empty orders:** Expected columns `month, revenue`; rows *(empty)*.
2. **Inclusive start and exclusive end**
   ```sql
   INSERT INTO orders (id,order_date,amount) VALUES (1,'2024-04-01',2),(2,'2025-03-31',3),(3,'2025-04-01',9);
   ```
   Expected rows: `('2024-04',2)`, `('2025-03',3)`.
3. **Several orders in one month**
   ```sql
   INSERT INTO orders (id,order_date,amount) VALUES (1,'2025-01-02',4),(2,'2025-01-31',6),(3,'2025-02-01',5);
   ```
   Expected rows: `('2025-01',10)`, `('2025-02',5)`.

### Medium 4. Top Three Products per Category

For each category, calculate each product's total sold revenue as `quantity * product price` across order items. Return up to three products with positive or zero recorded sales for each category, excluding products with no order-item rows. Return `category`, `product_id`, `product_name`, and `revenue`. Rank by revenue descending, breaking ties by product ID ascending. Order final rows by category ascending, then rank ascending.

**Schema:** `products(id, name, category, price)`, `order_items(order_id, product_id, quantity)`.

**Example fixture:**
```sql
INSERT INTO products VALUES (1,'A','Books',10),(2,'B','Books',5),(3,'C','Books',4),(4,'D','Books',3);
INSERT INTO orders VALUES (1,NULL,'2025-01-01',0),(2,NULL,'2025-01-02',0);
INSERT INTO order_items VALUES (1,1,2),(1,2,3),(2,3,4),(2,4,1);
```
**Expected result:**
| category | product_id | product_name | revenue |
|---|---:|---|---:|
| Books | 1 | A | 20 |
| Books | 3 | C | 16 |
| Books | 2 | B | 15 |

**Test cases:**
1. **No sold products:** Expected columns `category, product_id, product_name, revenue`; rows *(empty)*.
2. **Fewer than three sold products**
   ```sql
   INSERT INTO products VALUES (1,'A','x',2),(2,'B','x',3),(3,'C','y',4);
   INSERT INTO orders VALUES (1,NULL,'2025-01-01',0),(2,NULL,'2025-01-02',0);
   INSERT INTO order_items VALUES (1,1,2),(2,2,1);
   ```
   Expected rows: `('x',1,'A',4)`, `('x',2,'B',3)`.
3. **Tie uses product ID**
   ```sql
   INSERT INTO products VALUES (2,'B','x',5),(1,'A','x',10),(3,'C','x',5),(4,'D','x',1);
   INSERT INTO orders VALUES (1,NULL,'2025-01-01',0);
   INSERT INTO order_items VALUES (1,1,1),(1,2,2),(1,3,2),(1,4,10);
   ```
   Expected rows: `('x',1,'A',10)`, `('x',2,'B',10)`, `('x',3,'C',10)`.

### Medium 5. Duplicate Email Addresses

Return each non-NULL email address that occurs in more than one customer row and its `customer_count`. Compare email values case-sensitively. Order by email using binary ordering.

**Schema:** `customers(id, name, email)`.

**Example fixture:**
```sql
INSERT INTO customers (id,name,email) VALUES
  (1,'A','a@example.test'),(2,'B','b@example.test'),
  (3,'C','a@example.test'),(4,'D',NULL);
```
**Expected result:**
| email | customer_count |
|---|---:|
| a@example.test | 2 |

**Test cases:**
1. **No customers:** Expected columns `email, customer_count`; rows *(empty)*.
2. **All emails unique**
   ```sql
   INSERT INTO customers (id,name,email) VALUES (1,'A','a@x'),(2,'B','b@x');
   ```
   Expected rows: *(empty)*.
3. **NULLs are not duplicate email values**
   ```sql
   INSERT INTO customers (id,name,email) VALUES (1,'A',NULL),(2,'B',NULL),(3,'C','x'),(4,'D','x');
   ```
   Expected rows: `('x',2)`.
4. **Comparison is case-sensitive**
   ```sql
   INSERT INTO customers (id,name,email) VALUES (1,'A','A@x'),(2,'B','a@x');
   ```
   Expected rows: *(empty)*.

## Hard

### Hard 1. Rolling Seven-Day Active Users

For each calendar date that appears in `user_activity`, count the distinct users with at least one activity from that date through the six preceding calendar dates, inclusive. Return `activity_date` and `active_users`, ordered by date ascending. A user with multiple activity rows in the window counts once. Dates without any activity rows do not appear in the output.

**Schema:** `user_activity(user_id, activity_date)`; `(user_id, activity_date)` is unique.

**Example fixture:**
```sql
INSERT INTO user_activity VALUES
  (1,'2025-01-01'),(2,'2025-01-02'),(1,'2025-01-04'),
  (1,'2025-01-08'),(2,'2025-01-08');
```
**Expected result:**
| activity_date | active_users |
|---|---:|
| 2025-01-01 | 1 |
| 2025-01-02 | 2 |
| 2025-01-04 | 2 |
| 2025-01-08 | 2 |

**Test cases:**
1. **No activity:** Expected columns `activity_date, active_users`; rows *(empty)*.
2. **Inclusive six-day boundary**
   ```sql
   INSERT INTO user_activity VALUES
     (1,'2025-01-01'),(1,'2025-01-02'),(1,'2025-01-08'),(2,'2025-01-08');
   ```
   Expected rows: `('2025-01-01',1)`, `('2025-01-02',1)`, `('2025-01-08',2)`. The Jan 1 activity is outside the Jan 8 window; Jan 2 is on its inclusive start.
3. **Several events on reporting dates**
   ```sql
   INSERT INTO user_activity VALUES
     (1,'2025-02-01'),(2,'2025-02-01'),(1,'2025-02-03');
   ```
   Expected rows: `('2025-02-01',2)`, `('2025-02-03',2)`.

### Hard 2. Monthly Retention Cohorts

For each user, define their cohort as the calendar month of their first activity. For every cohort and month offset with at least one active user, return `cohort_month` (`YYYY-MM`), `month_number` (zero for cohort month), and the number of distinct users active in that month. Activity in any day of a month counts. Order by cohort month, then month number ascending. Do not generate zero-activity months.

**Schema:** `user_activity(user_id, activity_date)`; `(user_id, activity_date)` is unique.

**Example fixture:**
```sql
INSERT INTO user_activity VALUES
  (1,'2025-01-03'),(1,'2025-02-10'),(1,'2025-03-01'),
  (2,'2025-01-20'),(2,'2025-03-05'),(3,'2025-02-01');
```
**Expected result:**
| cohort_month | month_number | active_users |
|---|---:|---:|
| 2025-01 | 0 | 2 |
| 2025-01 | 1 | 1 |
| 2025-01 | 2 | 2 |
| 2025-02 | 0 | 1 |

**Test cases:**
1. **No activity:** Expected columns `cohort_month, month_number, active_users`; rows *(empty)*.
2. **Same month and later return**
   ```sql
   INSERT INTO user_activity VALUES
     (1,'2024-12-01'),(1,'2024-12-31'),(2,'2024-12-05'),(1,'2025-01-01');
   ```
   Expected rows: `('2024-12',0,2)`, `('2024-12',1,1)`.
3. **Different cohorts, no zero-filled months**
   ```sql
   INSERT INTO user_activity VALUES (1,'2025-01-01'),(2,'2025-03-01'),(2,'2025-05-01');
   ```
   Expected rows: `('2025-01',0,1)`, `('2025-03',0,1)`, `('2025-03',2,1)`.
4. **Multiple dates in one month count a user once**
   ```sql
   INSERT INTO user_activity VALUES (1,'2025-01-01'),(1,'2025-01-20'),(2,'2025-01-15');
   ```
   Expected rows: `('2025-01',0,2)`.

### Hard 3. Median Salary per Department

For each department that has at least one employee, return `department_id` and the median salary. For an odd number of employees, use the middle salary after sorting. For an even number, use the arithmetic mean of the two middle salaries. Return the median as a numeric value and order by department ID ascending. All salaries are non-NULL.

**Schema:** `departments(id, name)` and `employees(id, salary, department_id)`.

**Example fixture:**
```sql
INSERT INTO departments VALUES (1,'A'),(2,'B');
INSERT INTO employees (id,name,salary,department_id) VALUES
  (1,'a',10,1),(2,'b',30,1),(3,'c',20,1),(4,'d',5,2),(5,'e',9,2);
```
**Expected result:**
| department_id | median_salary |
|---:|---:|
| 1 | 20 |
| 2 | 7 |

**Test cases:**
1. **No employees:** Expected columns `department_id, median_salary`; rows *(empty)*.
2. **Even count, fractional median**
   ```sql
   INSERT INTO departments VALUES (1,'A');
   INSERT INTO employees (id,name,salary,department_id) VALUES (1,'a',1,1),(2,'b',2,1),(3,'c',8,1),(4,'d',9,1);
   ```
   Expected rows: `(1,5)`.
3. **One employee in one of two departments**
   ```sql
   INSERT INTO departments VALUES (1,'A'),(2,'B');
   INSERT INTO employees (id,name,salary,department_id) VALUES (1,'a',42,2);
   ```
   Expected rows: `(2,42)`. Departments without employees are omitted.
4. **Even count with fractional average**
   ```sql
   INSERT INTO departments VALUES (1,'A');
   INSERT INTO employees (id,name,salary,department_id) VALUES (1,'a',1,1),(2,'b',2,1);
   ```
   Expected rows: `(1,1.5)`.

### Hard 4. Consecutive Activity Streaks

For each user, find every maximal run of consecutive calendar dates on which that user was active. Return `user_id`, `streak_start`, `streak_end`, and `streak_days`; each date counts once. Order by user ID and streak start ascending.

**Schema:** `user_activity(user_id, activity_date)`; `(user_id, activity_date)` is unique; dates are valid ISO dates.

**Example fixture:**
```sql
INSERT INTO user_activity VALUES
  (1,'2025-01-01'),(1,'2025-01-02'),(1,'2025-01-04'),
  (2,'2025-01-02'),(2,'2025-01-03');
```
**Expected result:**
| user_id | streak_start | streak_end | streak_days |
|---:|---|---|---:|
| 1 | 2025-01-01 | 2025-01-02 | 2 |
| 1 | 2025-01-04 | 2025-01-04 | 1 |
| 2 | 2025-01-02 | 2025-01-03 | 2 |

**Test cases:**
1. **No activity:** Expected columns `user_id, streak_start, streak_end, streak_days`; rows *(empty)*.
2. **One-day streaks with gaps**
   ```sql
   INSERT INTO user_activity VALUES (1,'2025-01-01'),(1,'2025-01-03'),(1,'2025-01-05');
   ```
   Expected rows: `(1,'2025-01-01','2025-01-01',1)`, `(1,'2025-01-03','2025-01-03',1)`, `(1,'2025-01-05','2025-01-05',1)`.
3. **Month/year boundary**
   ```sql
   INSERT INTO user_activity VALUES (7,'2024-12-31'),(7,'2025-01-01'),(7,'2025-01-02');
   ```
   Expected rows: `(7,'2024-12-31','2025-01-02',3)`.
4. **Separate users are not combined**
   ```sql
   INSERT INTO user_activity VALUES (1,'2025-01-01'),(2,'2025-01-02'),(2,'2025-01-03');
   ```
   Expected rows: `(1,'2025-01-01','2025-01-01',1)`, `(2,'2025-01-02','2025-01-03',2)`.

### Hard 5. Employee Org Chart

The employee table represents one acyclic management tree with exactly one CEO (`manager_id IS NULL`), and every employee is reachable from that CEO. Return `employee_id`, `employee_name`, `level`, and `path`. The CEO has level 1; each report's level is one more than its manager's. The path joins names from CEO to employee using ` > `. Names do not contain that separator. Order rows by `path` using binary ordering.

**Schema:** `employees(id, name, manager_id)`.

**Example fixture:**
```sql
INSERT INTO employees (id,name,salary,manager_id) VALUES
  (1,'Ari',100,NULL),(2,'Bo',90,1),(3,'Cy',80,1),(4,'Dee',70,2);
```
**Expected result:**
| employee_id | employee_name | level | path |
|---:|---|---:|---|
| 1 | Ari | 1 | Ari |
| 2 | Bo | 2 | Ari > Bo |
| 4 | Dee | 3 | Ari > Bo > Dee |
| 3 | Cy | 2 | Ari > Cy |

**Test cases:**
1. **CEO only**
   ```sql
   INSERT INTO employees (id,name,salary,manager_id) VALUES (1,'CEO',0,NULL);
   ```
   Expected rows: `(1,'CEO',1,'CEO')`.
2. **Two levels below CEO**
   ```sql
   INSERT INTO employees (id,name,salary,manager_id) VALUES (1,'Root',0,NULL),(2,'Zed',0,1),(3,'Amy',0,1);
   ```
   Expected rows: `(1,'Root',1,'Root')`, `(3,'Amy',2,'Root > Amy')`, `(2,'Zed',2,'Root > Zed')`.
3. **Deeper chain**
   ```sql
   INSERT INTO employees (id,name,salary,manager_id) VALUES (10,'A',0,NULL),(20,'B',0,10),(30,'C',0,20),(40,'D',0,30);
   ```
   Expected rows: `(10,'A',1,'A')`, `(20,'B',2,'A > B')`, `(30,'C',3,'A > B > C')`, `(40,'D',4,'A > B > C > D')`.
