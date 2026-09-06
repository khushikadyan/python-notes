# SQL INTERVIEW PREPARATION — MASTER INDEX
## Complete Guide for Data Analyst, Business Analyst, Data Engineer, BI Developer, SDE, Backend, Database Developer

---

## FILES IN THIS SERIES

| File | Contents |
|---|---|
| `SQL_Interview_Prep_Part1_Fundamentals.md` | Sections 1–5: DB Concepts, Keys, Constraints, NULL, Data Types, DDL/DML/DCL/TCL, Filtering, All Joins, Aggregations |
| `SQL_Interview_Prep_Part2_Advanced.md` | Sections 6–16: Subqueries, CTEs, Temp Tables, Views, Stored Procedures, Triggers, Window Functions, 13 Classic Query Challenges, Indexing, Query Optimization, Normalization |
| `SQL_Interview_Prep_Part3_Scenarios_100Q.md` | Sections 17–20: 100 Real Interview Scenarios by Company, Company-Wise Q&A (Amazon/MS/Google/Oracle), 100 Practice Problems with Solutions, Mock Interviews |
| `SQL_Interview_Prep_Part4_CheatSheets_Revision.md` | Sections 21–25: Complete Cheat Sheets, 1/3/7-Day Revision Plans, Hyderabad & Bangalore Focus, Top 50 Q&A Quick Reference, Practice Schema |

---

## TOPIC INDEX

### Fundamentals
- DBMS vs RDBMS → Part1 §1.1
- Primary Key, Foreign Key, Unique, Composite, Candidate, Super Key → Part1 §1.2
- Constraints (NOT NULL, CHECK, DEFAULT, UNIQUE, FK) → Part1 §1.3
- NULL Handling, COALESCE, NULLIF, ISNULL → Part1 §1.4
- Data Types (INT, VARCHAR, DECIMAL, DATE) → Part1 §1.5

### SQL Commands
- DDL (CREATE, ALTER, DROP, TRUNCATE) → Part1 §2.1
- DML (SELECT, INSERT, UPDATE, DELETE) → Part1 §2.2
- DCL (GRANT, REVOKE) → Part1 §2.3
- TCL (BEGIN, COMMIT, ROLLBACK, SAVEPOINT), ACID → Part1 §2.4

### Filtering & Sorting
- WHERE, ORDER BY, LIMIT/TOP, DISTINCT → Part1 §3.1-3.4
- BETWEEN, IN/NOT IN, LIKE, IS NULL, CASE WHEN → Part1 §3.5-3.8

### Joins
- INNER JOIN → Part1 §4.1
- LEFT JOIN → Part1 §4.2
- RIGHT JOIN → Part1 §4.3
- FULL OUTER JOIN → Part1 §4.4
- CROSS JOIN → Part1 §4.5
- SELF JOIN → Part1 §4.6
- Join Performance → Part1 §4.7

### Aggregations
- COUNT, SUM, AVG, MIN, MAX → Part1 §5.1-5.2
- GROUP BY (with ROLLUP, CUBE, GROUPING SETS) → Part1 §5.3
- HAVING → Part1 §5.4

### Advanced SQL
- Subqueries (scalar, correlated, EXISTS vs IN) → Part2 §6
- CTEs, Recursive CTEs → Part2 §7
- Temp Tables vs Table Variables → Part2 §8
- Views, Materialized Views → Part2 §9
- Stored Procedures, Functions → Part2 §10
- Triggers (AFTER, BEFORE, INSTEAD OF) → Part2 §11

### Window Functions
- ROW_NUMBER, RANK, DENSE_RANK, NTILE → Part2 §12.2
- LEAD, LAG → Part2 §12.4
- FIRST_VALUE, LAST_VALUE → Part2 §12.5
- Running Totals, Moving Averages → Part2 §12.6
- ROWS vs RANGE frame → Part2 §12.7

