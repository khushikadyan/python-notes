# SQL INTERVIEW PREPARATION — COMPLETE GUIDE
## Part 2: Advanced SQL — Subqueries, CTEs, Window Functions, Views

---

# SECTION 6: SUBQUERIES

---

## 6.1 Types of Subqueries

### Scalar Subquery — Returns single value
```sql
-- Employee earning more than company average
SELECT emp_name, salary
FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);
```

### Row Subquery — Returns single row
```sql
SELECT * FROM employees
WHERE (dept_id, salary) = (
    SELECT dept_id, MAX(salary) 
    FROM employees 
    GROUP BY dept_id
    HAVING dept_id = 10
);
```

### Table Subquery (Derived Table) — Returns multiple rows/columns
```sql
-- Must be aliased
SELECT d.dept_name, s.avg_salary
FROM departments d
INNER JOIN (
    SELECT dept_id, AVG(salary) AS avg_salary
    FROM employees
    GROUP BY dept_id
) s ON d.dept_id = s.dept_id;
```

### Subquery positions:
```sql
-- In SELECT (scalar)
SELECT emp_name,
       salary,
       (SELECT AVG(salary) FROM employees) AS company_avg,
       salary - (SELECT AVG(salary) FROM employees) AS diff_from_avg
FROM employees;

-- In FROM (derived table)
SELECT * FROM (SELECT emp_name, salary FROM employees WHERE salary > 80000) AS high_earners;

-- In WHERE
SELECT * FROM employees WHERE dept_id IN (SELECT dept_id FROM departments WHERE location = 'NYC');

-- In HAVING
SELECT dept_id, AVG(salary)
FROM employees
GROUP BY dept_id
HAVING AVG(salary) > (SELECT AVG(salary) FROM employees);
```

---

## 6.2 Correlated Subquery

**Concept:** A subquery that references a column from the outer query. Executes once per row of the outer query — can be slow on large datasets.

```sql
-- Find employees earning more than the average of their OWN department
SELECT e1.emp_name, e1.salary, e1.dept_id
FROM employees e1
WHERE e1.salary > (
    SELECT AVG(e2.salary)
    FROM employees e2
    WHERE e2.dept_id = e1.dept_id  -- references outer query
);
```

**How it works:**
1. Outer query fetches Alice (dept 10)
2. Subquery computes AVG(salary) for dept 10
3. Compares Alice's salary to dept 10 average
4. Repeat for every employee

**Performance:** O(n) subquery executions. For large tables, rewrite using window functions:
```sql
-- Equivalent using window function (much faster)
SELECT emp_name, salary, dept_id
FROM (
    SELECT emp_name, salary, dept_id,
           AVG(salary) OVER (PARTITION BY dept_id) AS dept_avg
    FROM employees
) t
WHERE salary > dept_avg;
```

### EXISTS vs IN (Correlated vs Non-Correlated)
```sql
-- EXISTS: stops scanning as soon as match found (efficient)
SELECT c.customer_id, c.customer_name
FROM customers c
WHERE EXISTS (
    SELECT 1 FROM orders o WHERE o.customer_id = c.customer_id
);

-- NOT EXISTS: find customers with no orders
SELECT c.customer_id, c.customer_name
FROM customers c
WHERE NOT EXISTS (
    SELECT 1 FROM orders o WHERE o.customer_id = c.customer_id
);

-- IN: evaluates all rows (less efficient for large sets)
SELECT customer_id, customer_name
FROM customers
WHERE customer_id IN (SELECT customer_id FROM orders);
```

**Interview Q: When to use EXISTS vs IN?**  
A: Use EXISTS when:
- The subquery can return NULLs (NOT IN with NULLs returns 0 rows)
- The subquery result set is large (EXISTS short-circuits on first match)
- You only need to check existence, not the actual values

Use IN when:
- The subquery returns a small, known set
- You need to match against a list of values

---

# SECTION 7: CTEs (Common Table Expressions)

---

## 7.1 Basic CTE

```sql
-- Syntax: WITH cte_name AS (query) SELECT...
WITH dept_averages AS (
    SELECT dept_id, AVG(salary) AS avg_salary
    FROM employees
    GROUP BY dept_id
)
SELECT e.emp_name, e.salary, da.avg_salary,
       e.salary - da.avg_salary AS diff
FROM employees e
JOIN dept_averages da ON e.dept_id = da.dept_id
WHERE e.salary > da.avg_salary;
```

**Benefits over subqueries:**
- More readable (named, reusable within query)
- Can reference CTE multiple times
- Enables recursion
- Easier to debug (can test CTE independently)

```sql
-- Multiple CTEs
WITH
active_employees AS (
    SELECT * FROM employees WHERE status = 'Active'
),
dept_summary AS (
    SELECT dept_id, COUNT(*) AS headcount, AVG(salary) AS avg_sal
    FROM active_employees
    GROUP BY dept_id
),
top_departments AS (
    SELECT dept_id FROM dept_summary WHERE headcount >= 5
)
SELECT e.emp_name, d.dept_name, ds.avg_sal
FROM active_employees e
JOIN departments d ON e.dept_id = d.dept_id
JOIN dept_summary ds ON e.dept_id = ds.dept_id
WHERE e.dept_id IN (SELECT dept_id FROM top_departments);
```

---

## 7.2 Recursive CTE

**Use cases:** Hierarchies (org charts, categories, bill of materials), date sequences, graph traversal.

### Org Chart / Employee Hierarchy
```sql
-- Employees table with manager_id
WITH RECURSIVE emp_hierarchy AS (
    -- Anchor: start with CEO (no manager)
    SELECT emp_id, emp_name, manager_id, 0 AS level,
           CAST(emp_name AS VARCHAR(500)) AS path
    FROM employees
    WHERE manager_id IS NULL
    
    UNION ALL
    
    -- Recursive: join each employee to their manager
    SELECT e.emp_id, e.emp_name, e.manager_id, 
           h.level + 1,
           CAST(h.path + ' > ' + e.emp_name AS VARCHAR(500))
    FROM employees e
    INNER JOIN emp_hierarchy h ON e.manager_id = h.emp_id
)
SELECT emp_id, emp_name, level, path
FROM emp_hierarchy
ORDER BY path;
```

