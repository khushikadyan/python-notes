# SQL INTERVIEW PREPARATION — COMPLETE GUIDE
## Part 3: 100 Scenario-Based Questions + Company-Wise + Practice Problems

---

# SECTION 17: 100 REAL INTERVIEW SCENARIOS

---

## CATEGORY A: AMAZON / FLIPKART (E-Commerce)

### A1. Find the top 3 products by revenue in each category for the last 30 days
```sql
WITH product_revenue AS (
    SELECT 
        p.category_id,
        p.product_id,
        p.product_name,
        SUM(oi.quantity * oi.unit_price) AS revenue,
        DENSE_RANK() OVER (PARTITION BY p.category_id ORDER BY SUM(oi.quantity * oi.unit_price) DESC) AS dr
    FROM order_items oi
    JOIN products p ON oi.product_id = p.product_id
    JOIN orders o ON oi.order_id = o.order_id
    WHERE o.order_date >= DATEADD(DAY, -30, GETDATE())
    GROUP BY p.category_id, p.product_id, p.product_name
)
SELECT category_id, product_id, product_name, revenue
FROM product_revenue
WHERE dr <= 3
ORDER BY category_id, revenue DESC;
```

### A2. Find customers who placed orders in every month of 2024
```sql
SELECT customer_id
FROM orders
WHERE YEAR(order_date) = 2024
GROUP BY customer_id
HAVING COUNT(DISTINCT MONTH(order_date)) = 12;
```

### A3. Calculate the 7-day moving average of daily orders
```sql
SELECT order_date, daily_orders,
       AVG(daily_orders) OVER (
           ORDER BY order_date
           ROWS BETWEEN 6 PRECEDING AND CURRENT ROW
       ) AS moving_avg_7d
FROM (
    SELECT order_date, COUNT(*) AS daily_orders
    FROM orders
    GROUP BY order_date
) d;
```

### A4. Find customers whose total spend increased each consecutive month
```sql
WITH monthly_spend AS (
    SELECT customer_id, 
           FORMAT(order_date, 'yyyy-MM') AS month,
           SUM(amount) AS monthly_total
    FROM orders
    GROUP BY customer_id, FORMAT(order_date, 'yyyy-MM')
),
with_lag AS (
    SELECT customer_id, month, monthly_total,
           LAG(monthly_total) OVER (PARTITION BY customer_id ORDER BY month) AS prev_month
    FROM monthly_spend
)
SELECT customer_id
FROM with_lag
WHERE prev_month IS NOT NULL
GROUP BY customer_id
HAVING MIN(monthly_total - prev_month) > 0;  -- all months increased
```

### A5. Find products that were ordered together most frequently (market basket)
```sql
SELECT a.product_id AS product1, b.product_id AS product2, COUNT(*) AS co_occurrences
FROM order_items a
JOIN order_items b ON a.order_id = b.order_id AND a.product_id < b.product_id
GROUP BY a.product_id, b.product_id
ORDER BY co_occurrences DESC;
```

### A6. Calculate customer lifetime value (LTV) — total revenue per customer
```sql
SELECT 
    c.customer_id,
    c.customer_name,
    COUNT(DISTINCT o.order_id) AS total_orders,
    SUM(oi.quantity * oi.unit_price) AS lifetime_value,
    MIN(o.order_date) AS first_order,
    MAX(o.order_date) AS last_order,
    DATEDIFF(DAY, MIN(o.order_date), MAX(o.order_date)) AS customer_tenure_days
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id
JOIN order_items oi ON o.order_id = oi.order_id
GROUP BY c.customer_id, c.customer_name
ORDER BY lifetime_value DESC;
```

### A7. Find churned customers (no orders in last 90 days but ordered before that)
```sql
SELECT DISTINCT customer_id
FROM orders
WHERE order_date < DATEADD(DAY, -90, GETDATE())  -- ordered before 90 days ago
  AND customer_id NOT IN (
      SELECT customer_id FROM orders 
      WHERE order_date >= DATEADD(DAY, -90, GETDATE())  -- no recent orders
  );
```

### A8. Rank sellers by revenue and show percentile
```sql
SELECT seller_id, total_revenue,
       RANK()       OVER (ORDER BY total_revenue DESC) AS revenue_rank,
       NTILE(100)   OVER (ORDER BY total_revenue)      AS percentile,
       ROUND(PERCENT_RANK() OVER (ORDER BY total_revenue) * 100, 2) AS pct_rank
FROM (
    SELECT seller_id, SUM(amount) AS total_revenue
    FROM seller_orders GROUP BY seller_id
) s;
```

### A9. Find the time gap between consecutive orders for each customer
```sql
SELECT customer_id, order_id, order_date,
       LAG(order_date) OVER (PARTITION BY customer_id ORDER BY order_date) AS prev_order_date,
       DATEDIFF(DAY, 
           LAG(order_date) OVER (PARTITION BY customer_id ORDER BY order_date),
           order_date
       ) AS days_between_orders
FROM orders;
```

### A10. Find products with consistently declining sales over 3 consecutive months
```sql
WITH monthly_sales AS (
    SELECT product_id, 
           YEAR(order_date) AS yr, MONTH(order_date) AS mo,
           SUM(quantity) AS units_sold
    FROM order_items oi JOIN orders o ON oi.order_id = o.order_id
    GROUP BY product_id, YEAR(order_date), MONTH(order_date)
),
with_lags AS (
    SELECT product_id, yr, mo, units_sold,
           LAG(units_sold, 1) OVER (PARTITION BY product_id ORDER BY yr, mo) AS m1,
           LAG(units_sold, 2) OVER (PARTITION BY product_id ORDER BY yr, mo) AS m2
    FROM monthly_sales
)
SELECT DISTINCT product_id
FROM with_lags
WHERE units_sold < m1 AND m1 < m2;  -- declining 3 months in a row
```

---

## CATEGORY B: MICROSOFT / GOOGLE (Analytics Heavy)

