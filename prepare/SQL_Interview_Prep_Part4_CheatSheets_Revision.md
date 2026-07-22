# SQL INTERVIEW PREPARATION — COMPLETE GUIDE
## Part 4: Cheat Sheets, Revision Notes & Hyderabad/Bangalore Focus

---

# SECTION 21: COMPLETE CHEAT SHEETS

---

## 21.1 SQL SYNTAX CHEAT SHEET

### Core SELECT Syntax (Execution Order)
```sql
SELECT [DISTINCT] column(s) / expression(s) / aggregate(s)    -- 5: What to return
FROM table_name                                                 -- 1: Which table
[JOIN table2 ON condition]                                      -- 2: Combine tables
[WHERE row_filter_condition]                                    -- 3: Filter rows
[GROUP BY column(s)]                                           -- 4: Group for aggregation
[HAVING group_filter_condition]                                 -- 5: Filter groups
[ORDER BY column(s) [ASC|DESC]]                               -- 6: Sort result
[LIMIT n / TOP n / FETCH NEXT n ROWS ONLY]                    -- 7: Limit output
```

**Execution Order:**  
FROM → JOIN → WHERE → GROUP BY → HAVING → SELECT → DISTINCT → ORDER BY → LIMIT

### DDL
```sql
CREATE TABLE t (col datatype [constraint], ...);
ALTER TABLE t ADD col datatype;
ALTER TABLE t DROP COLUMN col;
ALTER TABLE t ALTER COLUMN col new_datatype;
DROP TABLE t;
TRUNCATE TABLE t;
CREATE [UNIQUE] INDEX idx ON t(col1, col2);
DROP INDEX idx [ON t];
CREATE VIEW v AS SELECT ...;
CREATE OR REPLACE VIEW v AS SELECT ...;
```

### DML
```sql
INSERT INTO t [(cols)] VALUES (v1, v2, ...);
INSERT INTO t SELECT ... FROM ...;
UPDATE t SET col1=val1 [, col2=val2] [WHERE cond];
DELETE FROM t [WHERE cond];
```

### Joins Quick Reference
```sql
INNER JOIN  t2 ON t1.id = t2.id      -- matching rows only
LEFT JOIN   t2 ON t1.id = t2.id      -- all left + matching right (NULLs for non-match)
RIGHT JOIN  t2 ON t1.id = t2.id      -- all right + matching left
FULL OUTER JOIN t2 ON t1.id = t2.id  -- all from both (NULLs where no match)
CROSS JOIN  t2                        -- cartesian product
```

### Set Operations
```sql
SELECT ... UNION     SELECT ...  -- distinct rows from both
SELECT ... UNION ALL SELECT ...  -- all rows from both (including duplicates)
SELECT ... INTERSECT SELECT ...  -- rows in BOTH queries
SELECT ... EXCEPT    SELECT ...  -- rows in first but NOT second
-- All require same number of columns and compatible types
```

---

## 21.2 WINDOW FUNCTIONS CHEAT SHEET

```
FUNCTION() OVER ([PARTITION BY cols] [ORDER BY cols] [ROWS/RANGE BETWEEN ... AND ...])
```

### Ranking Functions
| Function | Ties | Gaps | Use when |
|---|---|---|---|
| ROW_NUMBER() | Unique (arbitrary) | No | Need unique row ID |
| RANK() | Same rank | Yes (1,1,3) | Olympic-style ranking |
| DENSE_RANK() | Same rank | No (1,1,2) | Top-N with ties included |
| NTILE(n) | Split into n buckets | — | Quartiles, deciles, percentiles |

### Value Functions
```sql
LAG(col, n, default)  OVER (...)  -- nth previous row value
LEAD(col, n, default) OVER (...)  -- nth next row value
FIRST_VALUE(col)      OVER (...)  -- first value in window frame
LAST_VALUE(col)       OVER (... ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING)
NTH_VALUE(col, n)     OVER (...)  -- nth value in window
```

### Aggregate as Window
```sql
SUM(col)   OVER (PARTITION BY grp ORDER BY dt ROWS UNBOUNDED PRECEDING)  -- running total
AVG(col)   OVER (PARTITION BY grp ORDER BY dt ROWS BETWEEN 2 PRECEDING AND CURRENT ROW)  -- 3-day avg
COUNT(*)   OVER (PARTITION BY grp)  -- group size without collapsing rows
MAX(col)   OVER (PARTITION BY grp)  -- group max on every row
```

### Distribution Functions
```sql
PERCENT_RANK()  OVER (ORDER BY col)  -- 0.0 to 1.0 relative rank
CUME_DIST()     OVER (ORDER BY col)  -- cumulative distribution 0.0 to 1.0
```

### Frame Syntax
```
ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW    -- from start to current row
ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING            -- current + neighbors
ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING  -- entire partition
RANGE BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING   -- current to end
```

---

## 21.3 JOINS CHEAT SHEET