**Output:**
| emp_id | emp_name | level | path |
|---|---|---|---|
| 1 | CEO_John | 0 | CEO_John |
| 2 | VP_Alice | 1 | CEO_John > VP_Alice |
| 5 | Mgr_Dave | 2 | CEO_John > VP_Alice > Mgr_Dave |
| 8 | Eng_Carol | 3 | CEO_John > VP_Alice > Mgr_Dave > Eng_Carol |

### Generate Date Sequence
```sql
WITH RECURSIVE dates AS (
    SELECT CAST('2024-01-01' AS DATE) AS d
    UNION ALL
    SELECT DATEADD(DAY, 1, d) FROM dates WHERE d < '2024-12-31'
)
SELECT d AS calendar_date, DATENAME(WEEKDAY, d) AS day_name
FROM dates
OPTION (MAXRECURSION 366);  -- SQL Server recursion limit
```

**Interview Q: What is the structure of a recursive CTE?**  
A: A recursive CTE has two parts connected by UNION ALL:
1. **Anchor member:** The base case — a non-recursive SELECT that returns the starting rows
2. **Recursive member:** References the CTE itself, joins to extend the result by one level per iteration

The recursion stops when the recursive member returns no more rows or when MAXRECURSION is reached.

---

# SECTION 8: TEMPORARY TABLES AND VARIABLES

---

## 8.1 Temporary Tables

```sql
-- Local temp table (visible only to current session)
CREATE TABLE #temp_sales (
    region     VARCHAR(50),
    total_sales DECIMAL(12,2)
);

INSERT INTO #temp_sales
SELECT region, SUM(sale_amount) FROM sales GROUP BY region;

SELECT * FROM #temp_sales ORDER BY total_sales DESC;

DROP TABLE #temp_sales;  -- explicitly drop or auto-drops when session ends

-- Global temp table (visible to all sessions)
CREATE TABLE ##global_temp AS
SELECT * FROM employees WHERE status = 'Active';
```

## 8.2 Table Variables

```sql
DECLARE @emp_table TABLE (
    emp_id   INT,
    emp_name VARCHAR(100),
    salary   DECIMAL(10,2)
);

INSERT INTO @emp_table
SELECT emp_id, emp_name, salary FROM employees WHERE dept_id = 10;

SELECT * FROM @emp_table;
-- Automatically cleaned up when batch/procedure ends
```

| Feature | #Temp Table | @Table Variable |
|---|---|---|
| Scope | Session | Batch/Procedure |
| Statistics | Yes | No |
| Transactions | Participates | Partially |
| Indexes | Allowed | Limited |
| Performance (large) | Better | Worse |
| Performance (small) | Comparable | Better |

---

# SECTION 9: VIEWS

---

## 9.1 Regular Views

```sql
-- Create view
CREATE VIEW vw_employee_details AS
SELECT e.emp_id, e.emp_name, e.salary, d.dept_name, 
       m.emp_name AS manager_name
FROM employees e
LEFT JOIN departments d ON e.dept_id = d.dept_id
LEFT JOIN employees m ON e.manager_id = m.emp_id
WHERE e.status = 'Active';

-- Use view like a table
SELECT dept_name, COUNT(*) AS headcount, AVG(salary) AS avg_sal
FROM vw_employee_details
GROUP BY dept_name;

-- Modify view
ALTER VIEW vw_employee_details AS
SELECT e.emp_id, e.emp_name, e.salary, e.hire_date, d.dept_name
FROM employees e
LEFT JOIN departments d ON e.dept_id = d.dept_id;

-- Drop view
DROP VIEW vw_employee_details;
```

**Benefits:**
- Security (hide sensitive columns like salary from certain users)
- Simplify complex queries
- Logical data independence (tables can change, view abstracts it)

**Limitations:**
- Views don't store data (re-executed every time)
- Can be slow for complex views with large underlying tables

## 9.2 Updatable Views

```sql
-- A view is updatable if it maps directly to one table with no:
-- DISTINCT, GROUP BY, HAVING, UNION, aggregate functions, subqueries in SELECT

CREATE VIEW vw_active_employees AS
SELECT emp_id, emp_name, salary, dept_id
FROM employees WHERE status = 'Active';

-- This UPDATE goes through to base table
UPDATE vw_active_employees SET salary = 80000 WHERE emp_id = 101;

-- WITH CHECK OPTION: prevents updates that would make row invisible through view
CREATE VIEW vw_active_employees AS
SELECT emp_id, emp_name, status FROM employees WHERE status = 'Active'
WITH CHECK OPTION;

-- This would fail because updated row would no longer be visible in view
UPDATE vw_active_employees SET status = 'Inactive' WHERE emp_id = 101;  -- ERROR
```

## 9.3 Materialized Views (Indexed Views)

```sql
-- SQL Server: Indexed View (Materialized View equivalent)
CREATE VIEW vw_dept_salary_summary
WITH SCHEMABINDING AS
SELECT dept_id, COUNT_BIG(*) AS emp_count, SUM(salary) AS total_salary
FROM dbo.employees
GROUP BY dept_id;

-- Create clustered index to materialize
CREATE UNIQUE CLUSTERED INDEX idx_dept_salary 
ON vw_dept_salary_summary(dept_id);

-- PostgreSQL: Materialized View
CREATE MATERIALIZED VIEW mv_dept_summary AS
SELECT d.dept_name, COUNT(e.emp_id) AS headcount, AVG(e.salary) AS avg_salary
FROM employees e JOIN departments d ON e.dept_id = d.dept_id
GROUP BY d.dept_name;

-- Refresh materialized view
REFRESH MATERIALIZED VIEW mv_dept_summary;
REFRESH MATERIALIZED VIEW CONCURRENTLY mv_dept_summary;  -- no lock
```

**Regular View vs Materialized View:**
| Aspect | Regular View | Materialized View |
|---|---|---|
| Storage | No data stored | Data physically stored |
| Performance | Executed at query time | Pre-computed, fast reads |
| Freshness | Always current | Stale until refreshed |
| Use case | Simple query encapsulation | Heavy aggregations, reporting |

**Interview Q: When would you use a Materialized View?**  
A: Use materialized views for expensive aggregation queries that are run frequently but don't need real-time data. Examples: daily sales summaries, monthly revenue reports, dashboard KPIs. The trade-off is disk space and refresh latency — you're trading write overhead for read performance.

