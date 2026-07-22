# SQL INTERVIEW PREPARATION — COMPLETE GUIDE
## Part 1: SQL Fundamentals & Database Concepts

---

# SECTION 1: DATABASE FUNDAMENTALS

---

## 1.1 DBMS vs RDBMS

### Concept
| Feature | DBMS | RDBMS |
|---|---|---|
| Data Storage | Files (hierarchical/network) | Tables (rows & columns) |
| Relationships | No formal relationship | Foreign keys enforce relationships |
| ACID | Not guaranteed | Fully supported |
| Data Redundancy | High | Minimized via normalization |
| SQL Support | May not support | Full SQL support |
| Examples | XML stores, file systems | MySQL, PostgreSQL, Oracle, SQL Server |

### Real-World Example
A hospital using flat files to store patient records = DBMS.  
Same hospital using MySQL with `patients`, `doctors`, `appointments` tables with FK constraints = RDBMS.

### Interview Questions & Answers

**Q: What is the difference between DBMS and RDBMS?**  
A: DBMS stores data as files without enforcing relationships between datasets. RDBMS stores data in structured tables and enforces relationships through primary/foreign keys, supports ACID transactions, and uses SQL as the standard query language. Examples: MySQL, PostgreSQL, Oracle are RDBMS; XML-based or hierarchical stores are DBMS.

**Q: Why is RDBMS preferred for enterprise applications?**  
A: RDBMS provides data integrity through constraints, supports concurrent multi-user access via transactions, enforces referential integrity, and provides standardized SQL for querying. These properties are critical for financial, e-commerce, and healthcare systems.

---

## 1.2 KEYS IN SQL

### 1.2.1 Primary Key

**Concept:** Uniquely identifies each row in a table. Cannot be NULL. Only one per table.

```sql
CREATE TABLE employees (
    emp_id     INT         PRIMARY KEY,
    emp_name   VARCHAR(100) NOT NULL,
    email      VARCHAR(150) UNIQUE
);
```

**Real-World Example:** `employee_id` in an HR table — no two employees share the same ID, and every record must have one.

**Interview Q: Can a Primary Key contain NULL?**  
A: No. By definition, a Primary Key must be NOT NULL and UNIQUE. If a column is defined as PRIMARY KEY, NULL values are automatically rejected.

**Common Mistake:** Confusing PK with UNIQUE — UNIQUE allows one NULL (in most databases), PK allows none.

---

### 1.2.2 Foreign Key

**Concept:** A column (or set of columns) that references the Primary Key of another table. Enforces referential integrity.

```sql
CREATE TABLE orders (
    order_id    INT PRIMARY KEY,
    customer_id INT,
    order_date  DATE,
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
        ON DELETE CASCADE
        ON UPDATE CASCADE
);
```

**ON DELETE options:**
- `CASCADE` — deletes child rows when parent is deleted
- `SET NULL` — sets FK to NULL when parent is deleted
- `RESTRICT` — prevents deletion of parent if child exists
- `NO ACTION` — similar to RESTRICT (evaluated at end of transaction)

**Interview Q: What happens if you try to insert a record with a FK value that doesn't exist in the parent table?**  
A: The database throws a foreign key constraint violation error and rejects the INSERT. This is referential integrity enforcement.

**Interview Q: What is ON DELETE CASCADE?**  
A: When the parent record is deleted, all child records referencing it are automatically deleted. Used in scenarios like deleting a customer also deletes all their orders.

---

### 1.2.3 Unique Key

**Concept:** Ensures all values in a column are distinct. Unlike PK, allows one NULL value (in most RDBMS). Multiple unique keys per table allowed.

```sql
CREATE TABLE users (
    user_id   INT PRIMARY KEY,
    email     VARCHAR(200) UNIQUE,
    phone     VARCHAR(15) UNIQUE
);
```

**Interview Q: Difference between Primary Key and Unique Key?**

| Aspect | Primary Key | Unique Key |
|---|---|---|
| NULLs | Not allowed | One NULL allowed |
| Count per table | Only one | Multiple allowed |
| Clustered Index | Creates by default (SQL Server) | Creates non-clustered index |
| Purpose | Row identification | Data uniqueness |

---

### 1.2.4 Composite Key

**Concept:** A Primary Key made of two or more columns. Used when no single column uniquely identifies a row.

```sql
CREATE TABLE order_items (
    order_id   INT,
    product_id INT,
    quantity   INT,
    PRIMARY KEY (order_id, product_id)  -- Composite PK
);
```

**Real-World Example:** An `order_items` table — neither `order_id` alone nor `product_id` alone is unique, but the combination is.