### Classic Query Challenges
- 2nd/Nth Highest Salary → Part2 §13.1-13.2
- Duplicate Detection & Removal → Part2 §13.3-13.4
- Running Totals → Part2 §13.5
- Gaps and Islands / Consecutive Records → Part2 §13.6
- Top N Per Group → Part2 §13.7
- Latest Record Per Customer → Part2 §13.8
- Department-Wise Highest Salary → Part2 §13.9
- Customers With No Orders → Part2 §13.10
- YoY / MoM Analysis → Part2 §13.11

### Indexing
- Clustered Index → Part2 §14.1
- Non-Clustered Index → Part2 §14.2
- Composite Index, Leftmost Prefix Rule → Part2 §14.3
- Covering Index → Part2 §14.4

### Query Optimization
- Execution Plans (Seek vs Scan vs Lookup) → Part2 §15.1
- Optimization Techniques → Part2 §15.2
- Table Partitioning → Part2 §15.3

### Database Design
- 1NF, 2NF, 3NF, BCNF → Part2 §16.1-16.4
- Star vs Snowflake Schema → Part2 §16.5

### Interview Scenarios
- Amazon / E-Commerce (A1-A10) → Part3 §17A
- Microsoft / Google Analytics (B1-B5) → Part3 §17B
- Finance / Banking (C1-C5) → Part3 §17C
- Uber / Ride Sharing (D1-D3) → Part3 §17D
- HR / Employee Management (E1-E5) → Part3 §17E
- Inventory / Warehousing (F1-F2) → Part3 §17F
- Social Media (G1-G3) → Part3 §17G
- Hard General (H1-H5) → Part3 §17H

### Company-Wise Questions
- Amazon → Part3 §18.1 (Easy/Medium/Hard + RFM query)
- Microsoft → Part3 §18.2
- Google / Meta → Part3 §18.3
- Oracle / SAP → Part3 §18.4

### Practice Problems
- Easy (1-30) → Part3 §19 Easy
- Medium (31-70) → Part3 §19 Medium
- Hard (71-100) → Part3 §19 Hard

### Mock Interviews
- Junior Data Analyst → Part3 §20 Mock 1
- Senior Data Analyst → Part3 §20 Mock 2
- Business Analyst → Part3 §20 Mock 3

### Cheat Sheets
- SQL Syntax + Execution Order → Part4 §21.1
- Window Functions → Part4 §21.2
- Joins → Part4 §21.3
- Aggregation → Part4 §21.4
- Query Optimization → Part4 §21.5
- NULL Handling → Part4 §21.6
- Date Functions (SQL Server, MySQL, PostgreSQL) → Part4 §21.7

### Revision Plans
- 1-Day Revision → Part4 §22.1
- 3-Day Revision → Part4 §22.2
- 7-Day Revision Plan → Part4 §22.3

### Location-Specific
- Hyderabad Interview Focus (TCS, Infosys, Amazon, Microsoft) → Part4 §23.1
- Bangalore Interview Focus (Flipkart, Swiggy, Ola, Razorpay) → Part4 §23.2
- Food Delivery App Schema + Queries → Part4 §23.3
- SQL Gotchas & Tricks → Part4 §23.4

### Quick Reference
- Top 50 Q&A → Part4 §24

---

## QUICK STUDY PATHS BY ROLE

### Data Analyst / Business Analyst
Part1 (all) → Part2 §6,7,9,12,13 → Part3 (Category A,B,E, Easy+Medium problems) → Part4 (Cheat Sheets + Revision)

### Data Engineer
Part1 (all) → Part2 (all) → Part3 (Category H, Hard problems) → Part4 §23 (SCD Type 2, Partitioning)

### BI Developer
Part1 §1-4 → Part2 §7,9,12 → Part3 (Category B,D pivot queries) → Part4 §21.2 (window cheatsheet)

### Software Engineer / Backend Developer  
Part1 §1.2,1.3,2 → Part2 §8,10,11,14,15 → Part3 (Hard 71-100) → Part4 §21.5 (optimization cheatsheet)

### Database Developer / DBA
Part1 (all) → Part2 (all) → Part4 §21 (all cheat sheets) → Part4 §23 (indexing, partitioning deep dive)


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