### B1. Calculate retention rate by cohort (users who return in month 2)
```sql
WITH first_purchase AS (
    SELECT customer_id, MIN(DATEFROMPARTS(YEAR(order_date), MONTH(order_date), 1)) AS cohort_month
    FROM orders GROUP BY customer_id
),
activity AS (
    SELECT o.customer_id, DATEFROMPARTS(YEAR(order_date), MONTH(order_date), 1) AS activity_month
    FROM orders o
),
cohort_activity AS (
    SELECT f.cohort_month, a.activity_month,
           DATEDIFF(MONTH, f.cohort_month, a.activity_month) AS months_since_first,
           COUNT(DISTINCT f.customer_id) AS active_users
    FROM first_purchase f
    JOIN activity a ON f.customer_id = a.customer_id
    GROUP BY f.cohort_month, a.activity_month, DATEDIFF(MONTH, f.cohort_month, a.activity_month)
),
cohort_size AS (
    SELECT cohort_month, COUNT(*) AS total_users
    FROM first_purchase GROUP BY cohort_month
)
SELECT ca.cohort_month, ca.months_since_first, ca.active_users,
       cs.total_users,
       ROUND(ca.active_users * 100.0 / cs.total_users, 2) AS retention_rate
FROM cohort_activity ca
JOIN cohort_size cs ON ca.cohort_month = cs.cohort_month
ORDER BY ca.cohort_month, ca.months_since_first;
```

### B2. Median salary by department (SQL doesn't have built-in MEDIAN)
```sql
-- SQL Server
SELECT dept_id, 
       PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY salary) OVER (PARTITION BY dept_id) AS median_salary
FROM employees;

-- Alternative using ROW_NUMBER (works everywhere)
WITH ordered AS (
    SELECT dept_id, salary,
           ROW_NUMBER() OVER (PARTITION BY dept_id ORDER BY salary) AS rn,
           COUNT(*) OVER (PARTITION BY dept_id) AS cnt
    FROM employees
)
SELECT dept_id, AVG(CAST(salary AS FLOAT)) AS median_salary
FROM ordered
WHERE rn IN (FLOOR((cnt + 1) / 2.0), CEILING((cnt + 1) / 2.0))
GROUP BY dept_id;
```

### B3. Find the first and last purchase date for each customer and days between
```sql
SELECT customer_id,
       MIN(order_date)                      AS first_purchase,
       MAX(order_date)                      AS last_purchase,
       COUNT(DISTINCT order_id)             AS total_orders,
       DATEDIFF(DAY, MIN(order_date), MAX(order_date)) AS days_active
FROM orders
GROUP BY customer_id;
```

### B4. Pivot: Show monthly sales by quarter as columns
```sql
SELECT YEAR(order_date) AS yr,
    SUM(CASE WHEN MONTH(order_date) IN (1,2,3)   THEN amount ELSE 0 END) AS Q1,
    SUM(CASE WHEN MONTH(order_date) IN (4,5,6)   THEN amount ELSE 0 END) AS Q2,
    SUM(CASE WHEN MONTH(order_date) IN (7,8,9)   THEN amount ELSE 0 END) AS Q3,
    SUM(CASE WHEN MONTH(order_date) IN (10,11,12) THEN amount ELSE 0 END) AS Q4
FROM orders
GROUP BY YEAR(order_date)
ORDER BY yr;
```

### B5. Identify users who logged in on 3 or more consecutive days
```sql
WITH date_groups AS (
    SELECT user_id, login_date,
           DATEADD(DAY, -ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY login_date), login_date) AS grp
    FROM (SELECT DISTINCT user_id, CAST(login_time AS DATE) AS login_date FROM user_logins) d
),
streaks AS (
    SELECT user_id, grp, COUNT(*) AS streak_length,
           MIN(login_date) AS streak_start, MAX(login_date) AS streak_end
    FROM date_groups GROUP BY user_id, grp
)
SELECT user_id, streak_start, streak_end, streak_length
FROM streaks WHERE streak_length >= 3
ORDER BY streak_length DESC;
```

---

## CATEGORY C: FINANCE / BANKING (JPMorgan, Goldman Sachs, Deloitte)

### C1. Calculate running balance for a bank account
```sql
SELECT account_id, transaction_date, transaction_type, amount,
       SUM(CASE WHEN transaction_type = 'CREDIT' THEN amount ELSE -amount END)
           OVER (PARTITION BY account_id ORDER BY transaction_date, transaction_id
                 ROWS UNBOUNDED PRECEDING) AS running_balance
FROM transactions
ORDER BY account_id, transaction_date;
```

### C2. Flag transactions above 3x the account's average (fraud detection)
```sql
SELECT t.*,
       AVG(amount) OVER (PARTITION BY account_id) AS avg_amount,
       CASE WHEN amount > 3 * AVG(amount) OVER (PARTITION BY account_id) 
            THEN 'SUSPICIOUS' ELSE 'NORMAL' END AS flag
FROM transactions t;
```

### C3. Find employees whose salary is above 90th percentile
```sql
WITH percentiles AS (
    SELECT emp_id, emp_name, salary, dept_id,
           NTILE(10) OVER (ORDER BY salary) AS decile
    FROM employees
)
SELECT emp_id, emp_name, salary, dept_id
FROM percentiles WHERE decile = 10;

-- Or using PERCENT_RANK
SELECT emp_id, emp_name, salary
FROM (
    SELECT emp_id, emp_name, salary,
           PERCENT_RANK() OVER (ORDER BY salary) AS pct_rank
    FROM employees
) t WHERE pct_rank >= 0.90;
```

### C4. Year-over-year revenue growth by region
```sql
WITH yearly AS (
    SELECT region, YEAR(transaction_date) AS yr, SUM(revenue) AS total_rev
    FROM sales GROUP BY region, YEAR(transaction_date)
)
SELECT region, yr, total_rev,
       LAG(total_rev) OVER (PARTITION BY region ORDER BY yr) AS prev_year,
       ROUND((total_rev - LAG(total_rev) OVER (PARTITION BY region ORDER BY yr)) * 100.0
             / LAG(total_rev) OVER (PARTITION BY region ORDER BY yr), 2) AS yoy_growth_pct
FROM yearly
ORDER BY region, yr;
```

### C5. Find top 5 transactions by amount for each customer per month
```sql
WITH ranked AS (
    SELECT customer_id, transaction_id, amount,
           FORMAT(transaction_date, 'yyyy-MM') AS month,
           ROW_NUMBER() OVER (
               PARTITION BY customer_id, FORMAT(transaction_date, 'yyyy-MM')
               ORDER BY amount DESC
           ) AS rn
    FROM transactions
)
SELECT customer_id, month, transaction_id, amount
FROM ranked WHERE rn <= 5
ORDER BY customer_id, month, amount DESC;
```

---

## CATEGORY D: UBER / OLA (Ride / Location Based)