```
INNER JOIN:          A ∩ B (intersection)
LEFT JOIN:           A (all) + A ∩ B
RIGHT JOIN:          B (all) + A ∩ B
FULL OUTER JOIN:     A ∪ B (union, with NULLs)
LEFT ANTI-JOIN:      A - B (A only, not in B)
RIGHT ANTI-JOIN:     B - A (B only, not in A)
CROSS JOIN:          A × B (cartesian product)
```

### Common Patterns
```sql
-- Left Anti-Join (rows in A but not B)
SELECT a.* FROM a LEFT JOIN b ON a.id = b.id WHERE b.id IS NULL;

-- Right Anti-Join (rows in B but not A)
SELECT b.* FROM a RIGHT JOIN b ON a.id = b.id WHERE a.id IS NULL;

-- Find Orphan Records
SELECT * FROM child_table c LEFT JOIN parent_table p ON c.fk = p.pk WHERE p.pk IS NULL;

-- Self Join for hierarchy
SELECT e.name, m.name AS manager FROM employees e LEFT JOIN employees m ON e.mgr_id = m.emp_id;

-- Unmatched from both sides (Full Outer where at least one is NULL)
SELECT * FROM a FULL OUTER JOIN b ON a.id = b.id WHERE a.id IS NULL OR b.id IS NULL;
```

---

## 21.4 AGGREGATION CHEAT SHEET

```sql
COUNT(*)              -- all rows
COUNT(col)            -- non-NULL only
COUNT(DISTINCT col)   -- unique non-NULL
SUM(col)              -- sum of non-NULLs
AVG(col)              -- mean of non-NULLs (sum/count of non-NULLs)
MIN(col)              -- minimum non-NULL
MAX(col)              -- maximum non-NULL
STRING_AGG(col, ',')  -- concatenate (SQL Server 2017+ / PostgreSQL)
GROUP_CONCAT(col)     -- MySQL equivalent of STRING_AGG
```

### Conditional Aggregation (Pivot Pattern)
```sql
SUM(CASE WHEN condition THEN col ELSE 0 END)
COUNT(CASE WHEN condition THEN 1 END)  -- NULL is ignored by COUNT
AVG(CASE WHEN condition THEN col END)  -- NULL excluded from AVG
MAX(CASE WHEN condition THEN col END)
```

### ROLLUP / CUBE
```sql
GROUP BY ROLLUP(a, b)        -- (a,b), (a), ()
GROUP BY CUBE(a, b)          -- (a,b), (a,NULL), (NULL,b), ()
GROUP BY GROUPING SETS((a),(b))  -- (a), (b) only
GROUPING(col)                -- returns 1 if col is NULL due to rollup/cube
```

---

## 21.5 QUERY OPTIMIZATION CHEAT SHEET

### DO's ✓
```sql
-- Index the JOIN and WHERE columns
CREATE INDEX idx ON orders(customer_id, order_date);

-- Use sargable predicates (index-friendly)
WHERE order_date >= '2024-01-01'  -- sargable
WHERE YEAR(order_date) = 2024     -- NOT sargable (function prevents index use)

-- Filter early
FROM (SELECT * FROM orders WHERE status='Active') o JOIN ...

-- Use EXISTS for existence checks
WHERE EXISTS (SELECT 1 FROM orders WHERE customer_id = c.id)

-- Cover queries with indexes
CREATE INDEX idx ON orders(customer_id) INCLUDE (order_date, amount)
```

### DON'Ts ✗
```sql
SELECT *                              -- fetch only needed columns
WHERE UPPER(name) = 'ALICE'          -- function on indexed column
WHERE col IS NOT NULL AND col != ''  -- implicit type conversion
NOT IN (subquery returning NULLs)    -- returns 0 rows!
OR on different indexed columns       -- use UNION instead
Correlated subquery in SELECT/WHERE  -- use JOIN instead
```

### Hints (SQL Server)
```sql
SELECT * FROM orders WITH (NOLOCK)    -- dirty read, no shared lock
SELECT * FROM orders WITH (INDEX(idx_customer)) -- force specific index
OPTION (RECOMPILE)                    -- recompile for optimal plan each time
OPTION (MAXDOP 4)                     -- limit parallel threads
OPTION (HASH JOIN)                    -- force hash join algorithm
```

---

## 21.6 NULL HANDLING CHEAT SHEET

```sql
IS NULL                    -- check for NULL
IS NOT NULL                -- check for not NULL
COALESCE(a, b, c)         -- first non-NULL value (ANSI standard)
ISNULL(a, b)              -- SQL Server: return b if a is NULL
IFNULL(a, b)              -- MySQL: return b if a is NULL
NVL(a, b)                 -- Oracle: return b if a is NULL
NULLIF(a, b)              -- return NULL if a = b (else return a)
                           -- use to prevent division by zero: NULLIF(denominator, 0)

-- NULL in aggregates
COUNT(*)        -- counts NULLs
COUNT(col)      -- ignores NULLs
SUM/AVG/MIN/MAX -- all ignore NULLs

-- NULL in comparisons
NULL = NULL    → UNKNOWN  (use IS NULL)
NULL <> NULL   → UNKNOWN
1 + NULL       → NULL
'' = NULL      → UNKNOWN  (empty string ≠ NULL)

-- NULL in NOT IN
col NOT IN (1, 2, NULL)  → always returns 0 rows (UNKNOWN propagates)
-- Use NOT EXISTS instead
```