---

# SECTION 10: STORED PROCEDURES & FUNCTIONS

---

## 10.1 Stored Procedures

```sql
-- Basic stored procedure
CREATE PROCEDURE usp_GetEmployeesByDept
    @dept_id    INT,
    @min_salary DECIMAL(10,2) = 0  -- optional parameter with default
AS
BEGIN
    SET NOCOUNT ON;
    
    SELECT emp_id, emp_name, salary
    FROM employees
    WHERE dept_id = @dept_id
      AND salary >= @min_salary
    ORDER BY salary DESC;
END;

-- Execute
EXEC usp_GetEmployeesByDept @dept_id = 10;
EXEC usp_GetEmployeesByDept @dept_id = 10, @min_salary = 70000;

-- Procedure with OUTPUT parameter
CREATE PROCEDURE usp_GetDeptAvgSalary
    @dept_id    INT,
    @avg_salary DECIMAL(10,2) OUTPUT
AS
BEGIN
    SELECT @avg_salary = AVG(salary)
    FROM employees
    WHERE dept_id = @dept_id;
END;

-- Execute with OUTPUT
DECLARE @result DECIMAL(10,2);
EXEC usp_GetDeptAvgSalary @dept_id = 10, @avg_salary = @result OUTPUT;
SELECT @result AS dept_avg_salary;

-- Procedure with error handling
CREATE PROCEDURE usp_TransferEmployee
    @emp_id     INT,
    @new_dept   INT
AS
BEGIN
    BEGIN TRANSACTION;
    BEGIN TRY
        UPDATE employees SET dept_id = @new_dept WHERE emp_id = @emp_id;
        INSERT INTO employee_transfers VALUES (@emp_id, @new_dept, GETDATE());
        COMMIT TRANSACTION;
    END TRY
    BEGIN CATCH
        ROLLBACK TRANSACTION;
        THROW;
    END CATCH
END;
```

## 10.2 User-Defined Functions (UDFs)

```sql
-- Scalar function: returns single value
CREATE FUNCTION fn_GetFullName(@first VARCHAR(50), @last VARCHAR(50))
RETURNS VARCHAR(101)
AS
BEGIN
    RETURN @first + ' ' + @last;
END;

-- Usage
SELECT dbo.fn_GetFullName(first_name, last_name) AS full_name FROM employees;

-- Table-valued function (inline): returns table
CREATE FUNCTION fn_GetDeptEmployees(@dept_id INT)
RETURNS TABLE
AS
RETURN (
    SELECT emp_id, emp_name, salary
    FROM employees
    WHERE dept_id = @dept_id
);

-- Usage (like a table)
SELECT * FROM dbo.fn_GetDeptEmployees(10);
SELECT e.* FROM dbo.fn_GetDeptEmployees(10) e WHERE e.salary > 70000;
```

**Stored Procedure vs Function:**
| Aspect | Stored Procedure | Function |
|---|---|---|
| Return value | Optional (OUTPUT params) | Must return a value |
| Transaction control | Can use COMMIT/ROLLBACK | Cannot |
| Error handling | TRY/CATCH supported | Limited |
| Usage in SELECT | Cannot use in SELECT | Can use in SELECT |
| DML operations | Can INSERT/UPDATE/DELETE | Scalar cannot; TVF cannot modify |
| Purpose | Business logic, batch ops | Computation, reuse in queries |

---

# SECTION 11: TRIGGERS

---

```sql
-- AFTER INSERT trigger: log new employee
CREATE TRIGGER trg_AfterEmployeeInsert
ON employees
AFTER INSERT
AS
BEGIN
    INSERT INTO employee_audit (action, emp_id, emp_name, action_time, action_by)
    SELECT 'INSERT', emp_id, emp_name, GETDATE(), SYSTEM_USER
    FROM INSERTED;  -- INSERTED virtual table contains new rows
END;

-- AFTER UPDATE trigger: track salary changes
CREATE TRIGGER trg_SalaryChange
ON employees
AFTER UPDATE
AS
BEGIN
    IF UPDATE(salary)  -- only fires when salary column changes
    BEGIN
        INSERT INTO salary_audit (emp_id, old_salary, new_salary, changed_on)
        SELECT d.emp_id, d.salary, i.salary, GETDATE()
        FROM DELETED d JOIN INSERTED i ON d.emp_id = i.emp_id
        WHERE d.salary <> i.salary;
    END
END;

-- INSTEAD OF trigger: redirect operation (common on views)
CREATE TRIGGER trg_InsteadOfDelete
ON employees
INSTEAD OF DELETE
AS
BEGIN
    UPDATE employees SET status = 'Inactive'
    WHERE emp_id IN (SELECT emp_id FROM DELETED);
    -- Soft delete instead of hard delete
END;
```

**INSERTED and DELETED virtual tables:**
- `INSERTED`: Contains new rows (after INSERT or UPDATE)
- `DELETED`: Contains old rows (before DELETE or UPDATE)

**Interview Q: What are INSERTED and DELETED tables in triggers?**  
A: These are virtual tables automatically available inside trigger bodies. INSERTED holds the new row values (for INSERT and UPDATE triggers). DELETED holds the old row values (for DELETE and UPDATE triggers). For UPDATE triggers, you can join INSERTED and DELETED to compare old vs new values.

---

# SECTION 12: WINDOW FUNCTIONS — COMPLETE REFERENCE

---

## 12.1 Window Function Anatomy

```sql
function_name() OVER (
    PARTITION BY column(s)   -- divides rows into groups (optional)
    ORDER BY column(s)       -- defines row order within partition
    ROWS/RANGE BETWEEN       -- defines the window frame (optional)
        UNBOUNDED PRECEDING
        AND CURRENT ROW
)
```

**Key concept:** Window functions do NOT collapse rows (unlike GROUP BY). Every row gets a result based on a "window" of related rows.

---

## 12.2 ROW_NUMBER, RANK, DENSE_RANK

### Setup
```sql
-- Sample data
emp_name | dept | salary
---------|------|-------
Alice    | IT   | 90000
Bob      | IT   | 85000
Carol    | IT   | 85000    -- tie with Bob
Dave     | IT   | 75000
Eve      | HR   | 70000
Frank    | HR   | 70000    -- tie with Eve
Grace    | HR   | 65000
```

