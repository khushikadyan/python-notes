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