---

## 21.7 DATE FUNCTIONS CHEAT SHEET

### SQL Server
```sql
GETDATE()                          -- current datetime
GETUTCDATE()                       -- current UTC datetime
SYSDATETIME()                      -- higher precision
DATEADD(unit, n, date)             -- add n units to date
DATEDIFF(unit, start, end)         -- difference between dates
DATEPART(unit, date)               -- extract part (int)
DATENAME(unit, date)               -- extract part (string)
YEAR(date), MONTH(date), DAY(date) -- shortcuts
FORMAT(date, 'yyyy-MM-dd')         -- format as string
CAST('2024-01-15' AS DATE)         -- parse string to date
EOMONTH(date)                      -- last day of month
DATEFROMPARTS(y, m, d)             -- build date from parts

-- Units: YEAR, QUARTER, MONTH, WEEK, DAY, HOUR, MINUTE, SECOND
```

### MySQL
```sql
NOW(), CURDATE(), CURTIME()
DATE_ADD(date, INTERVAL n unit)
DATEDIFF(date1, date2)            -- date1 - date2 in days
DATE_FORMAT(date, '%Y-%m-%d')
YEAR(), MONTH(), DAY()
LAST_DAY(date)
STR_TO_DATE('01-15-2024', '%m-%d-%Y')
```

### PostgreSQL
```sql
NOW(), CURRENT_DATE, CURRENT_TIME
date + INTERVAL '1 month'
date1 - date2                     -- returns interval
EXTRACT(YEAR FROM date)
TO_CHAR(date, 'YYYY-MM-DD')
DATE_TRUNC('month', date)         -- truncate to month start
```

---

# SECTION 22: REVISION SHEETS

---

## 22.1 ONE-DAY REVISION SHEET

### Must-Know Concepts
1. **Keys:** PK = unique + NOT NULL. FK = referential integrity. UK = unique + allows 1 NULL.
2. **NULL:** Use IS NULL not = NULL. COALESCE for defaults. NOT IN with NULLs returns empty!
3. **Execution Order:** FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY → LIMIT
4. **DROP vs TRUNCATE vs DELETE:** DROP removes table. TRUNCATE removes data fast (DDL). DELETE is slow, logged, supports WHERE.
5. **INNER vs LEFT JOIN:** INNER = matching only. LEFT = all from left + NULLs for no match.
6. **WHERE vs HAVING:** WHERE filters rows (before grouping). HAVING filters groups (after GROUP BY).
7. **Subquery types:** Scalar (1 value), Row (1 row), Table (multiple). Correlated = references outer query.
8. **Window functions:** Don't collapse rows. OVER(PARTITION BY ... ORDER BY ...). ROW_NUMBER vs RANK vs DENSE_RANK.
9. **Index:** Clustered = physical order, 1 per table. Non-clustered = separate structure, multiple allowed.
10. **CTE:** WITH cte AS (query) SELECT... Recursive CTE = anchor + UNION ALL + recursive member.

### 5 Most Common Interview Queries
```sql
-- 1. Nth highest salary
SELECT salary FROM (SELECT salary, DENSE_RANK() OVER (ORDER BY salary DESC) AS dr FROM employees) t WHERE dr = N;

-- 2. Duplicate records
SELECT email, COUNT(*) FROM employees GROUP BY email HAVING COUNT(*) > 1;

-- 3. Employees with no order / no dept
SELECT * FROM employees e LEFT JOIN departments d ON e.dept_id = d.dept_id WHERE d.dept_id IS NULL;

-- 4. Top N per group
SELECT * FROM (SELECT *, DENSE_RANK() OVER (PARTITION BY dept_id ORDER BY salary DESC) AS dr FROM employees) t WHERE dr <= 3;

-- 5. Running total
SELECT date, SUM(amount) OVER (ORDER BY date ROWS UNBOUNDED PRECEDING) AS running_total FROM sales;
```

---

## 22.2 THREE-DAY REVISION SHEET

### Day 1: Fundamentals + Joins + Aggregations

**Keys & Constraints** — PK, FK, UK, CK, Candidate, Super, CHECK, DEFAULT  
**NULL Handling** — IS NULL, COALESCE, NULLIF, behavior in aggregates and NOT IN  
**DDL vs DML vs DCL vs TCL** — know all commands  
**All JOIN types** — with Venn diagrams in your head, know Anti-join pattern  
**Aggregate functions** — COUNT(*) vs COUNT(col), SUM, AVG, MIN, MAX  
**GROUP BY + HAVING** — know execution order, common mistakes  

**Practice queries:**
- Find departments with avg salary > 70K
- Find customers with no orders (LEFT JOIN + IS NULL)
- Count NULL vs non-NULL values
- Employees earning above dept average (correlated subquery)

### Day 2: Advanced SQL — Subqueries, CTEs, Window Functions