```sql
SELECT emp_name, dept, salary,
    ROW_NUMBER() OVER (PARTITION BY dept ORDER BY salary DESC) AS row_num,
    RANK()       OVER (PARTITION BY dept ORDER BY salary DESC) AS rnk,
    DENSE_RANK() OVER (PARTITION BY dept ORDER BY salary DESC) AS dense_rnk
FROM employees;
```

**Output:**
| emp_name | dept | salary | row_num | rnk | dense_rnk |
|---|---|---|---|---|---|
| Alice | IT | 90000 | 1 | 1 | 1 |
| Bob | IT | 85000 | 2 | 2 | 2 |
| Carol | IT | 85000 | 3 | 2 | 2 |
| Dave | IT | 75000 | 4 | 4 | 3 |
| Eve | HR | 70000 | 1 | 1 | 1 |
| Frank | HR | 70000 | 2 | 1 | 1 |
| Grace | HR | 65000 | 3 | 3 | 2 |

**Differences:**
- `ROW_NUMBER()`: Always unique sequential numbers; ties broken arbitrarily
- `RANK()`: Ties get same rank; next rank skips (1,2,2,4)
- `DENSE_RANK()`: Ties get same rank; no gaps (1,2,2,3)

**Classic Interview Pattern — Top N per group:**
```sql
-- Top 3 earners per department
WITH ranked AS (
    SELECT emp_name, dept_id, salary,
           DENSE_RANK() OVER (PARTITION BY dept_id ORDER BY salary DESC) AS dr
    FROM employees
)
SELECT emp_name, dept_id, salary
FROM ranked
WHERE dr <= 3;
```

**Interview Q: Difference between ROW_NUMBER, RANK, and DENSE_RANK?**  
A: All three assign numbers to rows within a partition ordered by a column. They differ in handling ties:
- ROW_NUMBER always gives unique sequential numbers (arbitrary tiebreak)
- RANK gives the same number to ties but skips the next number (1,1,3)
- DENSE_RANK gives the same number to ties without skipping (1,1,2)

For "Top N" queries, DENSE_RANK is usually preferred because it includes all tied rows at position N.

---

## 12.3 NTILE

```sql
-- Divide employees into 4 salary quartiles per department
SELECT emp_name, dept_id, salary,
       NTILE(4) OVER (PARTITION BY dept_id ORDER BY salary DESC) AS quartile
FROM employees;

-- NTILE(100) = percentile rank
SELECT emp_name, salary,
       NTILE(100) OVER (ORDER BY salary) AS percentile
FROM employees;
```

---

## 12.4 LEAD and LAG

```sql
-- LAG: access previous row's value
-- LEAD: access next row's value

SELECT 
    emp_name,
    order_date,
    order_amount,
    LAG(order_amount, 1, 0)  OVER (PARTITION BY customer_id ORDER BY order_date) AS prev_order,
    LEAD(order_amount, 1, 0) OVER (PARTITION BY customer_id ORDER BY order_date) AS next_order,
    order_amount - LAG(order_amount, 1, 0) OVER (PARTITION BY customer_id ORDER BY order_date) AS mom_change
FROM orders;
```

**Real-world use:** Month-over-month growth, previous order comparison, time-series analysis.

```sql
-- Find employees whose salary increased compared to previous year
WITH yearly_salary AS (
    SELECT emp_id, year, salary,
           LAG(salary) OVER (PARTITION BY emp_id ORDER BY year) AS prev_year_salary
    FROM salary_history
)
SELECT emp_id, year, salary, prev_year_salary,
       ROUND((salary - prev_year_salary) * 100.0 / prev_year_salary, 2) AS pct_increase
FROM yearly_salary
WHERE salary > prev_year_salary;
```

---

## 12.5 FIRST_VALUE and LAST_VALUE

```sql
SELECT 
    emp_name, dept_id, salary,
    FIRST_VALUE(emp_name) OVER (PARTITION BY dept_id ORDER BY salary DESC) AS top_earner,
    LAST_VALUE(emp_name)  OVER (
        PARTITION BY dept_id ORDER BY salary DESC
        ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING  -- IMPORTANT!
    ) AS lowest_earner
FROM employees;
```

**Important:** LAST_VALUE requires explicit ROWS frame — default frame is `ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW`, which makes LAST_VALUE return the current row's value.

---

## 12.6 Running Totals and Moving Averages

```sql
-- Running total
SELECT order_date, order_amount,
       SUM(order_amount) OVER (ORDER BY order_date 
           ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS running_total
FROM orders;

-- 3-day moving average
SELECT order_date, order_amount,
       AVG(order_amount) OVER (ORDER BY order_date 
           ROWS BETWEEN 2 PRECEDING AND CURRENT ROW) AS moving_avg_3d
FROM orders;

-- Cumulative count
SELECT customer_id, order_date,
       COUNT(*) OVER (PARTITION BY customer_id ORDER BY order_date 
           ROWS UNBOUNDED PRECEDING) AS cumulative_orders
FROM orders;

-- Percentage of total
SELECT dept_id, emp_name, salary,
       ROUND(salary * 100.0 / SUM(salary) OVER (PARTITION BY dept_id), 2) AS pct_of_dept
FROM employees;
```

---

## 12.7 Window Frame: ROWS vs RANGE

```sql
-- ROWS: physical rows relative to current row
-- RANGE: logical range based on ORDER BY value (handles duplicates differently)

-- ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING
-- Includes: previous row, current row, next row (exactly 3 rows if they exist)

-- RANGE BETWEEN 1 PRECEDING AND 1 FOLLOWING
-- Includes: all rows where ORDER BY value is within 1 of current row's value
-- For dates: ±1 day; for integers: ±1

SELECT order_date, order_amount,
    SUM(order_amount) OVER (
        ORDER BY order_date 
        ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
    ) AS running_total_rows,
    SUM(order_amount) OVER (
        ORDER BY order_date 
        RANGE BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
    ) AS running_total_range
FROM daily_sales;
```

---

# SECTION 13: SQL QUERY CHALLENGES — CLASSICS

---

## 13.1 Second Highest Salary