**Interview Q: When would you use a Composite Key?**  
A: When a single column cannot uniquely identify rows but a combination of columns can. Common in junction/bridge tables for many-to-many relationships (e.g., `student_courses` table with `student_id` + `course_id`).

---

### 1.2.5 Candidate Key

**Concept:** Any column (or set of columns) that can uniquely identify a row. A table may have multiple candidate keys; the one chosen becomes the Primary Key.

**Example:** In a `students` table:
- `student_id` — unique
- `email` — unique
- `aadhar_number` — unique

All three are Candidate Keys. We pick `student_id` as the Primary Key.

---

### 1.2.6 Super Key

**Concept:** Any set of columns that can uniquely identify a row, including supersets of candidate keys.

**Example:** `{student_id}`, `{email}`, `{student_id, email}`, `{student_id, name}` are all super keys.  
Candidate keys are the **minimal** super keys (no redundant columns).

**Interview Q: What is the relationship between Super Key, Candidate Key, and Primary Key?**  
A: Super Key ⊃ Candidate Key ⊃ Primary Key.  
- Super Key: any combo that uniquely identifies rows (may have redundant columns)  
- Candidate Key: minimal super key (no redundant columns)  
- Primary Key: the chosen candidate key for the table

---

## 1.3 CONSTRAINTS

```sql
CREATE TABLE products (
    product_id   INT          PRIMARY KEY,          -- PRIMARY KEY
    product_name VARCHAR(200) NOT NULL,              -- NOT NULL
    price        DECIMAL(10,2) DEFAULT 0.00,         -- DEFAULT
    category_id  INT,
    stock        INT          CHECK (stock >= 0),    -- CHECK
    sku          VARCHAR(50)  UNIQUE,                -- UNIQUE
    FOREIGN KEY (category_id) REFERENCES categories(id)  -- FOREIGN KEY
);
```

### Constraint Types Summary

| Constraint | Purpose |
|---|---|
| NOT NULL | Column must have a value |
| UNIQUE | All values must be distinct |
| PRIMARY KEY | NOT NULL + UNIQUE; row identifier |
| FOREIGN KEY | Referential integrity between tables |
| CHECK | Value must satisfy a condition |
| DEFAULT | Provides a default value if none given |

**Interview Q: Can you add a constraint to an existing table?**
```sql
-- Add NOT NULL
ALTER TABLE employees ALTER COLUMN phone VARCHAR(15) NOT NULL;

-- Add CHECK constraint
ALTER TABLE employees ADD CONSTRAINT chk_salary CHECK (salary > 0);

-- Add UNIQUE
ALTER TABLE employees ADD CONSTRAINT uq_email UNIQUE (email);
```

---

## 1.4 NULL HANDLING

### Concept
NULL means "unknown" or "missing" — it is NOT zero, NOT empty string, NOT false.

**Key Rules:**
- `NULL = NULL` evaluates to UNKNOWN (not TRUE)
- Use `IS NULL` / `IS NOT NULL` to check for nulls
- Aggregate functions (SUM, AVG, COUNT) ignore NULLs except `COUNT(*)`
- Any arithmetic with NULL returns NULL: `5 + NULL = NULL`

```sql
-- Wrong: This returns no rows because NULL = NULL is UNKNOWN
SELECT * FROM employees WHERE manager_id = NULL;

-- Correct
SELECT * FROM employees WHERE manager_id IS NULL;

-- COALESCE: return first non-null value
SELECT emp_name, COALESCE(phone, email, 'No Contact') AS contact
FROM employees;

-- NULLIF: returns NULL if two values are equal (prevents division by zero)
SELECT total_sales / NULLIF(total_orders, 0) AS avg_order_value
FROM sales_summary;

-- ISNULL (SQL Server) / IFNULL (MySQL)
SELECT emp_name, ISNULL(bonus, 0) AS bonus FROM employees;
```

**Interview Q: What is the difference between COALESCE and ISNULL?**  
A: `ISNULL(expr, replacement)` is SQL Server specific and takes exactly 2 arguments. `COALESCE(expr1, expr2, ..., exprN)` is ANSI SQL standard and returns the first non-null from N arguments. COALESCE is preferred for portability.

**Interview Q: Does COUNT(*) count NULLs?**  
A: `COUNT(*)` counts all rows including NULLs. `COUNT(column_name)` counts only non-NULL values in that column.

```sql
SELECT 
    COUNT(*)           AS total_rows,        -- includes NULLs
    COUNT(bonus)       AS rows_with_bonus,   -- excludes NULLs
    COUNT(DISTINCT dept) AS unique_depts
FROM employees;
```