**Subqueries** — scalar, correlated, EXISTS vs IN  
**CTEs** — multi-CTE, recursive CTE for hierarchies  
**Window Functions** — ROW_NUMBER, RANK, DENSE_RANK, LAG, LEAD, SUM OVER, NTILE  
**Classic Patterns** — Top N per group, running total, consecutive records, gap analysis  

**Practice queries:**
- Second/Nth highest salary (3 methods)
- Delete duplicate records
- Running sales total
- Consecutive login days
- Latest order per customer

### Day 3: Indexing + Optimization + Design + Mock Questions

**Index types** — Clustered vs Non-Clustered, Composite, Covering  
**Execution plans** — Index Seek vs Scan, Key Lookup, Hash/Merge/Nested Loops  
**Optimization rules** — sargable predicates, avoid SELECT *, covering indexes, EXISTS vs IN  
**Normalization** — 1NF, 2NF, 3NF, BCNF with examples  
**Star vs Snowflake** — know when to use each  
**Company-specific scenarios** — practice 5 from Amazon, 5 from finance category  

---

## 22.3 SEVEN-DAY REVISION SHEET

| Day | Topics | Practice |
|---|---|---|
| 1 | DB fundamentals, Keys, Constraints, Data Types | 20 easy problems |
| 2 | SQL commands (DDL/DML/DCL/TCL), Filtering, Sorting | 15 medium problems |
| 3 | All Joins (INNER/LEFT/RIGHT/FULL/CROSS/SELF) | Join-specific 20 problems |
| 4 | Aggregations, Subqueries, CTEs, Recursive CTEs | Classic patterns: duplicates, Nth highest |
| 5 | Window Functions, Running totals, Gaps & Islands | 20 window function problems |
| 6 | Indexing, Query Optimization, Execution Plans | Rewrite slow queries |
| 7 | Full mock interviews + company-specific scenarios | 3 complete mock tests |

---

# SECTION 23: HYDERABAD & BANGALORE INTERVIEW FOCUS

---

## 23.1 Most Frequently Asked in Hyderabad (TCS, Infosys, Wipro, HCL, Accenture)

### Level 1 (Service Company Screening)
```sql
-- These are almost always asked:
-- 1. What is a Primary Key? Can it have NULL?
-- 2. Difference between WHERE and HAVING
-- 3. What is a JOIN? Types of JOINs?
-- 4. Find second highest salary
-- 5. What is a view? How is it different from a table?
-- 6. What is normalization? 1NF, 2NF, 3NF?
-- 7. DELETE vs TRUNCATE vs DROP
-- 8. What is an index? Types?
-- 9. What is a stored procedure?
-- 10. What are ACID properties?
```

### Level 2 (Technical Rounds at Product Companies in Hyderabad)
**Companies:** Amazon Hyderabad, Microsoft Hyderabad, Oracle Hyderabad, Deloitte, Qualcomm, Nvidia

```sql
-- These are commonly asked:
-- 1. Write a recursive CTE to find manager hierarchy
-- 2. Find top 3 salaries per department
-- 3. Running sum / cumulative revenue
-- 4. Detect and remove duplicate records
-- 5. Find customers who bought every product in a category
-- 6. Calculate month-over-month growth
-- 7. Design normalized tables for given scenario (e.g., hospital management)
-- 8. Explain execution plan, how would you optimize this query?
-- 9. What is a correlated subquery? Give an example.
-- 10. When would you use a CTE vs subquery vs temp table?
```

### Amazon Hyderabad SQL Specialties
- Focus on high data volume scenarios (millions of rows)
- Emphasis on correct handling of NULLs
- Always ask about alternate approaches
- Want to see awareness of performance (indexes, execution plans)
- Behavioral: "Tell me about a time you optimized a slow query"

---

## 23.2 Most Frequently Asked in Bangalore (Flipkart, Swiggy, Ola, Myntra, Nykaa, Razorpay)

### Startup/Unicorn SQL Focus (E-Commerce, FinTech)
```sql
-- Real business problems they ask:
-- 1. User cohort retention analysis (see Section 17.B1)
-- 2. RFM customer segmentation
-- 3. Identify fraud patterns in transactions
-- 4. Calculate Day-1, Day-7, Day-30 retention
-- 5. Find the most popular search-to-purchase funnel
-- 6. Detect session-based user behavior (session window analysis)
-- 7. Product recommendation: items bought together
-- 8. Supply chain: find products with stockout risk
-- 9. Delivery partner performance analysis
-- 10. AB test result analysis

-- Bangalore product companies also ask:
-- Schema design for given system (Uber, food delivery, etc.)
-- "How would you design the data model for our order management system?"
```