```sql
-- Method 1: LIMIT/OFFSET
SELECT DISTINCT salary FROM employees ORDER BY salary DESC LIMIT 1 OFFSET 1;

-- Method 2: Subquery
SELECT MAX(salary) FROM employees
WHERE salary < (SELECT MAX(salary) FROM employees);

-- Method 3: DENSE_RANK (handles NULLs and ties best)
SELECT salary FROM (
    SELECT salary, DENSE_RANK() OVER (ORDER BY salary DESC) AS dr
    FROM employees
) t WHERE dr = 2;

-- Method 4: NOT IN
SELECT MAX(salary) FROM employees
WHERE salary NOT IN (SELECT MAX(salary) FROM employees);
```

## 13.2 Nth Highest Salary

```sql
-- Dynamic N-th highest (pass N as variable)
-- Method 1: Dense_Rank (recommended)
CREATE FUNCTION getNthHighestSalary(@N INT)
RETURNS TABLE
AS RETURN (
    SELECT salary FROM (
        SELECT salary, DENSE_RANK() OVER (ORDER BY salary DESC) AS dr
        FROM employees
    ) t WHERE dr = @N
);

-- Method 2: Correlated subquery
SELECT salary FROM employees e1
WHERE @N - 1 = (
    SELECT COUNT(DISTINCT salary) FROM employees e2
    WHERE e2.salary > e1.salary
);

-- Method 3: LIMIT OFFSET (MySQL)
SELECT DISTINCT salary FROM employees ORDER BY salary DESC LIMIT 1 OFFSET N-1;
```

## 13.3 Find Duplicate Records

```sql
-- Find duplicate emails
SELECT email, COUNT(*) AS cnt
FROM employees
GROUP BY email
HAVING COUNT(*) > 1;

-- Find ALL rows that are duplicates (with their data)
SELECT * FROM employees
WHERE email IN (
    SELECT email FROM employees
    GROUP BY email
    HAVING COUNT(*) > 1
)
ORDER BY email;

-- Find duplicates using window function
SELECT emp_id, emp_name, email,
       COUNT(*) OVER (PARTITION BY email) AS duplicate_count
FROM employees
HAVING duplicate_count > 1;  -- Not standard, use subquery instead

-- Better approach with ROW_NUMBER
SELECT * FROM (
    SELECT *, 
           ROW_NUMBER() OVER (PARTITION BY email ORDER BY emp_id) AS rn
    FROM employees
) t WHERE rn > 1;  -- These are the duplicates
```

## 13.4 Delete Duplicate Records (Keep One)

```sql
-- SQL Server / PostgreSQL: Delete using CTE + ROW_NUMBER
WITH CTE AS (
    SELECT emp_id,
           ROW_NUMBER() OVER (PARTITION BY email ORDER BY emp_id) AS rn
    FROM employees
)
DELETE FROM CTE WHERE rn > 1;  -- keeps row with lowest emp_id

-- MySQL: Cannot use CTE for DELETE directly
DELETE FROM employees
WHERE emp_id NOT IN (
    SELECT min_id FROM (
        SELECT MIN(emp_id) AS min_id 
        FROM employees 
        GROUP BY email
    ) t
);

-- Alternative: SELF JOIN delete
DELETE e1 FROM employees e1
INNER JOIN employees e2 ON e1.email = e2.email AND e1.emp_id > e2.emp_id;
```

## 13.5 Running Totals

```sql
-- Running total of daily sales
SELECT 
    order_date,
    daily_sales,
    SUM(daily_sales) OVER (ORDER BY order_date ROWS UNBOUNDED PRECEDING) AS running_total
FROM (
    SELECT order_date, SUM(sale_amount) AS daily_sales
    FROM orders
    GROUP BY order_date
) daily;

-- Running total partitioned by product
SELECT product_id, sale_date, amount,
       SUM(amount) OVER (PARTITION BY product_id ORDER BY sale_date) AS running_total
FROM product_sales;
```

## 13.6 Consecutive Records / Gaps and Islands

```sql
-- Classic GAPS AND ISLANDS problem
-- Find consecutive date ranges for each user's login activity

-- Island detection using difference of row numbers
WITH numbered AS (
    SELECT user_id, login_date,
           ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY login_date) AS rn,
           DATEADD(DAY, -ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY login_date), login_date) AS grp
    FROM user_logins
)
SELECT user_id, 
       MIN(login_date) AS start_date,
       MAX(login_date) AS end_date,
       COUNT(*) AS consecutive_days
FROM numbered
GROUP BY user_id, grp
ORDER BY user_id, start_date;

-- Find gaps: dates where users did NOT login
WITH date_series AS (
    SELECT CAST('2024-01-01' AS DATE) AS d
    UNION ALL
    SELECT DATEADD(DAY, 1, d) FROM date_series WHERE d < '2024-12-31'
),
user_dates AS (
    SELECT DISTINCT user_id FROM user_logins
    CROSS JOIN date_series
)
SELECT ud.user_id, ud.d AS missing_date
FROM user_dates ud
LEFT JOIN user_logins ul ON ud.user_id = ul.user_id AND ud.d = ul.login_date
WHERE ul.login_date IS NULL
OPTION (MAXRECURSION 366);
```

## 13.7 Top N Per Group

```sql
-- Top 2 products by sales per category
WITH ranked AS (
    SELECT category_id, product_name, total_sales,
           ROW_NUMBER() OVER (PARTITION BY category_id ORDER BY total_sales DESC) AS rn
    FROM product_sales
)
SELECT category_id, product_name, total_sales
FROM ranked
WHERE rn <= 2;

-- If ties should all be included (use DENSE_RANK)
WITH ranked AS (
    SELECT category_id, product_name, total_sales,
           DENSE_RANK() OVER (PARTITION BY category_id ORDER BY total_sales DESC) AS dr
    FROM product_sales
)
SELECT category_id, product_name, total_sales
FROM ranked
WHERE dr <= 2;
```

## 13.8 Latest Record Per Customer

