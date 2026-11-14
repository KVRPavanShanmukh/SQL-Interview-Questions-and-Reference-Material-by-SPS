# SQL Interview Questions and Reference Material by SPS

A curated, structured repository of SQL reference guides, command cheat sheets, function classifications, and interview preparation materials—organized progressively from foundational concepts to advanced query optimization and database architecture.

---

## Table of Contents

1. [Repository Overview & Direct File Links](#1-repository-overview--direct-file-links)
2. [Progressive Learning Index: Basic to Advanced](#2-progressive-learning-index-basic-to-advanced)
   - [Level 1: Foundations & Core Syntax (Basic)](#level-1-foundations--core-syntax-basic)
   - [Level 2: Data Manipulation, Filtering & Functions (Intermediate)](#level-2-data-manipulation-filtering--functions-intermediate)
   - [Level 3: Joins, Subqueries & Set Operations (Upper-Intermediate)](#level-3-joins-subqueries--set-operations-upper-intermediate)
   - [Level 4: Window Functions, CTEs & Database Design (Advanced)](#level-4-window-functions-ctes--database-design-advanced)
   - [Level 5: Performance Tuning, Transactions & Security (Expert)](#level-5-performance-tuning-transactions--security-expert)
3. [Interview Preparation Highlights](#3-interview-preparation-highlights)
4. [How to Use This Repository](#4-how-to-use-this-repository)
5. [License & Attribution](#5-license--attribution)

---

## 1. Repository Overview & Direct File Links

All reference documents, guides, and PDFs included in this repository are directly accessible below:

| Resource / File Name | Format | Direct Repository Link | Primary Focus |
| :--- | :---: | :--- | :--- |
| **Ultimate SQL Guide, Basics to Advanced BY SPS** | PDF | [Open PDF](./Ultimate%20SQL%20%20Guide%2C%20Basics%20to%20Advanced%20BY%20SPS_watermark.pdf) | End-to-end master reference covering basic syntax through advanced SQL concepts. |
| **Basic to Advance SQL Commands Complete Guide** | Markdown | [View Guide](./Basic%20to%20Advance%20SQL_Commands_Complete_Guide.md) | Comprehensive command reference across DDL, DML, DQL, DCL, and TCL. |
| **SQL Comments Interview Guide** | Markdown | [View Guide](./SQL_Comments_Interview_Guide.md) | Best practices, syntax variations, documentation standards, and interview Q&A on SQL comments. |
| **Classification of Functions in SQL** | Reference | [View File](./Classifcation%20of%20Functions%20in%20SQL) | Structured taxonomy of Scalar (Single-Row) and Aggregate (Multi-Row) SQL functions. |
| **LICENSE** | Text | [View License](./LICENSE) | CC BY-ND 4.0 licensing terms and usage permissions. |

> **Direct GitHub PDF Viewer Link:**  
> [`Ultimate SQL Guide, Basics to Advanced BY SPS_watermark.pdf`](https://github.com/KVRPavanShanmukh/SQL-Interview-Questions-and-Reference-Material-by-SPS/blob/main/Ultimate%20SQL%20%20Guide%2C%20Basics%20to%20Advanced%20BY%20SPS_watermark.pdf)

---

## 2. Progressive Learning Index: Basic to Advanced

Use this skill-based index to navigate the repository materials in sequential order—starting with fundamental SQL building blocks and progressing toward complex analytical and performance-critical concepts.

### Level 1: Foundations & Core Syntax (Basic)
* **Relational Database Fundamentals:** Tables, rows, columns, schemas, primary keys, and foreign keys.
* **SQL Command Categories:**
  * **DDL (Data Definition Language):** `CREATE`, `ALTER`, `DROP`, `TRUNCATE`, `RENAME` — covered in [Basic to Advance SQL Commands Complete Guide](./Basic%20to%20Advance%20SQL_Commands_Complete_Guide.md).
  * **DML (Data Manipulation Language):** `INSERT`, `UPDATE`, `DELETE` — covered in [Basic to Advance SQL Commands Complete Guide](./Basic%20to%20Advance%20SQL_Commands_Complete_Guide.md).
  * **DQL (Data Query Language):** `SELECT`, `DISTINCT`, column aliases, and basic expressions — covered in [Ultimate SQL Guide (PDF)](./Ultimate%20SQL%20%20Guide%2C%20Basics%20to%20Advanced%20BY%20SPS_watermark.pdf).
* **Code Readability & Documentation:** Single-line (`--`), multi-line (`/* ... */`), and dialect-specific comment rules — covered in [SQL Comments Interview Guide](./SQL_Comments_Interview_Guide.md).

### Level 2: Data Manipulation, Filtering & Functions (Intermediate)
* **Filtering & Sorting:** `WHERE`, comparison/logical operators (`AND`, `OR`, `NOT`, `IN`, `BETWEEN`, `LIKE`), `NULL` handling (`IS NULL`, `COALESCE`), and `ORDER BY` — covered in [Basic to Advance SQL Commands Complete Guide](./Basic%20to%20Advance%20SQL_Commands_Complete_Guide.md).
* **SQL Function Taxonomy:** Complete breakdown in [Classification of Functions in SQL](./Classifcation%20of%20Functions%20in%20SQL) and [Ultimate SQL Guide (PDF)](./Ultimate%20SQL%20%20Guide%2C%20Basics%20to%20Advanced%20BY%20SPS_watermark.pdf):
  * **Scalar (Single-Row) Functions:** String manipulation, numeric/math functions, date & time functions, and type conversion (`CAST`, `CONVERT`).
  * **Aggregate (Multi-Row) Functions:** `COUNT()`, `SUM()`, `AVG()`, `MIN()`, `MAX()`.
* **Grouping & Post-Aggregation Filtering:** `GROUP BY` and `HAVING` clauses (including execution order vs. `WHERE`).

### Level 3: Joins, Subqueries & Set Operations (Upper-Intermediate)
* **Multi-Table Joins:** `INNER JOIN`, `LEFT JOIN`, `RIGHT JOIN`, `FULL OUTER JOIN`, `CROSS JOIN`, and `SELF JOIN` — covered in [Ultimate SQL Guide (PDF)](./Ultimate%20SQL%20%20Guide%2C%20Basics%20to%20Advanced%20BY%20SPS_watermark.pdf).
* **Subqueries:** Single-row, multi-row, scalar, and correlated subqueries (`EXISTS`, `NOT EXISTS`, `ANY`, `ALL`).
* **Set Operations:** Combining result sets with `UNION`, `UNION ALL`, `INTERSECT`, and `EXCEPT` / `MINUS` — covered in [Basic to Advance SQL Commands Complete Guide](./Basic%20to%20Advance%20SQL_Commands_Complete_Guide.md).
* **Constraints & Data Integrity:** `NOT NULL`, `UNIQUE`, `PRIMARY KEY`, `FOREIGN KEY`, `CHECK`, and `DEFAULT`.

### Level 4: Window Functions, CTEs & Database Design (Advanced)
* **Common Table Expressions (CTEs):** Non-recursive `WITH` clauses and recursive CTEs for hierarchical/tree data traversal — covered in [Ultimate SQL Guide (PDF)](./Ultimate%20SQL%20%20Guide%2C%20Basics%20to%20Advanced%20BY%20SPS_watermark.pdf).
* **Window (Analytic) Functions:** `OVER()`, `PARTITION BY`, `ORDER BY`, frame specifications (`ROWS`/`RANGE BETWEEN`), ranking (`ROW_NUMBER()`, `RANK()`, `DENSE_RANK()`, `NTILE()`), and offset functions (`LAG()`, `LEAD()`, `FIRST_VALUE()`, `LAST_VALUE()`) — covered in [Classification of Functions in SQL](./Classifcation%20of%20Functions%20in%20SQL) & [Ultimate SQL Guide (PDF)](./Ultimate%20SQL%20%20Guide%2C%20Basics%20to%20Advanced%20BY%20SPS_watermark.pdf).
* **Database Objects:** Views, Materialized Views, Stored Procedures, User-Defined Functions (UDFs), and Triggers.
* **Normalization & Schema Design:** 1NF, 2NF, 3NF, BCNF, and denormalization trade-offs for analytical workloads.

### Level 5: Performance Tuning, Transactions & Security (Expert)
* **Transaction Control (TCL) & ACID Properties:** `COMMIT`, `ROLLBACK`, `SAVEPOINT`, isolation levels (`READ UNCOMMITTED`, `READ COMMITTED`, `REPEATABLE READ`, `SERIALIZABLE`), and concurrency anomalies (dirty reads, phantom reads, deadlocks) — covered in [Basic to Advance SQL Commands Complete Guide](./Basic%20to%20Advance%20SQL_Commands_Complete_Guide.md).
* **Access Control (DCL) & Security:** `GRANT`, `REVOKE`, role-based permissions, and SQL injection prevention (including comment-based injection vectors) — covered in [Basic to Advance SQL Commands Complete Guide](./Basic%20to%20Advance%20SQL_Commands_Complete_Guide.md) and [SQL Comments Interview Guide](./SQL_Comments_Interview_Guide.md).
* **Indexing & Query Optimization:** B-Tree vs. Hash indexes, clustered vs. non-clustered indexes, composite indexes, execution plans (`EXPLAIN` / `EXPLAIN ANALYZE`), and query refactoring — covered in [Ultimate SQL Guide (PDF)](./Ultimate%20SQL%20%20Guide%2C%20Basics%20to%20Advanced%20BY%20SPS_watermark.pdf).

---

## 3. Interview Preparation Highlights

* **SQL Logical Execution Order:** `FROM` / `JOIN` $\rightarrow$ `WHERE` $\rightarrow$ `GROUP BY` $\rightarrow$ `HAVING` $\rightarrow$ `SELECT` $\rightarrow$ `DISTINCT` $\rightarrow$ `ORDER BY` $\rightarrow$ `LIMIT` / `OFFSET`.
* **High-Frequency Interview Patterns:**
  * Finding the $N^{\text{th}}$ highest salary using `DENSE_RANK()`, `LIMIT/OFFSET`, and correlated subqueries.
  * Identifying and deduplicating duplicate records using CTEs + `ROW_NUMBER()`.
  * Handling `NULL` traps in `NOT IN` vs. `NOT EXISTS` and aggregate functions.
  * Understanding security implications of inline comments (`--`, `/* */`) in dynamic SQL queries ([SQL Comments Interview Guide](./SQL_Comments_Interview_Guide.md)).

---

## 4. How to Use This Repository

1. **Beginners:** Start with the [Basic to Advance SQL Commands Complete Guide](./Basic%20to%20Advance%20SQL_Commands_Complete_Guide.md) and [Classification of Functions in SQL](./Classifcation%20of%20Functions%20in%20SQL) to build a solid command and function vocabulary.
2. **Interview Candidates:** Review the [SQL Comments Interview Guide](./SQL_Comments_Interview_Guide.md) alongside Levels 3–5 in the [Ultimate SQL Guide (PDF)](./Ultimate%20SQL%20%20Guide%2C%20Basics%20to%20Advanced%20BY%20SPS_watermark.pdf).
3. **Offline Revision:** Download the [Ultimate SQL Guide, Basics to Advanced BY SPS (PDF)](./Ultimate%20SQL%20%20Guide%2C%20Basics%20to%20Advanced%20BY%20SPS_watermark.pdf) directly for end-to-end study.

---

## 5. License & Attribution

[![License: CC BY-ND 4.0](https://img.shields.io/badge/License-CC%20BY--ND%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by-nd/4.0/)

* **Author:** KVR Pavan Shanmukh (SPS)
* **License:** This repository and all included PDFs, guides, and reference materials are licensed under the **[Creative Commons Attribution-NoDerivatives 4.0 International (CC BY-ND 4.0)](./LICENSE)**. You are free to view, download, and share these materials with proper credit to the author, **provided no modifications, derivatives, or watermark removals are made**.