### Data Engineer Roles (Bangalore Focus)
```sql
-- ETL and pipeline-specific SQL questions:
-- 1. How do you handle SCD (Slowly Changing Dimensions)?
-- 2. What is partitioning and when do you use it?
-- 3. How do you incrementally load data?
-- 4. Explain the difference between OLTP and OLAP schema design
-- 5. How do you handle late-arriving data in a data warehouse?

-- SCD Type 2 Example:
CREATE TABLE dim_customer (
    customer_sk     INT IDENTITY PRIMARY KEY,   -- surrogate key
    customer_id     INT,                         -- natural key
    customer_name   VARCHAR(100),
    city            VARCHAR(50),
    effective_date  DATE NOT NULL,
    expiry_date     DATE,                        -- NULL = current record
    is_current      BIT DEFAULT 1
);

-- When customer city changes (SCD Type 2):
-- 1. UPDATE old record: set expiry_date = today - 1, is_current = 0
-- 2. INSERT new record with new city, effective_date = today, is_current = 1
```

---

## 23.3 Scenario-Based Design Questions (Common in Both Cities)

### Design the Data Model for a Food Delivery App (Swiggy/Zomato)

```sql
CREATE TABLE restaurants (
    restaurant_id INT PRIMARY KEY,
    name          VARCHAR(200),
    city          VARCHAR(100),
    rating        DECIMAL(3,2),
    is_active     BIT
);

CREATE TABLE menu_items (
    item_id       INT PRIMARY KEY,
    restaurant_id INT REFERENCES restaurants(restaurant_id),
    item_name     VARCHAR(200),
    price         DECIMAL(10,2),
    category      VARCHAR(100),
    is_available  BIT
);

CREATE TABLE customers (
    customer_id   INT PRIMARY KEY,
    name          VARCHAR(100),
    phone         VARCHAR(15) UNIQUE,
    city          VARCHAR(100)
);

CREATE TABLE delivery_partners (
    partner_id    INT PRIMARY KEY,
    name          VARCHAR(100),
    vehicle_type  VARCHAR(50),
    rating        DECIMAL(3,2),
    city          VARCHAR(100)
);

CREATE TABLE orders (
    order_id      INT PRIMARY KEY,
    customer_id   INT REFERENCES customers(customer_id),
    restaurant_id INT REFERENCES restaurants(restaurant_id),
    partner_id    INT REFERENCES delivery_partners(partner_id),
    order_time    DATETIME,
    delivery_time DATETIME,
    status        VARCHAR(50),
    total_amount  DECIMAL(10,2),
    delivery_fee  DECIMAL(10,2)
);

CREATE TABLE order_items (
    order_id   INT REFERENCES orders(order_id),
    item_id    INT REFERENCES menu_items(item_id),
    quantity   INT,
    unit_price DECIMAL(10,2),
    PRIMARY KEY (order_id, item_id)
);
```

**Analysis Queries:**
```sql
-- 1. Average delivery time by city
SELECT r.city, AVG(DATEDIFF(MINUTE, o.order_time, o.delivery_time)) AS avg_delivery_mins
FROM orders o JOIN restaurants r ON o.restaurant_id = r.restaurant_id
WHERE o.delivery_time IS NOT NULL
GROUP BY r.city ORDER BY avg_delivery_mins;

-- 2. Top 5 restaurants by orders this week
SELECT TOP 5 r.name, COUNT(*) AS order_count
FROM orders o JOIN restaurants r ON o.restaurant_id = r.restaurant_id
WHERE o.order_time >= DATEADD(WEEK, -1, GETDATE())
GROUP BY r.restaurant_id, r.name ORDER BY order_count DESC;

-- 3. Partner performance: orders, avg rating, avg delivery time
SELECT dp.name, COUNT(*) AS total_orders,
       AVG(dp.rating) AS avg_rating,
       AVG(DATEDIFF(MINUTE, o.order_time, o.delivery_time)) AS avg_delivery_mins
FROM delivery_partners dp JOIN orders o ON dp.partner_id = o.partner_id
GROUP BY dp.partner_id, dp.name ORDER BY avg_delivery_mins;

-- 4. Customer reorder rate (placed 2+ orders)
SELECT COUNT(*) AS total_customers,
       SUM(CASE WHEN order_count >= 2 THEN 1 ELSE 0 END) AS repeat_customers,
       ROUND(SUM(CASE WHEN order_count >= 2 THEN 1 ELSE 0 END) * 100.0 / COUNT(*), 2) AS reorder_rate
FROM (SELECT customer_id, COUNT(*) AS order_count FROM orders GROUP BY customer_id) c;
```

---

## 23.4 Commonly Asked SQL Tricks & Gotchas

### Trick 1: NULL in NOT IN
```sql
-- This returns 0 rows even if customer_id = 999 exists in customers!
SELECT * FROM customers WHERE customer_id NOT IN (1, 2, NULL);
-- BECAUSE: 999 NOT IN (1, 2, NULL) = (999 <> 1 AND 999 <> 2 AND 999 <> NULL)
--        = (TRUE AND TRUE AND UNKNOWN) = UNKNOWN
-- Rows where condition is UNKNOWN are excluded, so 0 rows returned.
-- SAFE: WHERE customer_id NOT IN (1, 2) -- no NULLs in list
-- OR: WHERE NOT EXISTS (SELECT 1 FROM ...)
```