```sql
-- Method 1: ROW_NUMBER (most efficient)
SELECT customer_id, order_id, order_date, amount
FROM (
    SELECT *,
           ROW_NUMBER() OVER (PARTITION BY customer_id ORDER BY order_date DESC) AS rn
    FROM orders
) t WHERE rn = 1;

-- Method 2: Correlated subquery
SELECT o1.*
FROM orders o1
WHERE order_date = (
    SELECT MAX(order_date) FROM orders o2 WHERE o2.customer_id = o1.customer_id
);

-- Method 3: JOIN to subquery
SELECT o.* 
FROM orders o
INNER JOIN (
    SELECT customer_id, MAX(order_date) AS last_order
    FROM orders GROUP BY customer_id
) latest ON o.customer_id = latest.customer_id AND o.order_date = latest.last_order;
```

## 13.9 Department-Wise Highest Salary

```sql
-- Method 1: DENSE_RANK
SELECT dept_id, emp_name, salary
FROM (
    SELECT dept_id, emp_name, salary,
           DENSE_RANK() OVER (PARTITION BY dept_id ORDER BY salary DESC) AS dr
    FROM employees
) t WHERE dr = 1;

-- Method 2: Correlated subquery
SELECT dept_id, emp_name, salary
FROM employees e1
WHERE salary = (
    SELECT MAX(salary) FROM employees e2 WHERE e2.dept_id = e1.dept_id
);

-- Method 3: JOIN with aggregation
SELECT e.dept_id, e.emp_name, e.salary
FROM employees e
INNER JOIN (
    SELECT dept_id, MAX(salary) AS max_sal FROM employees GROUP BY dept_id
) m ON e.dept_id = m.dept_id AND e.salary = m.max_sal;
```

## 13.10 Customers With No Orders

```sql
-- Method 1: LEFT JOIN + IS NULL (most common)
SELECT c.customer_id, c.customer_name
FROM customers c
LEFT JOIN orders o ON c.customer_id = o.customer_id
WHERE o.order_id IS NULL;

-- Method 2: NOT EXISTS (safest with NULLs)
SELECT customer_id, customer_name
FROM customers c
WHERE NOT EXISTS (
    SELECT 1 FROM orders o WHERE o.customer_id = c.customer_id
);

-- Method 3: NOT IN (careful with NULLs!)
SELECT customer_id, customer_name
FROM customers
WHERE customer_id NOT IN (
    SELECT customer_id FROM orders WHERE customer_id IS NOT NULL
);
```

## 13.11 Year-over-Year / Month-over-Month Analysis

```sql
-- Monthly revenue with MoM change
WITH monthly AS (
    SELECT 
        FORMAT(order_date, 'yyyy-MM') AS month,
        SUM(amount) AS revenue
    FROM orders
    GROUP BY FORMAT(order_date, 'yyyy-MM')
)
SELECT 
    month,
    revenue,
    LAG(revenue) OVER (ORDER BY month) AS prev_month_revenue,
    ROUND((revenue - LAG(revenue) OVER (ORDER BY month)) * 100.0 
          / LAG(revenue) OVER (ORDER BY month), 2) AS mom_pct_change
FROM monthly;

-- Year-over-Year using CASE + aggregate
SELECT 
    product_id,
    SUM(CASE WHEN YEAR(order_date) = 2023 THEN amount ELSE 0 END) AS revenue_2023,
    SUM(CASE WHEN YEAR(order_date) = 2024 THEN amount ELSE 0 END) AS revenue_2024,
    SUM(CASE WHEN YEAR(order_date) = 2024 THEN amount ELSE 0 END) -
    SUM(CASE WHEN YEAR(order_date) = 2023 THEN amount ELSE 0 END) AS yoy_change
FROM orders
GROUP BY product_id;
```

## 13.12 Cumulative Distribution & Percentiles

```sql
SELECT emp_name, salary,
       CUME_DIST()    OVER (ORDER BY salary) AS cumulative_dist,   -- 0 to 1
       PERCENT_RANK() OVER (ORDER BY salary) AS percent_rank,      -- 0 to 1
       NTILE(100)     OVER (ORDER BY salary) AS percentile         -- 1 to 100
FROM employees;
```

---

# SECTION 14: INDEXING — DEEP DIVE

---

## 14.1 Clustered Index

**Concept:** Physically reorders the data rows in the table based on the index key. There can be only ONE clustered index per table (because the data can only be sorted in one order).

```
Table without clustered index (HEAP):
Row 1: Dave  | 30 | 60000
Row 2: Alice | 10 | 90000
Row 3: Carol | 10 | 85000
Row 4: Bob   | 20 | 75000

After creating clustered index on emp_id:
Row 1: emp_id=1 | Alice | 10 | 90000
Row 2: emp_id=2 | Bob   | 20 | 75000
Row 3: emp_id=3 | Carol | 10 | 85000
Row 4: emp_id=4 | Dave  | 30 | 60000
```

```sql
-- PRIMARY KEY creates a clustered index by default (SQL Server)
CREATE TABLE employees (
    emp_id INT PRIMARY KEY,  -- clustered index automatically created
    ...
);

-- Explicit clustered index
CREATE CLUSTERED INDEX idx_emp_id ON employees(emp_id);

-- Range scans are fast with clustered index
SELECT * FROM employees WHERE emp_id BETWEEN 100 AND 200;  -- reads contiguous pages
```

**Internal Structure:** B-Tree where leaf nodes ARE the actual data pages.

## 14.2 Non-Clustered Index

**Concept:** Creates a separate B-Tree structure with pointers (row locators) back to the actual data rows. Can have multiple non-clustered indexes per table.

```
Non-clustered index on salary:
Index Leaf nodes:          Pointer to heap/clustered
60000 | emp_id=4          ------> actual row
75000 | emp_id=2          ------> actual row
85000 | emp_id=3          ------> actual row
90000 | emp_id=1          ------> actual row
```

```sql
CREATE NONCLUSTERED INDEX idx_salary ON employees(salary);
CREATE NONCLUSTERED INDEX idx_dept_salary ON employees(dept_id, salary);

-- Lookup process:
-- 1. SQL scans non-clustered index B-Tree for matching keys
-- 2. Follows row locator pointer to fetch full row (KEY LOOKUP / RID LOOKUP)
-- 3. Very expensive for large result sets → optimizer may choose table scan instead
```

## 14.3 Composite Index