### D1. Find drivers with highest average rating in each city
```sql
WITH driver_ratings AS (
    SELECT driver_id, city,
           AVG(rating) AS avg_rating,
           COUNT(*) AS total_trips,
           DENSE_RANK() OVER (PARTITION BY city ORDER BY AVG(rating) DESC) AS dr
    FROM trips
    WHERE rating IS NOT NULL
    GROUP BY driver_id, city
)
SELECT city, driver_id, avg_rating, total_trips
FROM driver_ratings WHERE dr = 1;
```

### D2. Calculate surge pricing windows (trips where demand > 2x supply)
```sql
WITH hourly_metrics AS (
    SELECT 
        city,
        DATEPART(HOUR, trip_start) AS hour,
        COUNT(DISTINCT trip_id) AS demand,
        COUNT(DISTINCT CASE WHEN driver_available = 1 THEN driver_id END) AS supply
    FROM trips t
    GROUP BY city, DATEPART(HOUR, trip_start)
)
SELECT city, hour, demand, supply,
       CAST(demand AS FLOAT) / NULLIF(supply, 0) AS demand_supply_ratio,
       CASE WHEN CAST(demand AS FLOAT) / NULLIF(supply, 0) > 2 THEN 'SURGE' ELSE 'NORMAL' END AS pricing
FROM hourly_metrics;
```

### D3. Find drivers who completed every ride without cancellation in last 7 days
```sql
SELECT driver_id
FROM trips
WHERE trip_date >= DATEADD(DAY, -7, GETDATE())
GROUP BY driver_id
HAVING SUM(CASE WHEN status = 'CANCELLED' THEN 1 ELSE 0 END) = 0
   AND COUNT(*) > 0;
```

---

## CATEGORY E: HR / EMPLOYEE MANAGEMENT (SAP, Workday, Oracle HR)

### E1. Find managers who manage more than 5 direct reports
```sql
SELECT manager_id, COUNT(*) AS direct_reports
FROM employees
WHERE manager_id IS NOT NULL
GROUP BY manager_id
HAVING COUNT(*) > 5
ORDER BY direct_reports DESC;
```

### E2. Find employees who joined in same month as their manager
```sql
SELECT e.emp_id, e.emp_name, e.hire_date,
       m.emp_name AS manager_name, m.hire_date AS manager_hire_date
FROM employees e
JOIN employees m ON e.manager_id = m.emp_id
WHERE MONTH(e.hire_date) = MONTH(m.hire_date)
  AND YEAR(e.hire_date) = YEAR(m.hire_date);
```

### E3. Compute salary percentile within each job title
```sql
SELECT emp_name, job_title, salary,
       PERCENT_RANK() OVER (PARTITION BY job_title ORDER BY salary) AS pct_within_title,
       ROUND(NTILE(4) OVER (PARTITION BY job_title ORDER BY salary), 0) AS quartile
FROM employees;
```

### E4. Find departments where all employees earn above the company average
```sql
WITH company_avg AS (SELECT AVG(salary) AS avg_sal FROM employees)
SELECT dept_id
FROM employees
GROUP BY dept_id
HAVING MIN(salary) > (SELECT avg_sal FROM company_avg);
```

### E5. Show employee count change month over month (headcount trend)
```sql
WITH monthly_hires AS (
    SELECT DATEFROMPARTS(YEAR(hire_date), MONTH(hire_date), 1) AS month,
           COUNT(*) AS new_hires
    FROM employees GROUP BY YEAR(hire_date), MONTH(hire_date)
),
monthly_exits AS (
    SELECT DATEFROMPARTS(YEAR(termination_date), MONTH(termination_date), 1) AS month,
           COUNT(*) AS exits
    FROM employees WHERE termination_date IS NOT NULL
    GROUP BY YEAR(termination_date), MONTH(termination_date)
)
SELECT COALESCE(h.month, e.month) AS month,
       COALESCE(h.new_hires, 0) AS hires,
       COALESCE(e.exits, 0) AS exits,
       COALESCE(h.new_hires, 0) - COALESCE(e.exits, 0) AS net_change,
       SUM(COALESCE(h.new_hires, 0) - COALESCE(e.exits, 0)) 
           OVER (ORDER BY COALESCE(h.month, e.month) ROWS UNBOUNDED PRECEDING) AS running_headcount
FROM monthly_hires h
FULL OUTER JOIN monthly_exits e ON h.month = e.month
ORDER BY month;
```

---

## CATEGORY F: WAREHOUSING / INVENTORY (Walmart, Target, SAP)

### F1. Find products that have been out of stock for more than 7 consecutive days
```sql
WITH stock_groups AS (
    SELECT product_id, stock_date, stock_level,
           DATEADD(DAY, -ROW_NUMBER() OVER (PARTITION BY product_id ORDER BY stock_date), stock_date) AS grp
    FROM inventory_daily WHERE stock_level = 0
)
SELECT product_id, MIN(stock_date) AS out_of_stock_start, 
       MAX(stock_date) AS out_of_stock_end,
       DATEDIFF(DAY, MIN(stock_date), MAX(stock_date)) + 1 AS consecutive_days
FROM stock_groups
GROUP BY product_id, grp
HAVING DATEDIFF(DAY, MIN(stock_date), MAX(stock_date)) + 1 > 7
ORDER BY consecutive_days DESC;
```

### F2. ABC analysis — classify products by contribution to revenue
```sql
WITH product_revenue AS (
    SELECT product_id, SUM(revenue) AS total_rev
    FROM sales GROUP BY product_id
),
cumulative AS (
    SELECT product_id, total_rev,
           SUM(total_rev) OVER (ORDER BY total_rev DESC ROWS UNBOUNDED PRECEDING) AS cum_rev,
           SUM(total_rev) OVER () AS grand_total
    FROM product_revenue
)
SELECT product_id, total_rev,
       ROUND(cum_rev * 100.0 / grand_total, 2) AS cum_pct,
       CASE 
           WHEN cum_rev * 100.0 / grand_total <= 80 THEN 'A'
           WHEN cum_rev * 100.0 / grand_total <= 95 THEN 'B'
           ELSE 'C'
       END AS abc_class
FROM cumulative ORDER BY total_rev DESC;
```

---

## CATEGORY G: SOCIAL MEDIA / CONTENT (Google, Facebook, Twitter)