### Trick 2: COUNT(DISTINCT) vs COUNT + DISTINCT
```sql
SELECT COUNT(DISTINCT dept_id) FROM employees;  -- distinct dept count
SELECT DISTINCT COUNT(*) FROM employees;         -- just the total row count (DISTINCT has no effect here)
```

### Trick 3: Window Function vs GROUP BY
```sql
-- GROUP BY collapses rows:
SELECT dept_id, AVG(salary) FROM employees GROUP BY dept_id;
-- 4 rows (one per dept)

-- Window function keeps all rows:
SELECT emp_name, dept_id, salary, AVG(salary) OVER (PARTITION BY dept_id) AS dept_avg
FROM employees;
-- All rows, each with dept avg attached
```

### Trick 4: BETWEEN is inclusive on both ends
```sql
WHERE salary BETWEEN 50000 AND 80000
-- = WHERE salary >= 50000 AND salary <= 80000
-- 80000 IS included!
```

### Trick 5: ORDER BY in window without PARTITION BY
```sql
-- Running total across ALL rows (no partition)
SUM(amount) OVER (ORDER BY order_date ROWS UNBOUNDED PRECEDING)

-- Running total per customer (with partition)
SUM(amount) OVER (PARTITION BY customer_id ORDER BY order_date ROWS UNBOUNDED PRECEDING)
```

### Trick 6: RANK after DENSE_RANK tie
```sql
-- Data: 100, 90, 90, 80
ROW_NUMBER: 1, 2, 3, 4
RANK:       1, 2, 2, 4  <-- gap after tie
DENSE_RANK: 1, 2, 2, 3  <-- no gap
```

### Trick 7: Implicit vs Explicit Joins
```sql
-- Implicit JOIN (old ANSI syntax, avoid)
SELECT * FROM e, d WHERE e.dept_id = d.dept_id;
-- If you forget WHERE: CROSS JOIN accidentally!

-- Explicit JOIN (preferred, safer)
SELECT * FROM employees e INNER JOIN departments d ON e.dept_id = d.dept_id;
```

---

# SECTION 24: TOP 50 INTERVIEW Q&A QUICK REFERENCE

---

1. **What is SQL?** — Structured Query Language for managing and querying relational databases.

2. **DBMS vs RDBMS?** — DBMS stores files without formal relationships; RDBMS uses tables with FK constraints and supports ACID, SQL.

3. **Primary Key vs Unique Key?** — PK: NOT NULL + UNIQUE, one per table. UK: UNIQUE, allows one NULL, multiple per table.

4. **DELETE vs TRUNCATE vs DROP?** — DELETE: logged, conditional, rollback-able. TRUNCATE: DDL, removes all data fast, resets identity. DROP: removes table entirely.

5. **WHERE vs HAVING?** — WHERE filters rows before grouping; HAVING filters groups after GROUP BY. HAVING can use aggregates.

6. **INNER JOIN vs LEFT JOIN?** — INNER: only matching rows. LEFT: all left rows + NULLs for non-matching right.

7. **NULL = NULL?** — FALSE. NULL = NULL → UNKNOWN. Use IS NULL to check.

8. **COALESCE vs ISNULL?** — COALESCE is ANSI standard, takes N args. ISNULL is SQL Server only, takes 2 args.

9. **Subquery vs CTE?** — CTEs are more readable, reusable within query, support recursion. Performance is usually similar; CTE is preferred for complex logic.

10. **Correlated subquery?** — References outer query column; executes once per outer row. Can be slow; often rewritable as JOIN or window function.

11. **ROW_NUMBER vs RANK vs DENSE_RANK?** — ROW_NUMBER: always unique. RANK: gaps after ties (1,1,3). DENSE_RANK: no gaps (1,1,2).

12. **Clustered vs Non-Clustered Index?** — Clustered: physical order of data, 1 per table. Non-clustered: separate B-Tree with pointers, multiple allowed.

13. **When to avoid indexes?** — Small tables, low-cardinality columns, columns with many NULLs, tables with frequent writes.

14. **Covering index?** — Index that satisfies entire query (all needed columns), eliminating base table lookup.

15. **1NF, 2NF, 3NF?** — 1NF: atomic values, no repeating groups. 2NF: no partial dependencies (remove if PK is composite). 3NF: no transitive dependencies.

16. **Star vs Snowflake schema?** — Star: denormalized dims, fast queries, more storage. Snowflake: normalized dims, slower queries, less storage.

17. **View vs Materialized View?** — View: virtual, runs query every time. Materialized: stores data physically, needs refresh, faster reads.

18. **Stored Procedure vs Function?** — SP: can do DML, transactions; not in SELECT. Function: returns value; usable in SELECT; can't do DML.

19. **Trigger types?** — AFTER (fires after DML), BEFORE (fires before DML), INSTEAD OF (replaces DML, common on views).

