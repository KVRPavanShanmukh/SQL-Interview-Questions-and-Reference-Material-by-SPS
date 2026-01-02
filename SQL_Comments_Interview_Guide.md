# SQL Comments: Complete Interview Reference Guide
## From Basics to Advanced Concepts

---

## TABLE OF CONTENTS
1. [Level 1: Basic Comments](#level-1-basic-comments)
2. [Level 2: Intermediate Comment Techniques](#level-2-intermediate-comment-techniques)
3. [Level 3: Advanced Comment Patterns](#level-3-advanced-comment-patterns)
4. [Level 4: Best Practices & Production Code](#level-4-best-practices--production-code)
5. [Interview Q&A Section](#interview-qa-section)

---

## LEVEL 1: BASIC COMMENTS

### 1.1 Single-Line Comments

**Syntax:**
```sql
-- This is a single-line comment
SELECT * FROM customers;
```

**Key Points:**
- Starts with two consecutive hyphens (`--`)
- Everything after `--` until the end of the line is ignored
- Used for brief explanations
- Cannot span multiple lines

**Examples:**

```sql
-- Select all customers
SELECT * FROM customers;

SELECT customer_id, -- unique customer identifier
       customer_name  -- person's name
FROM customers;

-- WHERE city = 'New York'; -- This entire line is commented out
```

**Interview Question:** 
> Q: What is the difference between `--` and `//` for comments in SQL?
> A: `--` is the standard SQL comment syntax. `//` is not valid in standard SQL (though supported in some database systems). Always use `--` for portability.

---

### 1.2 Multi-Line Comments

**Syntax:**
```sql
/* This is a multi-line comment
   that spans multiple lines */
SELECT * FROM customers;
```

**Key Points:**
- Starts with `/*` and ends with `*/`
- Can span multiple lines
- Cannot be nested in most SQL databases
- Useful for longer explanations

**Examples:**

```sql
/* Select all customers from the database
   This query retrieves the complete customer list
   for reporting purposes */
SELECT * FROM customers;

/* Temporary comment - remove this section after testing
SELECT * FROM test_table; */

SELECT customer_id, customer_name
/* , customer_email */ -- Temporarily disabled email column
FROM customers;
```

**Interview Question:**
> Q: Can you nest multi-line comments in SQL?
> A: No, most SQL databases don't support nested comments. You cannot have `/* comment1 /* comment2 */ comment1 */`. Some modern databases like PostgreSQL support nested comments, but it's not standard SQL.

---

## LEVEL 2: INTERMEDIATE COMMENT TECHNIQUES

### 2.1 Inline Comments

**Concept:**
Placing comments on the same line as code for quick clarifications.

**Examples:**

```sql
SELECT 
    customer_id,           -- Unique customer identifier
    customer_name,         -- Customer's full name
    email,                 -- Contact email address
    order_count            -- Total number of orders placed
FROM customers
WHERE status = 'active';   -- Only active customers

-- Alternative: Using /* */ inline
SELECT customer_id /* PK */, order_total /* Amount in USD */
FROM orders;
```

**When to Use:**
- Column explanations in SELECT clauses
- Clarifying filter conditions
- Brief context for aggregations

---

### 2.2 Comment Blocks for Query Sections

**Pattern:**
Organize long queries with comment blocks separating logical sections.

```sql
-- ========================================
-- QUERY: Monthly Sales Report
-- Date: January 2024
-- ========================================

-- 1. CTE to calculate monthly totals
WITH monthly_sales AS (
    SELECT 
        DATE_TRUNC('month', order_date) AS month,
        SUM(order_amount) AS total_sales
    FROM orders
    WHERE YEAR(order_date) = 2024
    GROUP BY DATE_TRUNC('month', order_date)
),

-- 2. CTE to rank months by sales
ranked_months AS (
    SELECT 
        month,
        total_sales,
        ROW_NUMBER() OVER (ORDER BY total_sales DESC) AS sales_rank
    FROM monthly_sales
)

-- 3. Final selection with filters
SELECT 
    month,
    total_sales,
    sales_rank
FROM ranked_months
WHERE sales_rank <= 5;  -- Top 5 months only
```

---

### 2.3 Commenting Out Code for Testing

**Purpose:** Temporarily disable queries without deleting them.

```sql
-- Testing new filters
/*
SELECT customer_id, customer_name, order_count
FROM customers
WHERE order_count > 100;
*/

-- Using existing filter instead
SELECT customer_id, customer_name, order_count
FROM customers
WHERE order_count > 50;
```

---

### 2.4 Documentation Comments

**Pattern:**
Include metadata about the query at the top.

```sql
/*
================================================================================
QUERY NAME: Customer Lifetime Value Report
PURPOSE: Calculate total spending per customer for analysis
AUTHOR: Data Analytics Team
DATE CREATED: 2024-01-15
LAST MODIFIED: 2024-01-20
MODIFIED BY: John Doe
DATABASE: main_analytics
FREQUENCY: Weekly (Tuesday 8 AM)
OWNER: Finance Department
NOTES: Used for customer segmentation and targeting
================================================================================
*/

SELECT 
    customer_id,
    customer_name,
    SUM(order_total) AS lifetime_value,
    COUNT(order_id) AS total_orders,
    AVG(order_total) AS avg_order_value
FROM orders
GROUP BY customer_id, customer_name
ORDER BY lifetime_value DESC;
```

---

## LEVEL 3: ADVANCED COMMENT PATTERNS

### 3.1 Explaining Complex Business Logic

**Purpose:** Document non-obvious decisions and business rules.

```sql
-- Complex scenario: Calculate average order value excluding outliers
-- Business rule: Exclude top 1% of orders (likely bulk/special deals)
-- and bottom 1% (likely discounted/promotional orders)
-- This gives us a more representative average for standard customers

SELECT 
    customer_id,
    -- Using PERCENTILE_CONT for statistical accuracy
    -- Note: Some databases use different function names
    AVG(order_total) AS normalized_avg_order_value
FROM orders
WHERE order_total NOT IN (
    -- Exclude top 1% (high-value outliers)
    SELECT PERCENTILE_CONT(0.99) WITHIN GROUP (ORDER BY order_total)
    FROM orders
)
AND order_total NOT IN (
    -- Exclude bottom 1% (discount/promo outliers)
    SELECT PERCENTILE_CONT(0.01) WITHIN GROUP (ORDER BY order_total)
    FROM orders
)
GROUP BY customer_id;
```

---

### 3.2 Performance-Related Comments

**Purpose:** Explain optimization decisions.

```sql
/*
PERFORMANCE NOTE: 
- Using INNER JOIN instead of subquery for better execution plan
- Original approach with subquery took 4.2 seconds on 10M rows
- Refactored join version takes 0.8 seconds
- Index: orders(customer_id, order_date) on line 3 query is crucial
*/

SELECT 
    c.customer_id,
    c.customer_name,
    COUNT(o.order_id) AS order_count,
    SUM(o.order_total) AS total_spent
FROM customers c
INNER JOIN orders o ON c.customer_id = o.customer_id
    -- Using INNER JOIN as LEFT JOIN was 5x slower due to
    -- aggregate function behavior with NULL handling
WHERE o.order_date >= DATEADD(YEAR, -1, GETDATE())
GROUP BY c.customer_id, c.customer_name
HAVING COUNT(o.order_id) > 5  -- Customers with 5+ orders only
ORDER BY total_spent DESC;
```

---

### 3.3 Data Quality & Edge Case Comments

**Purpose:** Flag known data issues and how they're handled.

```sql
/*
DATA QUALITY NOTES:
1. NULL customer_email values: ~2% of records
   - Handling: Excluded from email notifications via COALESCE check
2. Duplicate order entries: Some orders appear twice in raw data
   - Handling: Using ROW_NUMBER() window function to deduplicate
3. Future-dated orders: ~50 records with dates > today
   - Handling: Explicitly filtering WHERE order_date <= CURRENT_DATE
4. Negative amounts: ~0.5% of records (refunds/adjustments)
   - Handling: Including in calculations as legitimate transactions
*/

SELECT 
    customer_id,
    customer_name,
    COALESCE(email, 'no_email@unknown.com') AS customer_email,
    total_spent
FROM (
    SELECT DISTINCT ON (customer_id)  -- PostgreSQL syntax, use ROW_NUMBER for others
        c.customer_id,
        c.customer_name,
        c.email,
        SUM(o.order_total) AS total_spent,
        ROW_NUMBER() OVER (PARTITION BY c.customer_id ORDER BY o.order_date DESC) AS rn
    FROM customers c
    LEFT JOIN orders o ON c.customer_id = o.customer_id
    WHERE o.order_date <= CURRENT_DATE
) deduped
WHERE rn = 1;
```

---

### 3.4 Predicate Pushdown & Optimization Comments

**Purpose:** Explain filter placement strategy for distributed systems.

```sql
/*
QUERY OPTIMIZATION: PREDICATE PUSHDOWN STRATEGY
Modern distributed SQL engines (Spark, Presto) benefit from early filtering.
This query is designed with filters positioned to:
1. Filter in source tables first (WHERE clauses on base tables)
2. Minimize data shuffled between nodes
3. Apply aggregate filters AFTER GROUP BY (HAVING)

Execution order optimized for systems like Spark SQL:
- Source filtering (TableScan + Filter) → Most selective
- Early projections (Select specific columns) → Reduces memory
- Join predicates (ON conditions) → Applied before join
- Post-aggregation filters (HAVING) → Applied last
*/

-- Step 1: Filter BEFORE aggregation (most efficient)
SELECT 
    product_category,
    SUM(sales_amount) AS total_sales,
    COUNT(order_id) AS order_count
FROM sales_transactions
WHERE -- Applied at source (pushed down to storage)
    transaction_date >= DATE '2024-01-01'
    AND region_id IN (1, 2, 3)  -- Partition-aware filter
    AND sales_amount > 0  -- Exclude zero/negative values early
GROUP BY product_category
HAVING -- Applied after aggregation
    COUNT(order_id) >= 100  -- Minimum transaction threshold
ORDER BY total_sales DESC;
```

---

### 3.5 Version History & Migration Comments

**Purpose:** Track changes over time for maintenance.

```sql
/*
VERSION HISTORY:
v3.0 (2024-01-20) - John Doe
  - Changed INNER JOIN to LEFT JOIN for historical analysis
  - Added date range parameters for flexibility
  
v2.5 (2024-01-10) - Jane Smith
  - Optimized aggregation using window functions
  - Reduced query time from 12s to 3s
  - Added comments for business logic

v2.0 (2023-12-15) - Initial Production Release
  - Replaced subqueries with CTEs
  - Added indexing recommendations
  
DEPRECATED: Old version uses SELECT * - REMOVE after migration complete

MIGRATION NOTE: 
This query replaces stored procedure sp_customer_analysis
To be phased out by Q1 2025
*/

SELECT ...
```

---

## LEVEL 4: BEST PRACTICES & PRODUCTION CODE

### 4.1 Professional Commenting Standards

**✅ DO:**
- Explain the "why" behind decisions, not just the "what"
- Comment complex business logic and non-obvious joins
- Include data quality notes and edge cases handled
- Document performance-critical sections
- Use consistent formatting and indentation
- Keep comments updated when code changes

**❌ DON'T:**
- Comment obvious code (e.g., `SELECT * FROM customers -- select all customers`)
- Over-comment simple, self-explanatory statements
- Include secrets, passwords, or sensitive information in comments
- Forget to update comments when code logic changes
- Write misleading or outdated comments
- Use cryptic abbreviations without explanation

---

### 4.2 Production-Grade Comment Template

```sql
/*
================================================================================
QUERY METADATA
================================================================================
NAME:           Customer Churn Analysis Report
PURPOSE:        Identify high-risk customers likely to churn in next quarter
OWNER:          Customer Success Team
CREATED:        2024-01-15 by Analytics Team
LAST UPDATED:   2024-01-20 by John Doe
FREQUENCY:      Weekly (Every Monday 6 AM)
RUNTIME:        ~2.5 minutes on 100M row dataset
AFFECTED TEAMS: Sales, Customer Success, Finance
DEPENDENCIES:   orders, customers, payments, returns tables
ALERT OWNERS:   manager@company.com, analyst@company.com
================================================================================

BUSINESS LOGIC:
Churned customers = no purchases for 90+ days + high return rate (>30%)
Rationale: Customers without recent activity + quality concerns are highest risk

DATA QUALITY ISSUES HANDLED:
1. NULL payment dates: Treated as no transaction (counted as dormant)
2. Duplicate orders: Deduplicated using latest order date
3. Negative amounts: Included (refunds/returns tracked separately)
4. Future dates: Excluded (data entry errors)

PERFORMANCE NOTES:
- Indexed columns: customer_id, order_date, payment_status
- Join strategy: INNER JOINs used (removed NULL edge cases)
- Predicate pushdown: Filters applied on source tables first
- Previous version (subqueries): 8.2 sec → Current (CTEs): 2.5 sec

KNOWN LIMITATIONS:
- Does not account for seasonal variations
- Churned customers dataset lags by 2-3 days (batch job delay)
- Missing data for 15% of international customers
================================================================================
*/

WITH customer_activity AS (
    -- Calculate days since last purchase and order metrics
    SELECT 
        c.customer_id,
        c.customer_name,
        c.signup_date,
        MAX(o.order_date) AS last_purchase_date,
        DATEDIFF(DAY, MAX(o.order_date), CURRENT_DATE) AS days_since_purchase,
        COUNT(o.order_id) AS total_orders,
        SUM(o.order_total) AS lifetime_value
    FROM customers c
    LEFT JOIN orders o ON c.customer_id = o.customer_id
    WHERE c.signup_date < DATEADD(YEAR, -1, CURRENT_DATE)  -- Active for 1+ year
    GROUP BY c.customer_id, c.customer_name, c.signup_date
),

customer_returns AS (
    -- Calculate return rate (high returns indicate dissatisfaction)
    SELECT 
        c.customer_id,
        COUNT(DISTINCT r.order_id) AS return_count,
        COUNT(DISTINCT o.order_id) AS order_count,
        CAST(COUNT(DISTINCT r.order_id) AS FLOAT) 
            / NULLIF(COUNT(DISTINCT o.order_id), 0) AS return_rate
    FROM customers c
    LEFT JOIN orders o ON c.customer_id = o.customer_id
    LEFT JOIN returns r ON o.order_id = r.order_id
    WHERE o.order_date >= DATEADD(YEAR, -2, CURRENT_DATE)
    GROUP BY c.customer_id
)

-- Final churn risk assessment
SELECT 
    ca.customer_id,
    ca.customer_name,
    ca.days_since_purchase,
    ca.last_purchase_date,
    ca.total_orders,
    ca.lifetime_value,
    cr.return_rate,
    CASE 
        WHEN ca.days_since_purchase >= 90 AND cr.return_rate > 0.30 
            THEN 'HIGH_RISK'
        WHEN ca.days_since_purchase >= 60 AND cr.return_rate > 0.20 
            THEN 'MEDIUM_RISK'
        WHEN ca.days_since_purchase >= 30 
            THEN 'LOW_RISK'
        ELSE 'ACTIVE'
    END AS churn_risk_category
FROM customer_activity ca
LEFT JOIN customer_returns cr ON ca.customer_id = cr.customer_id
WHERE ca.days_since_purchase >= 30  -- Only customers inactive 30+ days
ORDER BY ca.days_since_purchase DESC;
```

---

### 4.3 Database-Specific Comment Features

**MySQL:**
```sql
-- Single-line comment
# Alternative single-line comment
/* Multi-line comment */
```

**PostgreSQL:**
```sql
-- Standard single-line
/* Standard multi-line */
/* Nested comments supported: /* inner */ outer */
```

**SQL Server:**
```sql
-- Standard single-line
/* Standard multi-line
   spanning multiple lines */
-- No nested comment support
```

**Oracle:**
```sql
-- Single-line
/* Multi-line */
-- No nested comments
```

---

## INTERVIEW Q&A SECTION

### Q1: What are the two main types of SQL comments?

**Answer:**
1. **Single-line comments**: Use `--` syntax, extend to end of line
2. **Multi-line comments**: Use `/* ... */` syntax, can span multiple lines

```sql
-- Example single-line
SELECT * FROM customers;

/* Example
   multi-line */
SELECT * FROM orders;
```

---

### Q2: Can you nest multi-line comments in SQL?

**Answer:**
Depends on the database:
- **Most databases (MySQL, SQL Server, PostgreSQL)**: NO, nesting is NOT supported
- **PostgreSQL (with special flag)**: YES, nesting IS supported

```sql
-- This will cause an error in most databases:
/* Outer comment
   /* Inner comment */  -- This closes the outer comment early!
   Rest is NOT commented
*/

-- Workaround: Use different syntax or separate comments
/* Outer comment */
/* Inner comment */
```

---

### Q3: How should you comment complex business logic in production?

**Answer:**
Focus on **WHY**, not WHAT. Include:
- Purpose of the calculation
- Business rules being applied
- Any edge cases or data quality issues handled
- Performance considerations

```sql
-- ❌ BAD: Just restates the code
SELECT COUNT(*) FROM customers; -- Count customers

-- ✅ GOOD: Explains the business logic
-- Customers with 2+ orders in last 90 days represent
-- "engaged" segment for targeted marketing campaign
-- Excludes test/demo accounts (email like '%@internal.%')
SELECT COUNT(*) 
FROM customers 
WHERE order_count >= 2 
  AND last_order_date >= DATEADD(DAY, -90, CURRENT_DATE)
  AND email NOT LIKE '%@internal.%';
```

---

### Q4: What's the best practice for commenting code you temporarily disable?

**Answer:**
Use multi-line comments and include WHY it's disabled:

```sql
/*
TEMPORARILY DISABLED (2024-01-20 by John Doe)
Reason: Awaiting data pipeline fix - email column has 40% NULLs
Expected re-enable: 2024-01-25
Alternative: Using customer_phone instead

SELECT customer_id, email 
FROM customers;
*/

-- Using alternative until email data is clean
SELECT customer_id, customer_phone 
FROM customers;
```

---

### Q5: How do comments help with query optimization discussion?

**Answer:**
Comments document optimization decisions and their impact:

```sql
/*
PERFORMANCE OPTIMIZATION:
- Original subquery approach: 8.2 seconds
- Refactored with JOIN: 2.5 seconds (69% faster)
- Reason: Avoids repeated table scans from subquery execution

Index requirements:
- CREATE INDEX idx_orders_customer ON orders(customer_id, order_date);
*/

SELECT c.customer_id, COUNT(o.order_id) 
FROM customers c
INNER JOIN orders o ON c.customer_id = o.customer_id  -- Optimized approach
WHERE o.order_date > DATEADD(YEAR, -1, CURRENT_DATE)
GROUP BY c.customer_id;
```

---

### Q6: What information should be in a query header comment?

**Answer:**
- Query name/purpose
- Owner/creator and last modified by
- Frequency of execution
- Any dependencies or related queries
- Known data quality issues
- Performance characteristics

```sql
/*
QUERY: Monthly Revenue Report
PURPOSE: Track monthly revenue trends for executive dashboard
OWNER: Finance Department (finance@company.com)
CREATED: 2024-01-10
LAST UPDATED: 2024-01-20
FREQUENCY: Daily at 6 AM
DEPENDENCIES: sales, orders, products tables
RUNTIME: ~45 seconds on production (100M rows)
KNOWN ISSUES: 
  - International sales missing 5% due to conversion delays
  - Refunds processed with 2-day lag
*/
```

---

### Q7: How do you handle comments for different SQL database systems?

**Answer:**
The two standard syntaxes work across all major databases:
- `--` single-line (most portable)
- `/* */` multi-line (most portable)

```sql
-- STANDARD (works everywhere): Use these

-- Single-line comment works in all databases
SELECT * FROM customers;

/* Multi-line comment 
   works in all databases */
SELECT * FROM orders;

-- Database-specific (avoid for portability):
# MySQL only
/* PostgreSQL allows nesting with special flag */
```

---

### Q8: What's the difference between documenting code and over-commenting?

**Answer:**

**✅ Good documentation:**
- Explains non-obvious business rules
- Clarifies complex JOIN conditions
- Notes data quality issues handled
- Documents optimization decisions
- Why a specific approach was chosen

**❌ Over-commenting:**
- Restating obvious code
- Excessive comments on simple statements
- Irrelevant information
- Comments that go out of sync with code

```sql
-- ❌ Over-commenting (obvious code)
SELECT customer_id, -- This is customer id
       customer_name -- This is customer name
FROM customers;    -- From customers table

-- ✅ Good commenting (informative)
-- BUSINESS RULE: "Active" customers = purchased in last 90 days
-- This segment receives priority customer support
SELECT customer_id, customer_name
FROM customers
WHERE last_purchase_date >= DATEADD(DAY, -90, CURRENT_DATE);
```

---

### Q9: How should comments handle deprecated code?

**Answer:**
Include deprecation notice with timeline and replacement information:

```sql
/*
⚠️ DEPRECATED: This procedure is being phased out
Replacement: Use get_customer_summary_v2 instead
Timeline: Will be removed 2024-03-31
Migration guide: See confluence link [insert URL]
Owner: Data Engineering Team
*/

-- Old version using cursor (slow and outdated)
CREATE PROCEDURE old_customer_analysis
AS BEGIN
    -- ... cursor-based implementation ...
END;

-- NEW VERSION: Use this instead
SELECT customer_id, summary_data
FROM customer_summary_v2
WHERE active = 1;
```

---

### Q10: What's the best comment style for collaborative teams?

**Answer:**
Consistent, standardized format with clear ownership:

```sql
/*
================================================================================
QUERY NAME:     Quarterly Sales Analysis
OWNER:          @john_doe (john@company.com)
MODIFIED:       2024-01-20 by @jane_smith
REVIEWED:       2024-01-20 by @manager
APPROVER:       @executive_sponsor
================================================================================

PURPOSE:
Generate quarterly sales metrics for board reporting

BUSINESS LOGIC:
- Sales include all completed transactions (payment_status = 'COMPLETED')
- Excludes test/internal transactions (source = 'INTERNAL')
- Refunds shown as negative amounts (not excluded)

ASSUMPTIONS:
- Data refreshed nightly from source system
- All dates in UTC timezone
- Currency conversions based on spot rates

KNOWN LIMITATIONS:
- Lag of 2-3 days for international transactions
- 15% of legacy data missing category information
- Sales tax calculations incomplete for 8% of US orders

DATA QUALITY NOTES:
See data quality dashboard: [insert URL]
Last validation: 2024-01-20

PERFORMANCE:
Runtime: 2.3 minutes (100M row dataset)
Last optimization: 2024-01-15 (CTE refactor)
Recommended frequency: Daily

DEPENDENCIES:
- Table: sales (updated nightly 1 AM UTC)
- Table: customers (real-time sync)
- View: reporting_dates (maintained by data team)

RELATED QUERIES:
- annual_sales_analysis.sql
- sales_by_region.sql
- customer_lifetime_value.sql

NEXT REVIEW DATE: 2024-02-20
================================================================================
*/

SELECT ...
```

---

## QUICK REFERENCE CHEAT SHEET

| Aspect | Single-Line (`--`) | Multi-Line (`/* */`) |
|--------|-------------------|-------------------|
| **Syntax** | `-- comment` | `/* comment */` |
| **Length** | One line only | Multiple lines |
| **Nesting** | N/A | Not supported (usually) |
| **Use Case** | Quick notes, inline | Long explanations, blocks |
| **Portability** | All databases | All databases |
| **Best For** | Inline clarifications | Documentation headers |

---

## KEY INTERVIEW TAKEAWAYS

1. **Commenting is about communication** - Explain WHY decisions were made, not WHAT the code does
2. **Two types**: `--` for single-line, `/* */` for multi-line - both universal
3. **Quality > Quantity** - Better to comment complex logic than everything
4. **Production professionalism** - Include metadata, owner, dependencies, known issues
5. **Data quality matters** - Document edge cases and how they're handled
6. **Performance perspective** - Explain optimization decisions and their impact
7. **Consistency wins** - Standardized format across team is more valuable than perfect comments
8. **Keep it current** - Update comments when code changes (more critical than initial creation)
9. **Avoid pitfalls** - No misleading, outdated, or over-obvious comments
10. **Database awareness** - Know your system (MySQL vs PostgreSQL vs SQL Server differences)

---

**Last Updated:** January 2024
**Relevant For:** All SQL Database Systems (MySQL, PostgreSQL, SQL Server, Oracle)
**Interview Confidence Level:** Beginner to Advanced Professional