### G1. Find users with mutual followers
```sql
SELECT a.follower_id AS user1, a.following_id AS user2
FROM follows a
JOIN follows b ON a.follower_id = b.following_id AND a.following_id = b.follower_id
WHERE a.follower_id < a.following_id;  -- avoid duplicates
```

### G2. Find the most viral post (most reshares within 24 hours of posting)
```sql
WITH post_reshares AS (
    SELECT p.post_id, p.post_time,
           COUNT(r.reshare_id) AS reshares_24h
    FROM posts p
    LEFT JOIN reshares r ON p.post_id = r.original_post_id
        AND r.reshare_time BETWEEN p.post_time AND DATEADD(HOUR, 24, p.post_time)
    GROUP BY p.post_id, p.post_time
)
SELECT TOP 10 post_id, reshares_24h
FROM post_reshares ORDER BY reshares_24h DESC;
```

### G3. Calculate engagement rate per post (likes + comments + shares / reach)
```sql
SELECT post_id, 
       likes + comments + shares AS total_engagements,
       reach,
       ROUND((likes + comments + shares) * 100.0 / NULLIF(reach, 0), 2) AS engagement_rate_pct
FROM post_metrics
ORDER BY engagement_rate_pct DESC;
```

---

## CATEGORY H: GENERAL HARD INTERVIEW QUESTIONS

### H1. Find all pairs of employees in the same department earning within 10% of each other
```sql
SELECT e1.emp_name AS emp1, e2.emp_name AS emp2,
       e1.salary AS sal1, e2.salary AS sal2,
       ABS(e1.salary - e2.salary) AS diff,
       e1.dept_id
FROM employees e1
JOIN employees e2 ON e1.dept_id = e2.dept_id AND e1.emp_id < e2.emp_id
WHERE ABS(e1.salary - e2.salary) <= e1.salary * 0.10;
```

### H2. Find employees with no salary increase in the last 2 years
```sql
SELECT emp_id, emp_name
FROM employees
WHERE emp_id NOT IN (
    SELECT emp_id FROM salary_history
    WHERE change_date >= DATEADD(YEAR, -2, GETDATE())
      AND new_salary > old_salary
);
```

### H3. Write a query to transpose rows to columns without PIVOT
```sql
-- Skills per employee stored as rows, show as columns
SELECT emp_id,
    MAX(CASE WHEN skill = 'SQL'    THEN 'Yes' END) AS SQL,
    MAX(CASE WHEN skill = 'Python' THEN 'Yes' END) AS Python,
    MAX(CASE WHEN skill = 'Java'   THEN 'Yes' END) AS Java,
    MAX(CASE WHEN skill = 'AWS'    THEN 'Yes' END) AS AWS
FROM emp_skills
GROUP BY emp_id;
```

### H4. Detect slow-paying customers (avg days to pay > 60)
```sql
SELECT customer_id,
       AVG(DATEDIFF(DAY, invoice_date, payment_date)) AS avg_days_to_pay,
       COUNT(*) AS total_invoices
FROM invoices
WHERE payment_date IS NOT NULL
GROUP BY customer_id
HAVING AVG(DATEDIFF(DAY, invoice_date, payment_date)) > 60
ORDER BY avg_days_to_pay DESC;
```

### H5. Find records where the sum of a column exceeds a threshold cumulatively
```sql
-- Find the date when cumulative sales first exceeded 1,000,000
SELECT TOP 1 order_date, running_total
FROM (
    SELECT order_date,
           SUM(amount) OVER (ORDER BY order_date ROWS UNBOUNDED PRECEDING) AS running_total
    FROM orders
) t
WHERE running_total >= 1000000
ORDER BY order_date;
```

---

# SECTION 18: COMPANY-WISE FREQUENTLY ASKED QUESTIONS

---

## 18.1 Amazon SQL Interview Questions

**Easy:**
1. Find all orders placed in the last 7 days
2. List customers who have never placed an order
3. Find the most expensive product in each category
4. Count orders by status
5. Find employees earning above department average

**Medium:**
6. Find customers who placed more than 3 orders in any single month
7. Calculate month-over-month revenue growth
8. Find products that were never sold
9. Get the second most recent order for each customer
10. Find departments where average salary > company average salary

**Hard:**
11. Find the longest streak of consecutive days a customer made a purchase
12. Build a customer segmentation (RFM — Recency, Frequency, Monetary)
13. Find all users who viewed product A before purchasing product B
14. Calculate rolling 30-day unique customer count
15. Detect duplicate orders placed within 10 minutes by the same customer

**RFM Segmentation Query:**
```sql
WITH rfm AS (
    SELECT customer_id,
           DATEDIFF(DAY, MAX(order_date), GETDATE()) AS recency,
           COUNT(DISTINCT order_id) AS frequency,
           SUM(amount) AS monetary
    FROM orders
    GROUP BY customer_id
),
rfm_scored AS (
    SELECT customer_id, recency, frequency, monetary,
           NTILE(5) OVER (ORDER BY recency ASC)   AS r_score,  -- lower recency = higher score
           NTILE(5) OVER (ORDER BY frequency DESC) AS f_score,
           NTILE(5) OVER (ORDER BY monetary DESC)  AS m_score
    FROM rfm
)
SELECT customer_id, r_score, f_score, m_score,
       r_score + f_score + m_score AS total_rfm_score,
       CASE
           WHEN r_score >= 4 AND f_score >= 4 THEN 'Champions'
           WHEN r_score >= 3 AND f_score >= 3 THEN 'Loyal Customers'
           WHEN r_score >= 4 AND f_score < 3  THEN 'Recent Customers'
           WHEN r_score < 2 AND f_score >= 4  THEN 'At Risk'
           ELSE 'Others'
       END AS customer_segment
FROM rfm_scored;
```

---

## 18.2 Microsoft SQL Interview Questions

1. Find all employees who report directly or indirectly to a given manager (use recursive CTE)
2. Pivot monthly sales data into rows by year
3. Find the employee with the highest salary in each job band
4. Find all pairs of customers who live in the same city and have the same name initial
5. Write a query to find all "orphan" records (FK with no corresponding PK)

```sql
-- Orphan records
SELECT o.order_id, o.customer_id
FROM orders o
LEFT JOIN customers c ON o.customer_id = c.customer_id
WHERE c.customer_id IS NULL;
```

---

## 18.3 Google / Meta (Data Science Heavy)