20. **ACID properties?** — Atomicity (all or nothing), Consistency (valid state), Isolation (concurrent transactions don't interfere), Durability (committed = permanent).

21. **UNION vs UNION ALL?** — UNION removes duplicates (sorts). UNION ALL keeps duplicates (faster).

22. **EXISTS vs IN?** — EXISTS stops on first match, handles NULLs correctly. IN loads full list; fails with NULLs in NOT IN.

23. **NOT IN with NULL in subquery?** — Returns 0 rows because `x NOT IN (..., NULL)` evaluates to UNKNOWN.

24. **Recursive CTE structure?** — Anchor member (base case) UNION ALL recursive member (references CTE, adds one level per iteration).

25. **LEAD/LAG functions?** — LAG accesses previous row, LEAD accesses next row. Syntax: LAG(col, offset, default) OVER (PARTITION BY... ORDER BY...).

26. **Running total query?** — SUM(col) OVER (ORDER BY date ROWS UNBOUNDED PRECEDING)

27. **Top 3 per group?** — WITH ranked AS (SELECT *, DENSE_RANK() OVER (PARTITION BY grp ORDER BY val DESC) AS dr FROM t) SELECT * FROM ranked WHERE dr <= 3

28. **Find consecutive records (gaps & islands)?** — Subtract ROW_NUMBER from date; same result = consecutive group.

29. **Duplicate detection?** — GROUP BY email HAVING COUNT(*) > 1. Or ROW_NUMBER() OVER (PARTITION BY email ORDER BY id) > 1.

30. **Delete duplicates keeping one?** — WITH cte AS (SELECT *, ROW_NUMBER() OVER (PARTITION BY email ORDER BY id) AS rn FROM t) DELETE FROM cte WHERE rn > 1

31. **Pivot without PIVOT keyword?** — MAX(CASE WHEN col = 'val' THEN value END) as new_col, grouped by row identifier.

32. **SCD Type 2?** — On change: expire old record (set end_date, is_current=0), insert new record with new value and current effective date.

33. **Execution plan operators?** — Index Seek (fast), Index Scan (slower), Table Scan (slowest), Key Lookup (extra trip for non-index columns).

34. **Sargable predicate?** — A WHERE condition that can use an index. Functions on columns make predicates non-sargable.

35. **Isolation levels?** — READ UNCOMMITTED, READ COMMITTED, REPEATABLE READ, SERIALIZABLE (increasing isolation and locking).

36. **NTILE(4)?** — Divides rows into 4 equal buckets (quartiles). NTILE(100) = percentile approximation.

37. **PERCENT_RANK?** — (rank-1)/(rows-1) per group. Returns 0.0 to 1.0. First row always 0.0.

38. **CUME_DIST?** — Cumulative distribution: fraction of rows ≤ current row. Returns 0.0 to 1.0. Last row always 1.0.

39. **Partition vs no partition in OVER()?** — PARTITION BY divides window into groups. Without it, the whole result set is one window.

40. **ROWS vs RANGE in window frame?** — ROWS: physical row count. RANGE: value-based (includes duplicates with same ORDER BY value).

41. **Anti-join pattern?** — LEFT JOIN + WHERE right.id IS NULL. Finds rows in left table not in right table.

42. **Self-join use case?** — Org hierarchy (employee-manager in same table), comparing rows within same table.

43. **Cross join use case?** — Generating all combinations (size × color), date dimension generation, test data.

44. **When is FULL OUTER JOIN used?** — When you need all records from both tables regardless of match — useful for data reconciliation, finding records missing in either side.

45. **Difference between RANK(1) and DENSE_RANK(1)?** — Both give rank 1 to the top record(s). Difference is only visible in lower ranks after ties.

46. **Common query to find Nth highest salary?** — SELECT salary FROM (SELECT salary, DENSE_RANK() OVER (ORDER BY salary DESC) AS dr FROM employees) t WHERE dr = N

47. **How to prevent duplicate inserts?** — Use UNIQUE constraint, or INSERT with NOT EXISTS check, or MERGE (UPSERT).

48. **MERGE statement?** — Combines INSERT, UPDATE, DELETE in one statement. WHEN MATCHED THEN UPDATE; WHEN NOT MATCHED THEN INSERT.

49. **Output clause (SQL Server)?** — Returns affected rows from INSERT/UPDATE/DELETE without a second SELECT. OUTPUT INSERTED.*, OUTPUT DELETED.*.

50. **Query to get the latest N records?** — SELECT TOP N * FROM table ORDER BY date_col DESC; or ROW_NUMBER() OVER (ORDER BY date_col DESC) <= N.

---

# SECTION 25: SAMPLE SQL SCHEMAS FOR PRACTICE

---

```sql
-- Complete practice schema for all scenarios above

CREATE TABLE departments (
    dept_id    INT PRIMARY KEY,
    dept_name  VARCHAR(100) NOT NULL,
    location   VARCHAR(100),
    budget     DECIMAL(15,2)
);

CREATE TABLE employees (
    emp_id      INT PRIMARY KEY,
    emp_name    VARCHAR(100) NOT NULL,
    email       VARCHAR(150) UNIQUE,
    hire_date   DATE,
    salary      DECIMAL(10,2) CHECK (salary > 0),
    bonus       DECIMAL(10,2),
    dept_id     INT REFERENCES departments(dept_id),
    manager_id  INT REFERENCES employees(emp_id),
    job_title   VARCHAR(100),
    status      VARCHAR(20) DEFAULT 'Active'
);

CREATE TABLE customers (
    customer_id   INT PRIMARY KEY,
    customer_name VARCHAR(100) NOT NULL,
    email         VARCHAR(150) UNIQUE,
    phone         VARCHAR(15),
    city          VARCHAR(100),
    country       VARCHAR(100),
    created_at    DATETIME DEFAULT GETDATE()
);

CREATE TABLE products (
    product_id   INT PRIMARY KEY,
    product_name VARCHAR(200) NOT NULL,
    category_id  INT,
    price        DECIMAL(10,2),
    stock        INT DEFAULT 0 CHECK (stock >= 0),
    is_active    BIT DEFAULT 1
);

CREATE TABLE orders (
    order_id     INT PRIMARY KEY,
    customer_id  INT REFERENCES customers(customer_id),
    order_date   DATETIME DEFAULT GETDATE(),
    status       VARCHAR(50),
    total_amount DECIMAL(12,2),
    shipping_address VARCHAR(500)
);

CREATE TABLE order_items (
    order_id    INT REFERENCES orders(order_id),
    product_id  INT REFERENCES products(product_id),
    quantity    INT NOT NULL CHECK (quantity > 0),
    unit_price  DECIMAL(10,2) NOT NULL,
    PRIMARY KEY (order_id, product_id)
);

CREATE TABLE salary_history (
    history_id  INT PRIMARY KEY,
    emp_id      INT REFERENCES employees(emp_id),
    old_salary  DECIMAL(10,2),
    new_salary  DECIMAL(10,2),
    change_date DATE,
    changed_by  VARCHAR(100)
);

-- Sample data
INSERT INTO departments VALUES
(10, 'Engineering', 'Hyderabad', 5000000),
(20, 'Marketing', 'Bangalore', 2000000),
(30, 'Finance', 'Mumbai', 3000000),
(40, 'HR', 'Delhi', 1500000),
(50, 'Operations', 'Pune', 2500000);

INSERT INTO employees VALUES
(1, 'Arjun Sharma',    'arjun@co.com',  '2020-01-15', 95000, 10000, 10, NULL, 'VP Engineering', 'Active'),
(2, 'Priya Patel',     'priya@co.com',  '2021-03-20', 75000, 7500,  10, 1, 'Senior Engineer', 'Active'),
(3, 'Rahul Gupta',     'rahul@co.com',  '2022-06-10', 65000, 5000,  10, 1, 'Engineer', 'Active'),
(4, 'Sneha Reddy',     'sneha@co.com',  '2021-09-01', 80000, 8000,  20, NULL, 'Marketing Manager', 'Active'),
(5, 'Vikram Singh',    'vikram@co.com', '2023-01-15', 55000, NULL,  20, 4, 'Marketing Analyst', 'Active'),
(6, 'Anitha Kumar',    'anitha@co.com', '2020-07-20', 90000, 9000,  30, NULL, 'Finance Director', 'Active'),
(7, 'Kiran Nair',      'kiran@co.com',  '2022-11-01', 70000, 7000,  30, 6, 'Finance Analyst', 'Active'),
(8, 'Deepa Joshi',     'deepa@co.com',  '2019-04-05', 105000, 15000, 10, 1, 'Principal Engineer', 'Active'),
(9, 'Manoj Verma',     'manoj@co.com',  '2023-08-01', 50000, NULL,  40, NULL, 'HR Manager', 'Active'),
(10, 'Kavitha Iyer',   'kavitha@co.com','2021-12-10', 60000, 5000,  40, 9, 'HR Analyst', 'Active');
```

---

# KEY REMINDERS FOR INTERVIEW DAY

## Before Writing the Query:
1. **Clarify the problem** — repeat it back to the interviewer
2. **Ask about edge cases** — NULLs? Duplicates? Ties?
3. **State your approach** — "I'll use a window function to rank, then filter"
4. **Think about scale** — "On a large table, I'd add an index on dept_id"

## While Writing:
1. Use meaningful aliases (e, d, o — not a, b, c)
2. Format clearly — one clause per line
3. Start with the simplest version, optimize after
4. Say what you're typing out loud

## After Writing:
1. **Trace through** with sample data — does it give the right answer?
2. **Edge cases** — what if the column is NULL? What if there's a tie?
3. **Optimization** — "This uses a correlated subquery; I could rewrite with a JOIN for better performance"
4. **Alternate approaches** — always have a backup solution

## Common Interview Mistakes to Avoid:
- Using `= NULL` instead of `IS NULL`
- Forgetting PARTITION BY in window functions (applies to all rows)
- Using HAVING when WHERE would work (HAVING is slower)
- Not handling NULL in NOT IN subqueries
- Using `SELECT *` (always name columns in interviews to show awareness)
- Forgetting that BETWEEN is inclusive on both ends
- Confusing RANK (gaps) with DENSE_RANK (no gaps)
- Not aliasing derived tables / subqueries (required in most SQL engines)