```sql
-- Index on multiple columns
CREATE INDEX idx_dept_salary ON employees(dept_id, salary DESC);

-- Leftmost prefix rule:
-- Index can be used for:
--   WHERE dept_id = 10
--   WHERE dept_id = 10 AND salary > 70000
--   ORDER BY dept_id, salary
-- Index CANNOT be used efficiently for:
--   WHERE salary > 70000 (skips dept_id, the leading column)
--   ORDER BY salary (skips dept_id)

-- Column order matters: place high-cardinality, frequently-filtered columns first
CREATE INDEX idx_status_date ON orders(status, order_date);  -- if filtering by status first
```

## 14.4 Covering Index

**Concept:** An index that includes ALL columns needed by a query, eliminating the need to access the base table (no key lookup).

```sql
-- Query
SELECT emp_name, salary FROM employees WHERE dept_id = 10;

-- Without covering index: 
-- 1. Scan index on dept_id → 2. KEY LOOKUP to get emp_name and salary

-- Covering index: includes all queried columns
CREATE NONCLUSTERED INDEX idx_dept_covering 
ON employees(dept_id)
INCLUDE (emp_name, salary);  -- INCLUDE columns don't narrow search but avoid key lookups
-- Now the entire query is satisfied from the index alone
```

## 14.5 Index Interview Questions

**Q: What is the difference between Clustered and Non-Clustered Index?**  
A: A clustered index determines the physical order of data rows in the table — the leaf nodes of the B-Tree are the actual data pages. There can be only one clustered index per table. A non-clustered index is a separate B-Tree structure with key columns and pointers (row locators) to the actual data. A table can have many non-clustered indexes. Non-clustered indexes require an extra step to fetch non-indexed columns (key lookup).

**Q: When would you NOT use an index?**  
A:
- Small tables (full scan is faster than index overhead)
- Columns with very low cardinality (e.g., boolean flag — 50% selectivity makes index useless)
- Columns with many NULLs that are frequently queried for NULLs
- Tables with very frequent INSERT/UPDATE/DELETE (index maintenance overhead)
- Columns not used in WHERE, JOIN, or ORDER BY

**Q: What is an index covering query?**  
A: A query is "covered" by an index when all columns it needs (in SELECT, WHERE, JOIN, ORDER BY) are present in the index, so the engine never needs to access the base table. Use INCLUDE columns to add non-key columns to the index leaf level for covering.

**Q: How do you find unused or missing indexes?**
```sql
-- SQL Server: Missing index DMVs
SELECT 
    migs.avg_total_user_cost * migs.avg_user_impact * (migs.user_seeks + migs.user_scans) AS improvement_measure,
    'CREATE INDEX idx_' + CONVERT(VARCHAR, mid.index_handle) + 
    ' ON ' + mid.statement + ' (' + 
    ISNULL(mid.equality_columns, '') + 
    ISNULL(', ' + mid.inequality_columns, '') + ')' +
    ISNULL(' INCLUDE (' + mid.included_columns + ')', '') AS create_index_statement
FROM sys.dm_db_missing_index_groups mig
JOIN sys.dm_db_missing_index_group_stats migs ON mig.index_group_handle = migs.group_handle
JOIN sys.dm_db_missing_index_details mid ON mig.index_handle = mid.index_handle
ORDER BY improvement_measure DESC;

-- Find unused indexes
SELECT OBJECT_NAME(i.object_id) AS table_name, i.name AS index_name,
       ius.user_seeks, ius.user_scans, ius.user_lookups, ius.user_updates
FROM sys.indexes i
LEFT JOIN sys.dm_db_index_usage_stats ius ON i.object_id = ius.object_id AND i.index_id = ius.index_id
WHERE OBJECT_NAME(i.object_id) NOT IN ('sysdiagrams')
  AND ius.user_seeks + ius.user_scans + ius.user_lookups = 0
ORDER BY ius.user_updates DESC;
```

---

# SECTION 15: QUERY OPTIMIZATION

---

## 15.1 Execution Plans

```sql
-- SQL Server: View estimated execution plan
-- Press Ctrl+L or prepend EXPLAIN
SET SHOWPLAN_ALL ON;
SELECT * FROM employees WHERE dept_id = 10;
SET SHOWPLAN_ALL OFF;

-- Actual execution plan (runs query)
SET STATISTICS IO ON;
SET STATISTICS TIME ON;
SELECT * FROM employees WHERE dept_id = 10;
SET STATISTICS IO OFF;
SET STATISTICS TIME OFF;

-- PostgreSQL
EXPLAIN SELECT * FROM employees WHERE dept_id = 10;
EXPLAIN ANALYZE SELECT * FROM employees WHERE dept_id = 10;
-- ANALYZE actually runs the query
```

**Key execution plan operators:**
- **Index Seek:** Using index to jump to specific rows — FAST
- **Index Scan:** Scanning entire index — slower, but better than table scan
- **Table Scan / Clustered Index Scan:** Reading entire table — SLOW for large tables
- **Key Lookup / RID Lookup:** Extra trip to base table after index seek — can be expensive
- **Nested Loops:** Good for small inner table
- **Hash Match:** Good for large, unsorted inputs
- **Merge Join:** Fastest when both inputs are sorted

## 15.2 Optimization Techniques