1. A/B test analysis: find if conversion rate is statistically different between two groups
2. Calculate session duration from page view events
3. Find users who searched but never clicked (zero click searches)
4. Compute daily active users (DAU), weekly active users (WAU), and DAU/WAU ratio
5. Find content that went viral (10x normal engagement within 1 hour)

```sql
-- DAU/WAU Ratio
WITH dau AS (
    SELECT event_date, COUNT(DISTINCT user_id) AS daily_users
    FROM user_events GROUP BY event_date
),
wau AS (
    SELECT event_date,
           COUNT(DISTINCT user_id) OVER (
               ORDER BY event_date ROWS BETWEEN 6 PRECEDING AND CURRENT ROW
           ) AS weekly_users
    FROM user_events
)
SELECT d.event_date, d.daily_users,
       w.weekly_users,
       ROUND(d.daily_users * 100.0 / NULLIF(w.weekly_users, 0), 2) AS dau_wau_ratio
FROM dau d JOIN wau w ON d.event_date = w.event_date;
```

---

## 18.4 Oracle / SAP (Enterprise / ERP)

1. Find all employees in a 3-level management chain under a specific VP
2. Find all invoices older than payment terms but not yet paid
3. Identify GL entries that don't balance (debits ≠ credits)
4. Find suppliers with delivery times consistently above SLA

```sql
-- GL balance check
SELECT journal_id, 
       SUM(CASE WHEN entry_type = 'DEBIT'  THEN amount ELSE 0 END) AS total_debit,
       SUM(CASE WHEN entry_type = 'CREDIT' THEN amount ELSE 0 END) AS total_credit,
       SUM(CASE WHEN entry_type = 'DEBIT'  THEN amount ELSE 0 END) -
       SUM(CASE WHEN entry_type = 'CREDIT' THEN amount ELSE 0 END) AS imbalance
FROM journal_entries
GROUP BY journal_id
HAVING ABS(SUM(CASE WHEN entry_type = 'DEBIT' THEN amount ELSE 0 END) -
           SUM(CASE WHEN entry_type = 'CREDIT' THEN amount ELSE 0 END)) > 0.01;
```

---

# SECTION 19: 100 PRACTICE PROBLEMS WITH SOLUTIONS

---

## EASY (1-30)

**1. Find all employees in department 10**
```sql
SELECT * FROM employees WHERE dept_id = 10;
```

**2. Find employees whose name starts with 'A'**
```sql
SELECT * FROM employees WHERE emp_name LIKE 'A%';
```

**3. Find total salary by department**
```sql
SELECT dept_id, SUM(salary) AS total_salary FROM employees GROUP BY dept_id;
```

**4. Find employees with salary between 50000 and 80000**
```sql
SELECT * FROM employees WHERE salary BETWEEN 50000 AND 80000;
```

**5. Count employees per department, sorted by count descending**
```sql
SELECT dept_id, COUNT(*) AS headcount FROM employees GROUP BY dept_id ORDER BY headcount DESC;
```

**6. Find departments with more than 3 employees**
```sql
SELECT dept_id, COUNT(*) AS cnt FROM employees GROUP BY dept_id HAVING COUNT(*) > 3;
```

**7. Find the maximum salary in the company**
```sql
SELECT MAX(salary) AS max_salary FROM employees;
```

**8. Find employees hired in 2023**
```sql
SELECT * FROM employees WHERE YEAR(hire_date) = 2023;
```

**9. Find all unique job titles**
```sql
SELECT DISTINCT job_title FROM employees ORDER BY job_title;
```

**10. Find employees with no manager**
```sql
SELECT * FROM employees WHERE manager_id IS NULL;
```

**11. Find top 5 highest-paid employees**
```sql
SELECT TOP 5 emp_name, salary FROM employees ORDER BY salary DESC;
-- MySQL: SELECT emp_name, salary FROM employees ORDER BY salary DESC LIMIT 5;
```

**12. Add 10% bonus to all employees in department 20**
```sql
UPDATE employees SET bonus = salary * 0.10 WHERE dept_id = 20;
```

**13. Delete all inactive employees**
```sql
DELETE FROM employees WHERE status = 'Inactive';
```

**14. Find employees earning more than 75000**
```sql
SELECT emp_name, salary FROM employees WHERE salary > 75000 ORDER BY salary DESC;
```

**15. Show employee name and department name**
```sql
SELECT e.emp_name, d.dept_name 
FROM employees e JOIN departments d ON e.dept_id = d.dept_id;
```

**16. Count total orders per customer**
```sql
SELECT customer_id, COUNT(*) AS order_count FROM orders GROUP BY customer_id;
```

**17. Find orders placed today**
```sql
SELECT * FROM orders WHERE CAST(order_date AS DATE) = CAST(GETDATE() AS DATE);
```

**18. Find the average salary**
```sql
SELECT AVG(salary) AS avg_salary FROM employees;
```

**19. Find products with price > 100 in category 'Electronics'**
```sql
SELECT * FROM products WHERE category = 'Electronics' AND price > 100;
```

**20. Show departments with no employees**
```sql
SELECT d.dept_id, d.dept_name 
FROM departments d 
LEFT JOIN employees e ON d.dept_id = e.dept_id 
WHERE e.emp_id IS NULL;
```

**21. Find the earliest hire date**
```sql
SELECT MIN(hire_date) AS earliest_hire FROM employees;
```

**22. Find employees whose salary is above the average**
```sql
SELECT emp_name, salary FROM employees WHERE salary > (SELECT AVG(salary) FROM employees);
```

**23. Show customer name and their total order amount**
```sql
SELECT c.customer_name, SUM(o.amount) AS total_spent
FROM customers c JOIN orders o ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.customer_name
ORDER BY total_spent DESC;
```

**24. Find products that were never ordered**
```sql
SELECT p.product_id, p.product_name
FROM products p
LEFT JOIN order_items oi ON p.product_id = oi.product_id
WHERE oi.product_id IS NULL;
```

**25. Get the 3rd highest salary**
```sql
SELECT salary FROM (
    SELECT salary, DENSE_RANK() OVER (ORDER BY salary DESC) AS dr FROM employees
) t WHERE dr = 3;
```

**26. Show employees ordered by hire date, newest first**
```sql
SELECT emp_name, hire_date FROM employees ORDER BY hire_date DESC;
```