**Common Mistakes with NULL:**
1. Using `= NULL` instead of `IS NULL`
2. Forgetting that `NOT IN` with NULLs in the subquery returns no rows
3. Assuming `AVG` counts NULL rows (it doesn't)

```sql
-- Dangerous: If subquery returns any NULL, the whole NOT IN returns empty
SELECT * FROM employees 
WHERE dept_id NOT IN (SELECT dept_id FROM departments); -- if dept_id has NULLs, returns 0 rows!

-- Safe alternative
SELECT * FROM employees e
WHERE NOT EXISTS (
    SELECT 1 FROM departments d WHERE d.dept_id = e.dept_id
);
```

---

## 1.5 DATA TYPES

### Numeric Types
| Type | Range | Use |
|---|---|---|
| INT / INTEGER | -2B to 2B | IDs, counts |
| BIGINT | -9.2 × 10^18 to 9.2 × 10^18 | Large IDs, timestamps |
| SMALLINT | -32K to 32K | Codes, flags |
| DECIMAL(p,s) / NUMERIC(p,s) | Fixed precision | Money, prices |
| FLOAT / REAL | Approx decimal | Scientific calculations |

### String Types
| Type | Use |
|---|---|
| CHAR(n) | Fixed-length strings (padded with spaces) |
| VARCHAR(n) | Variable-length strings (up to n chars) |
| TEXT / NVARCHAR(MAX) | Large text |
| NCHAR / NVARCHAR | Unicode characters |

**Interview Q: CHAR vs VARCHAR?**  
A: CHAR(10) always stores 10 bytes (padded with spaces). VARCHAR(10) stores only as many bytes as needed + 2 bytes for length metadata. CHAR is faster for fixed-length data (e.g., country codes). VARCHAR is more space-efficient for variable data (e.g., names).

### Date/Time Types
| Type | Format | Use |
|---|---|---|
| DATE | YYYY-MM-DD | Dates only |
| TIME | HH:MM:SS | Time only |
| DATETIME | YYYY-MM-DD HH:MM:SS | Date + time |
| TIMESTAMP | YYYY-MM-DD HH:MM:SS | Auto-updates on row change |
| YEAR | YYYY | Year only |

---

# SECTION 2: SQL COMMANDS

---

## 2.1 DDL — Data Definition Language

**Commands:** CREATE, ALTER, DROP, TRUNCATE, RENAME  
**Auto-commits:** Yes (cannot be rolled back in most databases)

### CREATE
```sql
-- Create table
CREATE TABLE departments (
    dept_id    INT          PRIMARY KEY,
    dept_name  VARCHAR(100) NOT NULL,
    location   VARCHAR(100),
    created_at DATETIME     DEFAULT GETDATE()
);

-- Create table from SELECT
CREATE TABLE emp_backup AS
SELECT * FROM employees WHERE dept_id = 10;

-- Create index
CREATE INDEX idx_emp_dept ON employees(dept_id);

-- Create view
CREATE VIEW active_employees AS
SELECT emp_id, emp_name, dept_id 
FROM employees 
WHERE status = 'Active';
```

### ALTER
```sql
-- Add column
ALTER TABLE employees ADD middle_name VARCHAR(50);

-- Drop column
ALTER TABLE employees DROP COLUMN middle_name;

-- Modify column type
ALTER TABLE employees ALTER COLUMN salary DECIMAL(12,2);  -- SQL Server
ALTER TABLE employees MODIFY salary DECIMAL(12,2);        -- MySQL

-- Rename column (SQL Server 2022+)
EXEC sp_rename 'employees.emp_name', 'employee_name', 'COLUMN';

-- Add constraint
ALTER TABLE employees ADD CONSTRAINT fk_dept 
    FOREIGN KEY (dept_id) REFERENCES departments(dept_id);

-- Drop constraint
ALTER TABLE employees DROP CONSTRAINT fk_dept;
```

### DROP vs TRUNCATE vs DELETE

```sql
DROP TABLE employees;    -- Removes table + structure + data + indexes + constraints
TRUNCATE TABLE employees; -- Removes all data, resets identity, keeps structure
DELETE FROM employees;   -- Removes all rows (logged, can be rolled back)
DELETE FROM employees WHERE dept_id = 10; -- Conditional delete
```

| Operation | Structure | Data | WHERE clause | Rollback | Speed |
|---|---|---|---|---|---|
| DROP | Removed | Removed | No | No (DDL) | Fastest |
| TRUNCATE | Kept | All removed | No | No* | Fast |
| DELETE | Kept | Conditional | Yes | Yes | Slow (logged) |

*TRUNCATE can be rolled back inside an explicit transaction in SQL Server and PostgreSQL.

**Interview Q: What is the difference between DROP and TRUNCATE?**  
A: DROP removes the entire table structure along with all data, indexes, constraints, and triggers. The table no longer exists after DROP. TRUNCATE removes all rows from a table but keeps the table structure intact, resets identity columns, and is much faster than DELETE because it doesn't log individual row deletions.

---

## 2.2 DML — Data Manipulation Language

**Commands:** SELECT, INSERT, UPDATE, DELETE  
**Transactional:** Yes (can be rolled back)

### INSERT
```sql
-- Single row
INSERT INTO employees (emp_id, emp_name, salary, dept_id)
VALUES (101, 'Alice Johnson', 75000, 10);

-- Multiple rows
INSERT INTO employees (emp_id, emp_name, salary, dept_id)
VALUES 
    (102, 'Bob Smith', 65000, 20),
    (103, 'Carol White', 80000, 10),
    (104, 'Dave Brown', 70000, 30);

-- Insert from SELECT
INSERT INTO emp_archive
SELECT emp_id, emp_name, salary, dept_id, GETDATE()
FROM employees
WHERE status = 'Inactive';

-- INSERT with OUTPUT (SQL Server) — returns inserted rows
INSERT INTO employees OUTPUT INSERTED.*
VALUES (105, 'Eve Davis', 90000, 10, 'Active');
```

### UPDATE
```sql
-- Single column
UPDATE employees SET salary = 80000 WHERE emp_id = 101;

-- Multiple columns
UPDATE employees 
SET salary = salary * 1.10, 
    last_updated = GETDATE()
WHERE dept_id = 10 AND performance_rating = 'A';

-- Update with JOIN (SQL Server)
UPDATE e
SET e.dept_name_cache = d.dept_name
FROM employees e
INNER JOIN departments d ON e.dept_id = d.dept_id;

-- Update with subquery
UPDATE employees
SET salary = (SELECT AVG(salary) * 1.05 FROM employees WHERE dept_id = 10)
WHERE dept_id = 10 AND salary < (SELECT AVG(salary) FROM employees WHERE dept_id = 10);
```

### DELETE
```sql
-- Delete with condition
DELETE FROM employees WHERE emp_id = 101;

-- Delete with JOIN
DELETE e FROM employees e
INNER JOIN departments d ON e.dept_id = d.dept_id
WHERE d.dept_name = 'Old Department';

-- Delete with subquery
DELETE FROM orders
WHERE customer_id IN (
    SELECT customer_id FROM customers WHERE status = 'Blacklisted'
);

-- Delete with OUTPUT (SQL Server)
DELETE FROM employees 
OUTPUT DELETED.emp_id, DELETED.emp_name
WHERE status = 'Terminated';
```

---

## 2.3 DCL — Data Control Language

**Commands:** GRANT, REVOKE

```sql
-- Grant permissions
GRANT SELECT, INSERT ON employees TO 'analyst_user';
GRANT ALL PRIVILEGES ON DATABASE hr_db TO 'admin_user';

-- Revoke permissions
REVOKE INSERT ON employees FROM 'analyst_user';

-- Grant with GRANT OPTION (allow user to grant others)
GRANT SELECT ON employees TO 'manager_user' WITH GRANT OPTION;
```

---

## 2.4 TCL — Transaction Control Language

**Commands:** BEGIN TRANSACTION, COMMIT, ROLLBACK, SAVEPOINT

```sql
BEGIN TRANSACTION;

    UPDATE accounts SET balance = balance - 5000 WHERE account_id = 'A001';
    UPDATE accounts SET balance = balance + 5000 WHERE account_id = 'A002';

    -- Check if both succeeded
    IF @@ERROR <> 0
        ROLLBACK TRANSACTION;
    ELSE
        COMMIT TRANSACTION;

-- SAVEPOINT example
BEGIN TRANSACTION;
    INSERT INTO orders VALUES (1001, 201, GETDATE());
    SAVEPOINT after_order;
    
    INSERT INTO order_items VALUES (1001, 501, 2, 150.00);
    -- If this fails, rollback to savepoint (not entire transaction)
    ROLLBACK TO SAVEPOINT after_order;
    
COMMIT;
```

**ACID Properties:**
- **Atomicity:** All operations in a transaction succeed or all fail
- **Consistency:** Database moves from one valid state to another
- **Isolation:** Transactions don't interfere with each other
- **Durability:** Committed changes persist even after system failure

**Interview Q: What are isolation levels?**

| Level | Dirty Read | Non-Repeatable Read | Phantom Read |
|---|---|---|---|
| READ UNCOMMITTED | Yes | Yes | Yes |
| READ COMMITTED | No | Yes | Yes |
| REPEATABLE READ | No | No | Yes |
| SERIALIZABLE | No | No | No |

---

# SECTION 3: FILTERING AND SORTING

---

## 3.1 WHERE Clause

```sql
-- Basic conditions
SELECT * FROM employees WHERE salary > 50000;
SELECT * FROM employees WHERE dept_id = 10 AND status = 'Active';
SELECT * FROM employees WHERE dept_id = 10 OR dept_id = 20;
SELECT * FROM employees WHERE NOT (dept_id = 10);

-- Operator precedence: NOT > AND > OR
-- Use parentheses to be explicit
SELECT * FROM employees 
WHERE (dept_id = 10 OR dept_id = 20) AND salary > 60000;
```

## 3.2 ORDER BY

```sql
-- Ascending (default)
SELECT * FROM employees ORDER BY salary;
SELECT * FROM employees ORDER BY salary ASC;

-- Descending
SELECT * FROM employees ORDER BY salary DESC;

-- Multiple columns
SELECT * FROM employees ORDER BY dept_id ASC, salary DESC;

-- Order by column position
SELECT emp_name, salary, dept_id FROM employees ORDER BY 2 DESC;

-- Order by expression
SELECT emp_name, salary * 12 AS annual_salary 
FROM employees 
ORDER BY annual_salary DESC;

-- NULLS FIRST / NULLS LAST (PostgreSQL)
SELECT * FROM employees ORDER BY bonus DESC NULLS LAST;
```

## 3.3 LIMIT / TOP / FETCH

```sql
-- MySQL / PostgreSQL
SELECT * FROM employees ORDER BY salary DESC LIMIT 10;
SELECT * FROM employees ORDER BY salary DESC LIMIT 10 OFFSET 20; -- rows 21-30

-- SQL Server
SELECT TOP 10 * FROM employees ORDER BY salary DESC;
SELECT TOP 10 PERCENT * FROM employees ORDER BY salary DESC;

-- ANSI SQL (SQL Server 2012+, PostgreSQL)
SELECT * FROM employees 
ORDER BY salary DESC
OFFSET 0 ROWS FETCH NEXT 10 ROWS ONLY;

-- Page 3 (rows 21-30)
SELECT * FROM employees 
ORDER BY salary DESC
OFFSET 20 ROWS FETCH NEXT 10 ROWS ONLY;
```

## 3.4 DISTINCT

```sql
-- Distinct single column
SELECT DISTINCT dept_id FROM employees;

-- Distinct multiple columns (combination must be unique)
SELECT DISTINCT dept_id, job_title FROM employees;

-- COUNT DISTINCT
SELECT COUNT(DISTINCT dept_id) AS unique_departments FROM employees;

-- DISTINCT vs GROUP BY performance note:
-- GROUP BY can use indexes more efficiently for large datasets
-- SELECT dept_id FROM employees GROUP BY dept_id;  -- often faster
```

**Performance Note:** DISTINCT requires sorting/hashing all rows to remove duplicates. For large tables, consider whether GROUP BY or EXISTS serves better.

## 3.5 BETWEEN

```sql
-- Inclusive on both ends
SELECT * FROM employees WHERE salary BETWEEN 50000 AND 80000;
-- Equivalent to: WHERE salary >= 50000 AND salary <= 80000

-- Date range
SELECT * FROM orders 
WHERE order_date BETWEEN '2024-01-01' AND '2024-12-31';

-- NOT BETWEEN
SELECT * FROM employees WHERE salary NOT BETWEEN 50000 AND 80000;
```

## 3.6 IN / NOT IN

```sql
SELECT * FROM employees WHERE dept_id IN (10, 20, 30);
SELECT * FROM employees WHERE dept_id NOT IN (10, 20, 30);

-- IN with subquery
SELECT * FROM employees 
WHERE dept_id IN (SELECT dept_id FROM departments WHERE location = 'New York');

-- WARNING: NOT IN with NULL in subquery
-- If subquery returns any NULL, NOT IN returns empty result set!
SELECT * FROM employees 
WHERE dept_id NOT IN (10, 20, NULL);  -- Returns 0 rows!

-- Safe alternative using NOT EXISTS
SELECT * FROM employees e
WHERE NOT EXISTS (
    SELECT 1 FROM departments d 
    WHERE d.dept_id = e.dept_id AND d.location = 'London'
);
```

## 3.7 LIKE

```sql
-- Wildcards: % (any sequence), _ (single character)
SELECT * FROM employees WHERE emp_name LIKE 'A%';       -- starts with A
SELECT * FROM employees WHERE emp_name LIKE '%son';     -- ends with son
SELECT * FROM employees WHERE emp_name LIKE '%anna%';   -- contains anna
SELECT * FROM employees WHERE emp_name LIKE 'J__n';     -- John, Joan (4 chars)
SELECT * FROM employees WHERE phone LIKE '91________';  -- 10 digit Indian number

-- Case sensitivity depends on database collation
-- PostgreSQL: ILIKE for case-insensitive
SELECT * FROM employees WHERE emp_name ILIKE 'alice%';

-- Escape special characters
SELECT * FROM products WHERE description LIKE '50\% off' ESCAPE '\';
```

**Performance Note:** Leading wildcard (`'%text'`) prevents index usage. Use full-text search for pattern matching on large tables.

## 3.8 CASE WHEN

```sql
-- Simple CASE
SELECT emp_name,
    CASE dept_id
        WHEN 10 THEN 'Engineering'
        WHEN 20 THEN 'Marketing'
        WHEN 30 THEN 'Finance'
        ELSE 'Other'
    END AS department_name
FROM employees;

-- Searched CASE
SELECT emp_name, salary,
    CASE
        WHEN salary < 40000 THEN 'Entry Level'
        WHEN salary BETWEEN 40000 AND 70000 THEN 'Mid Level'
        WHEN salary BETWEEN 70001 AND 100000 THEN 'Senior Level'
        ELSE 'Executive'
    END AS salary_band
FROM employees;

-- CASE in aggregate
SELECT 
    dept_id,
    SUM(CASE WHEN gender = 'M' THEN 1 ELSE 0 END) AS male_count,
    SUM(CASE WHEN gender = 'F' THEN 1 ELSE 0 END) AS female_count,
    COUNT(*) AS total
FROM employees
GROUP BY dept_id;

-- CASE in ORDER BY
SELECT emp_name, status
FROM employees
ORDER BY 
    CASE status 
        WHEN 'Active' THEN 1 
        WHEN 'On Leave' THEN 2 
        ELSE 3 
    END;
```

---

# SECTION 4: JOINS — COMPLETE REFERENCE

---

## Setup: Sample Tables for All Join Examples

```sql
-- DEPARTMENTS table
CREATE TABLE departments (
    dept_id   INT PRIMARY KEY,
    dept_name VARCHAR(100)
);

INSERT INTO departments VALUES
(10, 'Engineering'),
(20, 'Marketing'),
(30, 'Finance'),
(40, 'HR');       -- No employees assigned

-- EMPLOYEES table
CREATE TABLE employees (
    emp_id    INT PRIMARY KEY,
    emp_name  VARCHAR(100),
    salary    DECIMAL(10,2),
    dept_id   INT           -- FK to departments
);

INSERT INTO employees VALUES
(1, 'Alice',   90000, 10),
(2, 'Bob',     75000, 20),
(3, 'Carol',   85000, 10),
(4, 'Dave',    60000, 30),
(5, 'Eve',     95000, NULL);  -- No department
```

---

## 4.1 INNER JOIN

**Returns:** Only rows with matching values in BOTH tables.

```
departments:          employees:
10 | Engineering  ->  1 | Alice | 10
20 | Marketing    ->  2 | Bob   | 20
30 | Finance      ->  4 | Dave  | 30
40 | HR           ->  (no match)
                      5 | Eve   | NULL (no match)

Result: Rows 1, 2, 3, 4 (matched both sides)
```

```sql
SELECT e.emp_id, e.emp_name, e.salary, d.dept_name
FROM employees e
INNER JOIN departments d ON e.dept_id = d.dept_id;
```

**Output:**
| emp_id | emp_name | salary | dept_name |
|---|---|---|---|
| 1 | Alice | 90000 | Engineering |
| 2 | Bob | 75000 | Marketing |
| 3 | Carol | 85000 | Engineering |
| 4 | Dave | 60000 | Finance |

Note: Eve (NULL dept_id) and HR department (no employees) are excluded.

**Interview Q: What does INNER JOIN return?**  
A: INNER JOIN returns only the rows where the join condition is satisfied in both tables. Rows with no match in either table are excluded from the result set.

---

## 4.2 LEFT JOIN (LEFT OUTER JOIN)

**Returns:** ALL rows from the LEFT table + matching rows from the right table. Non-matching right rows show NULL.

```
All employees (left) + matching department info (right)
Eve gets dept_name = NULL because no match
HR department not shown (it's in right table only)
```

```sql
SELECT e.emp_id, e.emp_name, e.salary, d.dept_name
FROM employees e
LEFT JOIN departments d ON e.dept_id = d.dept_id;
```

**Output:**
| emp_id | emp_name | salary | dept_name |
|---|---|---|---|
| 1 | Alice | 90000 | Engineering |
| 2 | Bob | 75000 | Marketing |
| 3 | Carol | 85000 | Engineering |
| 4 | Dave | 60000 | Finance |
| 5 | Eve | 95000 | NULL |

**Common Pattern — Find unmatched rows:**
```sql
-- Employees with NO department
SELECT e.emp_id, e.emp_name
FROM employees e
LEFT JOIN departments d ON e.dept_id = d.dept_id
WHERE d.dept_id IS NULL;
```

**Interview Q: How do you find all customers who have never placed an order?**
```sql
SELECT c.customer_id, c.customer_name
FROM customers c
LEFT JOIN orders o ON c.customer_id = o.customer_id
WHERE o.order_id IS NULL;
```

---

## 4.3 RIGHT JOIN (RIGHT OUTER JOIN)

**Returns:** ALL rows from the RIGHT table + matching rows from the left. Non-matching left rows show NULL.

```sql
SELECT e.emp_id, e.emp_name, d.dept_name
FROM employees e
RIGHT JOIN departments d ON e.dept_id = d.dept_id;
```

**Output:**
| emp_id | emp_name | dept_name |
|---|---|---|
| 1 | Alice | Engineering |
| 3 | Carol | Engineering |
| 2 | Bob | Marketing |
| 4 | Dave | Finance |
| NULL | NULL | HR |

**Note:** RIGHT JOIN is rarely used in practice — a LEFT JOIN with swapped table order produces the same result. Most developers prefer LEFT JOIN consistently.

---

## 4.4 FULL OUTER JOIN

**Returns:** ALL rows from BOTH tables. Non-matching rows from either side show NULL.

```sql
SELECT e.emp_id, e.emp_name, d.dept_id, d.dept_name
FROM employees e
FULL OUTER JOIN departments d ON e.dept_id = d.dept_id;
```

**Output:**
| emp_id | emp_name | dept_id | dept_name |
|---|---|---|---|
| 1 | Alice | 10 | Engineering |
| 3 | Carol | 10 | Engineering |
| 2 | Bob | 20 | Marketing |
| 4 | Dave | 30 | Finance |
| 5 | Eve | NULL | NULL |
| NULL | NULL | 40 | HR |

**MySQL Workaround (no FULL OUTER JOIN support):**
```sql
SELECT e.emp_id, e.emp_name, d.dept_name
FROM employees e LEFT JOIN departments d ON e.dept_id = d.dept_id
UNION
SELECT e.emp_id, e.emp_name, d.dept_name
FROM employees e RIGHT JOIN departments d ON e.dept_id = d.dept_id;
```

---

## 4.5 CROSS JOIN

**Returns:** Cartesian product — every row from table A paired with every row from table B.  
Result count = rows(A) × rows(B)

```sql
SELECT e.emp_name, d.dept_name
FROM employees e
CROSS JOIN departments d;
-- 5 employees × 4 departments = 20 rows
```

**Real-World Use Cases:**
- Generate all possible combinations (e.g., size × color for products)
- Create a date dimension table
- Generate test data

```sql
-- Generate date range using CROSS JOIN trick
SELECT DATEADD(DAY, ones.n + tens.n * 10, '2024-01-01') AS calendar_date
FROM (VALUES(0),(1),(2),(3),(4),(5),(6),(7),(8),(9)) AS ones(n)
CROSS JOIN (VALUES(0),(1),(2),(3),(4),(5),(6),(7),(8),(9)) AS tens(n)
WHERE DATEADD(DAY, ones.n + tens.n * 10, '2024-01-01') <= '2024-12-31';
```

---

## 4.6 SELF JOIN

**Returns:** A table joined with itself. Used for hierarchical/recursive data.

```sql
-- Employee-Manager relationship in same table
SELECT 
    e.emp_name   AS employee,
    m.emp_name   AS manager
FROM employees e
LEFT JOIN employees m ON e.manager_id = m.emp_id;
```

**Real-World Examples:**
- Employee-Manager hierarchy
- Parent-Child categories
- Friend relationships in social networks

```sql
-- Find employees earning more than their manager
SELECT e.emp_name AS employee, e.salary AS emp_salary,
       m.emp_name AS manager,  m.salary AS mgr_salary
FROM employees e
INNER JOIN employees m ON e.manager_id = m.emp_id
WHERE e.salary > m.salary;
```

**Interview Q: When would you use a SELF JOIN?**  
A: A SELF JOIN is used when a table has a relationship with itself — the classic example being an employees table where each employee has a `manager_id` that references another row in the same table. It's also used for comparing rows within the same table, like finding duplicate records or pairs of items.

---

## 4.7 JOIN Performance Considerations

```sql
-- Always join on indexed columns
-- Ensure FK columns are indexed (not automatic in all databases)
CREATE INDEX idx_orders_customer ON orders(customer_id);

-- Reduce rows before joining with WHERE in subquery
SELECT e.emp_name, d.dept_name
FROM (SELECT * FROM employees WHERE status = 'Active') e
INNER JOIN departments d ON e.dept_id = d.dept_id;

-- Avoid functions on joined columns (prevents index use)
-- BAD:
JOIN ON UPPER(e.dept_code) = UPPER(d.dept_code)
-- GOOD: normalize data at load time, join on normalized values

-- Use EXISTS instead of IN for large subqueries
SELECT * FROM customers c
WHERE EXISTS (SELECT 1 FROM orders o WHERE o.customer_id = c.customer_id);
```

---

# SECTION 5: AGGREGATE FUNCTIONS

---

## 5.1 COUNT

```sql
COUNT(*)           -- counts all rows including NULLs
COUNT(column)      -- counts non-NULL values in column
COUNT(DISTINCT col) -- counts distinct non-NULL values

SELECT 
    COUNT(*)                    AS total_employees,
    COUNT(bonus)                AS employees_with_bonus,
    COUNT(DISTINCT dept_id)     AS unique_departments,
    COUNT(DISTINCT manager_id)  AS unique_managers
FROM employees;
```

## 5.2 SUM, AVG, MIN, MAX

```sql
SELECT
    dept_id,
    COUNT(*)            AS headcount,
    SUM(salary)         AS total_salary,
    AVG(salary)         AS avg_salary,
    MIN(salary)         AS min_salary,
    MAX(salary)         AS max_salary,
    MAX(salary) - MIN(salary) AS salary_range
FROM employees
GROUP BY dept_id;
```

**Interview Q: What does AVG return when all values are NULL?**  
A: AVG returns NULL when all values in the column are NULL, since AVG ignores NULLs in its calculation and dividing by zero rows yields NULL.

## 5.3 GROUP BY

```sql
-- Basic GROUP BY
SELECT dept_id, COUNT(*) AS headcount
FROM employees
GROUP BY dept_id;

-- GROUP BY multiple columns
SELECT dept_id, job_title, AVG(salary) AS avg_sal
FROM employees
GROUP BY dept_id, job_title;

-- GROUP BY with expression
SELECT YEAR(hire_date) AS hire_year, COUNT(*) AS hires
FROM employees
GROUP BY YEAR(hire_date)
ORDER BY hire_year;

-- ROLLUP: adds subtotals and grand total
SELECT dept_id, job_title, SUM(salary)
FROM employees
GROUP BY ROLLUP(dept_id, job_title);

-- CUBE: all possible grouping combinations
SELECT dept_id, job_title, SUM(salary)
FROM employees
GROUP BY CUBE(dept_id, job_title);

-- GROUPING SETS: custom grouping combinations
SELECT dept_id, job_title, SUM(salary)
FROM employees
GROUP BY GROUPING SETS((dept_id), (job_title), ());
```

**Rule:** Every column in SELECT that is NOT inside an aggregate function MUST appear in GROUP BY.

## 5.4 HAVING

```sql
-- HAVING filters AFTER aggregation
-- WHERE filters BEFORE aggregation

-- Departments with more than 5 employees
SELECT dept_id, COUNT(*) AS headcount
FROM employees
WHERE status = 'Active'          -- filters before grouping
GROUP BY dept_id
HAVING COUNT(*) > 5;             -- filters after grouping

-- Interview classic: departments with avg salary > 70000
SELECT dept_id, AVG(salary) AS avg_salary
FROM employees
GROUP BY dept_id
HAVING AVG(salary) > 70000
ORDER BY avg_salary DESC;
```

**Interview Q: Can you use WHERE instead of HAVING?**  
A: No — WHERE cannot reference aggregate functions because it filters rows before GROUP BY runs. HAVING filters the aggregated results after GROUP BY. However, conditions on non-aggregated columns can go in WHERE (which is more efficient since it reduces rows before aggregation).

**Common Mistake:**
```sql
-- WRONG: WHERE cannot reference aggregate
SELECT dept_id, AVG(salary) FROM employees 
WHERE AVG(salary) > 70000  -- ERROR
GROUP BY dept_id;

-- CORRECT: Use HAVING
SELECT dept_id, AVG(salary) FROM employees 
GROUP BY dept_id
HAVING AVG(salary) > 70000;
```