```sql
-- 1. Avoid SELECT * — only fetch needed columns
-- BAD:
SELECT * FROM orders WHERE customer_id = 101;
-- GOOD:
SELECT order_id, order_date, amount FROM orders WHERE customer_id = 101;

-- 2. Avoid functions on indexed columns in WHERE
-- BAD (index on hire_date not used):
SELECT * FROM employees WHERE YEAR(hire_date) = 2024;
-- GOOD:
SELECT * FROM employees WHERE hire_date >= '2024-01-01' AND hire_date < '2025-01-01';

-- 3. Use EXISTS instead of COUNT for existence checks
-- BAD:
IF (SELECT COUNT(*) FROM orders WHERE customer_id = 101) > 0 ...
-- GOOD:
IF EXISTS (SELECT 1 FROM orders WHERE customer_id = 101) ...

-- 4. Avoid DISTINCT when not needed (it forces sort/hash)
-- If data is already unique, don't add DISTINCT

-- 5. Use JOINs instead of correlated subqueries for large tables
-- BAD (N+1 problem):
SELECT emp_name, (SELECT dept_name FROM departments WHERE dept_id = e.dept_id) AS dept
FROM employees e;
-- GOOD:
SELECT e.emp_name, d.dept_name
FROM employees e JOIN departments d ON e.dept_id = d.dept_id;

-- 6. Push conditions down (filter early)
-- BAD: join all rows then filter
SELECT * FROM large_table l JOIN other_table o ON l.id = o.id
WHERE l.status = 'Active';
-- Usually same, but optimizer may not push through complex views:
SELECT * FROM (SELECT * FROM large_table WHERE status = 'Active') l
JOIN other_table o ON l.id = o.id;

-- 7. Use appropriate data types (INT vs VARCHAR for joins)
-- Joining INT to INT is faster than INT to VARCHAR

-- 8. Avoid OR on different columns (prevents index use)
-- BAD:
SELECT * FROM employees WHERE emp_id = 101 OR email = 'alice@co.com';
-- GOOD: Use UNION
SELECT * FROM employees WHERE emp_id = 101
UNION
SELECT * FROM employees WHERE email = 'alice@co.com';

-- 9. Parameterize queries (prevents plan bloat)
-- BAD (new plan for each literal):
SELECT * FROM orders WHERE customer_id = 101;
SELECT * FROM orders WHERE customer_id = 102;
-- GOOD: Use parameterized query or stored procedure
```

## 15.3 Partitioning

```sql
-- Table partitioning for large tables (e.g., time-series data)

-- SQL Server: Create partition function
CREATE PARTITION FUNCTION pf_OrderDate (DATE)
AS RANGE RIGHT FOR VALUES ('2022-01-01', '2023-01-01', '2024-01-01');

-- Create partition scheme
CREATE PARTITION SCHEME ps_OrderDate
AS PARTITION pf_OrderDate
TO (fg_2021, fg_2022, fg_2023, fg_2024, fg_current);

-- Create partitioned table
CREATE TABLE orders (
    order_id   INT,
    order_date DATE,
    amount     DECIMAL(10,2)
) ON ps_OrderDate(order_date);

-- Benefits:
-- 1. Partition elimination: query on 2024 data only scans 2024 partition
-- 2. Faster DELETEs: TRUNCATE PARTITION vs row-by-row delete
-- 3. Partition switching: load new partition without locking existing data
```

---

# SECTION 16: DATABASE DESIGN & NORMALIZATION

---

## 16.1 First Normal Form (1NF)

**Rules:**
1. Each column contains atomic (indivisible) values
2. Each column contains values of the same type
3. Each row is unique (has a primary key)
4. No repeating groups

```
BEFORE 1NF (VIOLATION):
order_id | customer | products
---------|----------|----------
1        | Alice    | Laptop, Mouse, Keyboard   <- not atomic!
2        | Bob      | Phone

AFTER 1NF:
order_id | customer | product
---------|----------|----------
1        | Alice    | Laptop
1        | Alice    | Mouse
1        | Alice    | Keyboard
2        | Bob      | Phone
```

## 16.2 Second Normal Form (2NF)

**Rules:** Must be in 1NF + No partial dependencies (all non-key columns depend on the FULL primary key, not just part of it).

*Only relevant when the table has a composite primary key.*

```
BEFORE 2NF (PK = order_id + product_id):
order_id | product_id | quantity | product_name | customer_name
---------|------------|----------|--------------|---------------
1        | P01        | 2        | Laptop       | Alice
1        | P02        | 1        | Mouse        | Alice
                                   ↑                    ↑
                            depends only on         depends only on
                            product_id               order_id
                            (partial dep!)           (partial dep!)

AFTER 2NF: Split into 3 tables
orders:           order_items:         products:
order_id|customer order_id|product_id  product_id|product_name
--------|-------- ---------|---------- ----------|------------
1       |Alice    1        |P01|2      P01        |Laptop
2       |Bob      1        |P02|1      P02        |Mouse
```

## 16.3 Third Normal Form (3NF)

**Rules:** Must be in 2NF + No transitive dependencies (non-key columns should not depend on other non-key columns).

```
BEFORE 3NF:
emp_id | emp_name | dept_id | dept_name | dept_location
-------|----------|---------|-----------|---------------
1      | Alice    | 10      | IT        | New York
2      | Bob      | 20      | HR        | London
                               ↑               ↑
                    dept_name and dept_location depend on dept_id (not on emp_id)
                    This is a TRANSITIVE dependency!

AFTER 3NF:
employees:              departments:
emp_id|emp_name|dept_id  dept_id|dept_name|location
------|--------|-------  -------|---------|--------
1     |Alice   |10       10     |IT       |New York
2     |Bob     |20       20     |HR       |London
```

## 16.4 BCNF (Boyce-Codd Normal Form)

**Rules:** Must be in 3NF + For every functional dependency X → Y, X must be a super key.

A stricter version of 3NF that handles certain edge cases with multiple candidate keys.

## 16.5 Star Schema vs Snowflake Schema

### Star Schema
```
                    [Date Dimension]
                          |
[Product Dimension] -- [FACT TABLE] -- [Customer Dimension]
                          |
                    [Store Dimension]

FACT TABLE: order_id, date_id, product_id, customer_id, store_id, quantity, revenue
```

**Characteristics:**
- Denormalized dimension tables (all attributes in one table)
- Simple, fast queries (few joins)
- Used in data warehouses / OLAP
- More storage (redundant data in dimensions)

### Snowflake Schema
```
[City] → [Country]
  ↓
[Store] → [Region]
  ↓
[FACT TABLE] → [Product] → [Category] → [Department]
  ↓
[Customer] → [City] → [Country]
```

**Characteristics:**
- Normalized dimension tables (hierarchies split into multiple tables)
- More complex queries (more joins)
- Less storage (no redundancy)
- Better for large slowly changing dimensions

**Interview Q: When would you choose Star Schema over Snowflake?**  
A: Star Schema is preferred for analytical workloads (data warehouses, OLAP, BI tools) where query simplicity and speed matter most. The denormalized structure reduces join complexity, making queries faster and easier to write. Snowflake Schema is better when dimension tables are very large (millions of rows) and storage is a concern, or when the organization requires strict data consistency and the ETL process benefits from normalized staging. Most modern cloud data warehouses (Redshift, Snowflake, BigQuery) handle star schema efficiently with columnar storage.