**27. Find customers from New York who spent more than 1000**
```sql
SELECT c.customer_name, SUM(o.amount) AS total
FROM customers c JOIN orders o ON c.customer_id = o.customer_id
WHERE c.city = 'New York'
GROUP BY c.customer_id, c.customer_name
HAVING SUM(o.amount) > 1000;
```

**28. Assign salary grade: <40K=Grade1, 40K-70K=Grade2, >70K=Grade3**
```sql
SELECT emp_name, salary,
    CASE WHEN salary < 40000 THEN 'Grade 1'
         WHEN salary <= 70000 THEN 'Grade 2'
         ELSE 'Grade 3'
    END AS salary_grade
FROM employees;
```

**29. Count NULL vs non-NULL values in a column**
```sql
SELECT 
    COUNT(*) AS total,
    COUNT(bonus) AS non_null_bonus,
    COUNT(*) - COUNT(bonus) AS null_bonus
FROM employees;
```

**30. Find employees hired in Q1 (Jan-Mar)**
```sql
SELECT * FROM employees WHERE MONTH(hire_date) BETWEEN 1 AND 3;
```

---

## MEDIUM (31-70)

**31. Find the second highest salary in each department**
```sql
SELECT dept_id, salary FROM (
    SELECT dept_id, salary, DENSE_RANK() OVER (PARTITION BY dept_id ORDER BY salary DESC) AS dr
    FROM employees
) t WHERE dr = 2;
```

**32. Find employees who earn more than their manager**
```sql
SELECT e.emp_name, e.salary, m.emp_name AS manager, m.salary AS mgr_salary
FROM employees e JOIN employees m ON e.manager_id = m.emp_id
WHERE e.salary > m.salary;
```

**33. Get running total of sales by date**
```sql
SELECT order_date, daily_sales,
       SUM(daily_sales) OVER (ORDER BY order_date ROWS UNBOUNDED PRECEDING) AS running_total
FROM (SELECT order_date, SUM(amount) AS daily_sales FROM orders GROUP BY order_date) d;
```

**34. Find customers who ordered both product 'A' and product 'B'**
```sql
SELECT customer_id FROM orders o JOIN order_items oi ON o.order_id = oi.order_id
WHERE oi.product_name = 'A'
INTERSECT
SELECT customer_id FROM orders o JOIN order_items oi ON o.order_id = oi.order_id
WHERE oi.product_name = 'B';
```

**35. Find duplicate rows and count occurrences**
```sql
SELECT emp_name, email, COUNT(*) AS duplicates
FROM employees GROUP BY emp_name, email HAVING COUNT(*) > 1;
```

**36. Show month-over-month sales with growth percentage**
```sql
WITH monthly AS (
    SELECT FORMAT(order_date, 'yyyy-MM') AS month, SUM(amount) AS revenue
    FROM orders GROUP BY FORMAT(order_date, 'yyyy-MM')
)
SELECT month, revenue,
       LAG(revenue) OVER (ORDER BY month) AS prev_month,
       ROUND((revenue - LAG(revenue) OVER (ORDER BY month)) * 100.0 / 
             LAG(revenue) OVER (ORDER BY month), 2) AS growth_pct
FROM monthly;
```

**37. Find employees with the same salary**
```sql
SELECT salary, STRING_AGG(emp_name, ', ') AS employees, COUNT(*) AS cnt
FROM employees GROUP BY salary HAVING COUNT(*) > 1;
```

**38. Find the most recently hired employee in each department**
```sql
SELECT dept_id, emp_name, hire_date FROM (
    SELECT dept_id, emp_name, hire_date,
           ROW_NUMBER() OVER (PARTITION BY dept_id ORDER BY hire_date DESC) AS rn
    FROM employees
) t WHERE rn = 1;
```

**39. Calculate employee tenure in years and months**
```sql
SELECT emp_name, hire_date,
       DATEDIFF(YEAR, hire_date, GETDATE()) AS years,
       DATEDIFF(MONTH, hire_date, GETDATE()) % 12 AS months
FROM employees;
```

**40. Find products with price above category average**
```sql
SELECT p.product_id, p.product_name, p.price, ca.avg_price
FROM products p
JOIN (SELECT category_id, AVG(price) AS avg_price FROM products GROUP BY category_id) ca
ON p.category_id = ca.category_id
WHERE p.price > ca.avg_price;
```

**41. Rank employees within department by salary and show dense rank**
```sql
SELECT emp_name, dept_id, salary,
       DENSE_RANK() OVER (PARTITION BY dept_id ORDER BY salary DESC) AS dept_rank
FROM employees;
```

**42. Find the percentage of total sales each product contributes**
```sql
SELECT product_id, SUM(amount) AS product_sales,
       ROUND(SUM(amount) * 100.0 / SUM(SUM(amount)) OVER (), 2) AS pct_of_total
FROM order_items GROUP BY product_id ORDER BY product_sales DESC;
```

**43. Find orders that contain more than 5 different products**
```sql
SELECT order_id, COUNT(DISTINCT product_id) AS unique_products
FROM order_items GROUP BY order_id HAVING COUNT(DISTINCT product_id) > 5;
```

**44. Find all employees hired on the same date**
```sql
SELECT hire_date, COUNT(*) AS hired_count, STRING_AGG(emp_name, ', ') AS employees
FROM employees GROUP BY hire_date HAVING COUNT(*) > 1;
```

**45. Calculate average days between orders for each customer**
```sql
WITH order_gaps AS (
    SELECT customer_id, order_date,
           DATEDIFF(DAY, LAG(order_date) OVER (PARTITION BY customer_id ORDER BY order_date), order_date) AS gap
    FROM orders
)
SELECT customer_id, AVG(gap) AS avg_days_between_orders
FROM order_gaps WHERE gap IS NOT NULL
GROUP BY customer_id;
```

**46. Get top 10% earners in the company**
```sql
SELECT emp_name, salary FROM (
    SELECT emp_name, salary, NTILE(10) OVER (ORDER BY salary DESC) AS decile
    FROM employees
) t WHERE decile = 1;
```

**47. Find products not sold in last 90 days**
```sql
SELECT p.product_id, p.product_name
FROM products p
WHERE p.product_id NOT IN (
    SELECT DISTINCT oi.product_id FROM order_items oi
    JOIN orders o ON oi.order_id = o.order_id
    WHERE o.order_date >= DATEADD(DAY, -90, GETDATE())
);
```

