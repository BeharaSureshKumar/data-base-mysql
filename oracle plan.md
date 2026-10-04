# Oracle Complete Learning Roadmap (Beginner to Experienced)

## Goal

Become proficient in:

* Oracle SQL
* Advanced SQL
* PL/SQL
* Performance Tuning
* Oracle Architecture
* DBA Concepts
* Real-Time Production Scenarios
* Interview Preparation

**Duration:** 120 Days

---

# Phase 1: Oracle SQL Fundamentals (Days 1–15)

## Day 1: Database Basics

* What is Database?
* RDBMS Concepts
* Oracle Architecture Overview
* Tables, Rows, Columns
* Primary Key
* Foreign Key
* Data Types

## Day 2: SQL Basics

* SELECT
* WHERE
* DISTINCT
* ORDER BY
* FETCH FIRST N ROWS

## Day 3: Filtering Data

* AND
* OR
* NOT
* BETWEEN
* IN
* LIKE
* IS NULL

## Day 4: String Functions

* UPPER
* LOWER
* INITCAP
* LENGTH
* SUBSTR
* REPLACE
* TRIM

## Day 5: Number Functions

* ROUND
* TRUNC
* MOD
* CEIL
* FLOOR

## Day 6: Date Functions

* SYSDATE
* ADD_MONTHS
* MONTHS_BETWEEN
* NEXT_DAY
* LAST_DAY
* EXTRACT

## Day 7: Conversion Functions

* TO_CHAR
* TO_DATE
* TO_NUMBER
* CAST

## Day 8: Conditional Logic

* CASE
* DECODE
* NVL
* NVL2
* COALESCE

## Day 9: Aggregate Functions

* COUNT
* SUM
* AVG
* MIN
* MAX

## Day 10: GROUP BY

* GROUP BY
* HAVING
* Multiple Grouping

## Day 11–12: Joins

* Inner Join
* Left Join
* Right Join
* Full Join
* Self Join
* Cross Join

## Day 13: Set Operators

* UNION
* UNION ALL
* INTERSECT
* MINUS

## Day 14: Subqueries

* Single Row Subquery
* Multiple Row Subquery
* Correlated Subquery

## Day 15: Revision

* Practice 50 SQL Queries

---

# Phase 2: Intermediate SQL (Days 16–30)

## Day 16–18: Advanced Joins

* Multi-table Joins
* Self Joins
* Semi Joins
* Anti Joins

## Day 19–20: Common Table Expressions (CTE)

```sql
WITH emp_cte AS
(
    SELECT *
    FROM employees
)
SELECT *
FROM emp_cte;
```

## Day 21–22: Recursive CTE

* Hierarchical Data
* Parent Child Relationships

## Day 23–24: Views

* Simple Views
* Complex Views
* Materialized Views

## Day 25: Constraints

* Primary Key
* Foreign Key
* Unique
* Check
* Not Null

## Day 26: Sequences

```sql
CREATE SEQUENCE emp_seq
START WITH 1
INCREMENT BY 1;
```

## Day 27: Synonyms

* Public Synonyms
* Private Synonyms

## Day 28: Indexes

* B-Tree Index
* Bitmap Index
* Function Based Index

## Day 29: Partitioning

* Range Partition
* List Partition
* Hash Partition

## Day 30: Practice

* SQL Exercises

---

# Phase 3: Window Functions (Days 31–40)

## Day 31

* ROW_NUMBER()

## Day 32

* RANK()

## Day 33

* DENSE_RANK()

## Day 34

* LEAD()

## Day 35

* LAG()

## Day 36

* FIRST_VALUE()

## Day 37

* LAST_VALUE()

## Day 38

* NTILE()

## Day 39

### Running Totals

```sql
SELECT emp_id,
       salary,
       SUM(salary)
       OVER(ORDER BY emp_id) running_total
FROM employees;
```

## Day 40

### Moving Average

```sql
SELECT emp_id,
       AVG(salary)
       OVER(
           ORDER BY emp_id
           ROWS BETWEEN 2 PRECEDING AND CURRENT ROW
       ) moving_avg
FROM employees;
```

---

# Phase 4: PL/SQL (Days 41–60)

## Day 41–42: PL/SQL Basics

```sql
BEGIN
   DBMS_OUTPUT.PUT_LINE('Hello Oracle');
END;
/
```

## Day 43

* Variables

## Day 44

* IF ELSE

## Day 45

* Loops

  * FOR LOOP
  * WHILE LOOP
  * LOOP

## Day 46

* Collections

## Day 47

* Records

## Day 48

* Implicit Cursors

## Day 49

* Explicit Cursors

## Day 50

* Parameterized Cursors

## Day 51

* Exception Handling

## Day 52