**48. Show department summary: name, headcount, min, avg, max salary**
```sql
SELECT d.dept_name, COUNT(e.emp_id) AS headcount,
       MIN(e.salary) AS min_sal, AVG(e.salary) AS avg_sal, MAX(e.salary) AS max_sal
FROM departments d LEFT JOIN employees e ON d.dept_id = e.dept_id
GROUP BY d.dept_id, d.dept_name ORDER BY headcount DESC;
```

**49. Find customers who placed their first order this month**
```sql
SELECT customer_id FROM orders
GROUP BY customer_id
HAVING MIN(order_date) >= DATEFROMPARTS(YEAR(GETDATE()), MONTH(GETDATE()), 1);
```

**50. Find all employees who joined after their direct manager**
```sql
SELECT e.emp_name, e.hire_date, m.emp_name AS manager, m.hire_date AS mgr_hire
FROM employees e JOIN employees m ON e.manager_id = m.emp_id
WHERE e.hire_date > m.hire_date;
```

---

## HARD (71-100)

**71. Implement a full RFM customer segmentation**
*(See H18.1 Amazon section above for complete solution)*

**72. Find all nodes in a hierarchy below a given root (recursive)**
```sql
WITH RECURSIVE tree AS (
    SELECT emp_id, emp_name, manager_id, 0 AS depth
    FROM employees WHERE emp_id = 5  -- root
    UNION ALL
    SELECT e.emp_id, e.emp_name, e.manager_id, t.depth + 1
    FROM employees e JOIN tree t ON e.manager_id = t.emp_id
)
SELECT * FROM tree ORDER BY depth, emp_name;
```

**73. Find the median salary without a MEDIAN function**
```sql
WITH ordered AS (
    SELECT salary, 
           ROW_NUMBER() OVER (ORDER BY salary) AS rn,
           COUNT(*) OVER () AS cnt
    FROM employees
)
SELECT AVG(CAST(salary AS FLOAT)) AS median
FROM ordered WHERE rn IN (FLOOR((cnt+1)/2.0), CEILING((cnt+1)/2.0));
```

**74. Find employees who appear in consecutive salary ranks (no gap)**
```sql
SELECT e1.emp_id, e1.emp_name, e1.salary AS curr_sal,
       e2.emp_name AS next_ranked, e2.salary AS next_sal
FROM (SELECT emp_id, emp_name, salary, DENSE_RANK() OVER (ORDER BY salary) AS dr FROM employees) e1
JOIN (SELECT emp_id, emp_name, salary, DENSE_RANK() OVER (ORDER BY salary) AS dr FROM employees) e2
    ON e1.dr + 1 = e2.dr;
```

**75. Find customers whose last 3 orders were all above average order value**
```sql
WITH customer_orders AS (
    SELECT customer_id, order_id, amount,
           ROW_NUMBER() OVER (PARTITION BY customer_id ORDER BY order_date DESC) AS rn,
           AVG(amount) OVER () AS global_avg
    FROM orders
)
SELECT customer_id FROM customer_orders
WHERE rn <= 3
GROUP BY customer_id
HAVING MIN(amount) > MAX(global_avg);  -- all 3 recent orders above global avg
```

**76. Build a session analysis — group events into sessions (30-min gap = new session)**
```sql
WITH with_gaps AS (
    SELECT user_id, event_time,
           CASE WHEN DATEDIFF(MINUTE, 
               LAG(event_time) OVER (PARTITION BY user_id ORDER BY event_time),
               event_time) > 30 OR 
               LAG(event_time) OVER (PARTITION BY user_id ORDER BY event_time) IS NULL
           THEN 1 ELSE 0 END AS new_session
    FROM user_events
),
session_ids AS (
    SELECT user_id, event_time,
           SUM(new_session) OVER (PARTITION BY user_id ORDER BY event_time) AS session_id
    FROM with_gaps
)
SELECT user_id, session_id,
       MIN(event_time) AS session_start,
       MAX(event_time) AS session_end,
       DATEDIFF(MINUTE, MIN(event_time), MAX(event_time)) AS duration_minutes,
       COUNT(*) AS events_in_session
FROM session_ids
GROUP BY user_id, session_id;
```

**77. Find the employee with the 3rd highest salary in each department (handle ties)**
```sql
WITH ranked AS (
    SELECT emp_id, emp_name, dept_id, salary,
           DENSE_RANK() OVER (PARTITION BY dept_id ORDER BY salary DESC) AS dr
    FROM employees
)
SELECT dept_id, emp_name, salary FROM ranked WHERE dr = 3;
```

**78. Calculate day-over-day retention: users who came back next day**
```sql
WITH daily_users AS (
    SELECT DISTINCT user_id, CAST(event_time AS DATE) AS activity_date
    FROM events
)
SELECT d1.activity_date, COUNT(DISTINCT d1.user_id) AS users,
       COUNT(DISTINCT d2.user_id) AS retained_next_day,
       ROUND(COUNT(DISTINCT d2.user_id) * 100.0 / COUNT(DISTINCT d1.user_id), 2) AS retention_rate
FROM daily_users d1
LEFT JOIN daily_users d2 ON d1.user_id = d2.user_id 
    AND d2.activity_date = DATEADD(DAY, 1, d1.activity_date)
GROUP BY d1.activity_date;
```

**79. Find pairs of orders from the same customer placed within 5 minutes (duplicate detection)**
```sql
SELECT o1.order_id AS order1, o2.order_id AS order2,
       o1.customer_id, o1.order_time AS t1, o2.order_time AS t2
FROM orders o1 JOIN orders o2 
    ON o1.customer_id = o2.customer_id 
   AND o1.order_id < o2.order_id
   AND ABS(DATEDIFF(MINUTE, o1.order_time, o2.order_time)) <= 5;
```

**80. Find the best-performing quarter for each product (rolling 90-day peak)**
```sql
WITH daily_sales AS (
    SELECT product_id, sale_date, SUM(amount) AS daily_revenue
    FROM sales GROUP BY product_id, sale_date
),
rolling AS (
    SELECT product_id, sale_date, daily_revenue,
           SUM(daily_revenue) OVER (
               PARTITION BY product_id ORDER BY sale_date
               ROWS BETWEEN 89 PRECEDING AND CURRENT ROW
           ) AS rolling_90d_revenue
    FROM daily_sales
)
SELECT product_id, sale_date AS period_end, rolling_90d_revenue
FROM (
    SELECT *, RANK() OVER (PARTITION BY product_id ORDER BY rolling_90d_revenue DESC) AS rk
    FROM rolling
) t WHERE rk = 1;
```

---

# SECTION 20: MOCK INTERVIEWS — 10 COMPLETE SESSIONS

---

## Mock Interview 1: Junior Data Analyst (Easy-Medium)

**Interviewer:** "I'm going to ask you 5 SQL questions of increasing difficulty. Please explain your thought process."

**Q1: What is a PRIMARY KEY?**  
**Candidate Answer:** "A Primary Key is a column or combination of columns that uniquely identifies each row in a table. It has two constraints enforced automatically: UNIQUE (no two rows can have the same PK value) and NOT NULL (every row must have a PK value). Each table can have only one Primary Key."

**Q2: Write a query to find all customers who have placed more than 3 orders.**
```sql
SELECT customer_id, COUNT(*) AS order_count
FROM orders
GROUP BY customer_id
HAVING COUNT(*) > 3
ORDER BY order_count DESC;
```
**Key Points to Mention:** "I use GROUP BY to aggregate orders by customer, then HAVING to filter after aggregation — I can't use WHERE here because WHERE runs before GROUP BY."

**Q3: Find the second highest salary.**
```sql
SELECT MAX(salary) FROM employees
WHERE salary < (SELECT MAX(salary) FROM employees);
```
**Follow-up:** "What if there are multiple employees with the same salary?"  
"I'd use DENSE_RANK to handle ties: `SELECT salary FROM (SELECT salary, DENSE_RANK() OVER (ORDER BY salary DESC) AS dr FROM employees) t WHERE dr = 2`"

**Q4: What is the difference between WHERE and HAVING?**  
"WHERE filters individual rows BEFORE aggregation — it can't reference aggregate functions. HAVING filters groups AFTER GROUP BY aggregation — it can reference aggregate functions like COUNT(*) or SUM(). WHERE is applied first in query execution, then GROUP BY, then HAVING."

**Q5: Write a query to find employees with no department assigned.**
```sql
SELECT emp_id, emp_name FROM employees WHERE dept_id IS NULL;
-- Note: WHERE dept_id = NULL would return 0 rows - wrong!
```

---

## Mock Interview 2: Senior Data Analyst (Medium-Hard)

**Q1: Explain the difference between RANK, DENSE_RANK, and ROW_NUMBER.**  
*(See Section 12.2 for complete answer)*

**Q2: You have a table of website events with user_id and event_timestamp. Find users who visited on 3 or more consecutive days in the last month.**
```sql
WITH recent_visits AS (
    SELECT DISTINCT user_id, CAST(event_timestamp AS DATE) AS visit_date
    FROM events WHERE event_timestamp >= DATEADD(MONTH, -1, GETDATE())
),
with_groups AS (
    SELECT user_id, visit_date,
           DATEADD(DAY, -ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY visit_date), visit_date) AS grp
    FROM recent_visits
),
streaks AS (
    SELECT user_id, COUNT(*) AS consecutive_days FROM with_groups GROUP BY user_id, grp
)
SELECT DISTINCT user_id FROM streaks WHERE consecutive_days >= 3;
```
**Thought process:** "The classic gaps-and-islands technique. When I subtract a ROW_NUMBER from a date, rows that are consecutive dates will have the same resulting date — that's the group key."

**Q3: Design a query to calculate the Customer Acquisition Cost (CAC) trend by channel.**
```sql
SELECT marketing_channel,
       YEAR(signup_date) AS year, MONTH(signup_date) AS month,
       SUM(marketing_spend) AS total_spend,
       COUNT(DISTINCT customer_id) AS new_customers,
       ROUND(SUM(marketing_spend) / COUNT(DISTINCT customer_id), 2) AS cac
FROM customer_acquisitions
GROUP BY marketing_channel, YEAR(signup_date), MONTH(signup_date)
ORDER BY marketing_channel, year, month;
```

---

## Mock Interview 3: Business Analyst (Scenario-Based)

**Interviewer:** "We're an e-commerce company. I'll give you business problems and I want you to translate them to SQL."

**Business Problem 1:** "Marketing wants to know which customers haven't bought in 6 months but used to be regular buyers (at least 3 orders in the year before that)."

```sql
WITH customer_stats AS (
    SELECT customer_id,
           -- Regular buyers: 3+ orders in the year before the 6-month cutoff
           SUM(CASE WHEN order_date BETWEEN DATEADD(MONTH, -18, GETDATE()) 
                                        AND DATEADD(MONTH, -6, GETDATE()) 
                    THEN 1 ELSE 0 END) AS orders_prior_year,
           -- Lapsed: no orders in last 6 months
           SUM(CASE WHEN order_date >= DATEADD(MONTH, -6, GETDATE()) 
                    THEN 1 ELSE 0 END) AS recent_orders
    FROM orders GROUP BY customer_id
)
SELECT customer_id
FROM customer_stats
WHERE orders_prior_year >= 3 AND recent_orders = 0;
```

**Business Problem 2:** "Our finance team needs a report showing which product categories are growing or shrinking quarter over quarter."

```sql
WITH quarterly_revenue AS (
    SELECT p.category_id,
           DATEPART(YEAR, o.order_date) AS yr,
           DATEPART(QUARTER, o.order_date) AS qtr,
           SUM(oi.quantity * oi.unit_price) AS revenue
    FROM order_items oi
    JOIN products p ON oi.product_id = p.product_id
    JOIN orders o ON oi.order_id = o.order_id
    GROUP BY p.category_id, DATEPART(YEAR, o.order_date), DATEPART(QUARTER, o.order_date)
)
SELECT category_id, yr, qtr, revenue,
       LAG(revenue) OVER (PARTITION BY category_id ORDER BY yr, qtr) AS prev_qtr,
       ROUND((revenue - LAG(revenue) OVER (PARTITION BY category_id ORDER BY yr, qtr)) 
             * 100.0 / LAG(revenue) OVER (PARTITION BY category_id ORDER BY yr, qtr), 2) AS qoq_growth,
       CASE WHEN revenue > LAG(revenue) OVER (PARTITION BY category_id ORDER BY yr, qtr) THEN 'Growing'
            WHEN revenue < LAG(revenue) OVER (PARTITION BY category_id ORDER BY yr, qtr) THEN 'Shrinking'
            ELSE 'Stable' END AS trend
FROM quarterly_revenue ORDER BY category_id, yr, qtr;
```