* Functions

## Day 53

* Procedures

## Day 54

* Packages

## Day 55

* Triggers

## Day 56

* Compound Triggers

## Day 57

* Dynamic SQL

## Day 58

* BULK COLLECT

## Day 59

* FORALL

## Day 60

* PL/SQL Revision

---

# Phase 5: Advanced Oracle SQL (Days 61–75)

## Day 61

### Hierarchical Queries

```sql
SELECT employee_id,
       manager_id
FROM employees
START WITH manager_id IS NULL
CONNECT BY PRIOR employee_id = manager_id;
```

## Day 62

* PIVOT

## Day 63

* UNPIVOT

## Day 64

### MERGE Statement

```sql
MERGE INTO target_table t
USING source_table s
ON (t.id = s.id)
WHEN MATCHED THEN
    UPDATE SET t.name = s.name
WHEN NOT MATCHED THEN
    INSERT (id,name)
    VALUES (s.id,s.name);
```

## Day 65

* Analytical Functions

## Day 66

### Regular Expressions

```sql
SELECT *
FROM employees
WHERE REGEXP_LIKE(email,
                  '^[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}$');
```

## Day 67

* XML Data

## Day 68

* JSON Data

## Day 69

* Flashback Query

## Day 70

* Global Temporary Tables

## Day 71

* External Tables

## Day 72

* Materialized Views

## Day 73

* Parallel Queries

## Day 74

* Advanced Partitioning

## Day 75

* Practice

---

# Phase 6: Query Optimization (Days 76–90)

## Day 76

### Explain Plan

```sql
EXPLAIN PLAN FOR
SELECT *
FROM employees;

SELECT *
FROM TABLE(DBMS_XPLAN.DISPLAY);
```

## Day 77

* AUTOTRACE

## Day 78

* Oracle Optimizer

## Day 79

* Cardinality

## Day 80

* Statistics

## Day 81

* Index Usage

## Day 82

* Covering Indexes

## Day 83

* Bind Variables

## Day 84

* SQL Trace

## Day 85

* TKPROF

## Day 86

* AWR Reports

## Day 87

* ASH Reports

## Day 88

* SQL Tuning Advisor

## Day 89

* Performance Tuning Scenarios

## Day 90

* Mock Interview

---

# Phase 7: Oracle DBA Basics (Days 91–105)

## Day 91

* Oracle Architecture

## Day 92

* Instance vs Database

## Day 93

### Memory Structures

* SGA
* PGA

## Day 94

### Background Processes

* PMON
* SMON
* DBWR
* LGWR
* CKPT

## Day 95

* Tablespaces

## Day 96

* Datafiles

## Day 97

* Redo Logs

## Day 98

* Undo Tablespace

## Day 99

* Control Files

## Day 100

* User Management

## Day 101

* Backup Concepts

## Day 102

* RMAN Basics

## Day 103

* Recovery Concepts

## Day 104

### Data Pump

* EXPDP
* IMPDP

## Day 105

* DBA Revision

---

# Phase 8: Experienced-Level Topics (Days 106–120)

## Oracle Internals

* SQL Parsing
* Hard Parse
* Soft Parse
* Cursor Sharing

## Concurrency

* Locks
* Deadlocks
* Latches
* Mutexes

## Transactions

* ACID Properties
* Commit
* Rollback
* Savepoint

## Advanced Performance Tuning

* Wait Events
* AWR Analysis
* ASH Analysis
* SQL Profiles
* SQL Baselines

## High Availability

* RAC
* Data Guard
* GoldenGate

## Security

* Roles
* Profiles
* Auditing
* VPD
* TDE

## Real-Time Scenarios

* Slow Query Tuning
* Deadlock Resolution
* Batch Performance Issues
* Partitioning Strategy
* Data Migration
* ETL Optimization

---

# Daily Study Plan

| Activity                | Time       |
| ----------------------- | ---------- |
| Theory                  | 1 Hour     |
| Hands-on Practice       | 2 Hours    |
| Scenario-Based Learning | 1 Hour     |
| Revision                | 30 Minutes |

---

# Final Interview Preparation Checklist

## SQL

* 300 SQL Queries
* 50 Join Problems
* 50 Subquery Problems
* 50 Window Function Problems

## PL/SQL

* 100 Programs
* Procedures
* Functions
* Packages
* Triggers

## Performance Tuning

* 50 Tuning Scenarios
* Explain Plans
* Index Optimization
* AWR Analysis

## DBA

* 50 DBA Questions
* Backup and Recovery
* Architecture
* RMAN

## Real-Time Production Issues

* Deadlocks
* Blocking Sessions
* High CPU Queries
* Slow Reports
* Index Problems
* Data Migration Issues

---

# End Goal

