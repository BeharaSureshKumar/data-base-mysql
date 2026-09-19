# 2-Day Database Learning Plan (MySQL + Oracle)

## Goal
Become interview-ready and project-ready at a senior (10+ years) database knowledge level, focusing on MySQL while understanding Oracle concepts.

---

# Day 1 (8–10 Hours) – SQL & Database Fundamentals

## Session 1 (2 Hours) – Database Fundamentals
- DBMS vs RDBMS
- Tables, Rows, Columns
- Primary Key
- Foreign Key
- Unique Key
- Composite Key
- Candidate Key
- Surrogate Key
- Constraints
  - NOT NULL
  - UNIQUE
  - CHECK
  - DEFAULT

### Example
```sql
CREATE TABLE employee(
   emp_id INT PRIMARY KEY,
   emp_name VARCHAR(100) NOT NULL,
   email VARCHAR(100) UNIQUE
);
```

---

## Session 2 (2 Hours) – CRUD Operations
Practice:
- INSERT
- UPDATE
- DELETE
- SELECT

```sql
INSERT INTO employee VALUES(1,'Suresh','abc@gmail.com');

UPDATE employee
SET emp_name='Kumar'
WHERE emp_id=1;

DELETE FROM employee
WHERE emp_id=1;
```

Target: Practice 50 queries.

---

## Session 3 (2 Hours) – SQL Queries
Learn:
- WHERE
- AND
- OR
- NOT
- BETWEEN
- IN
- LIKE

```sql
SELECT *
FROM employee
WHERE salary > 50000;
```

---

## Session 4 (2 Hours) – Sorting & Aggregations

Learn:
- ORDER BY
- GROUP BY
- HAVING
- DISTINCT

Functions:
- COUNT()
- SUM()
- AVG()
- MAX()
- MIN()

```sql
SELECT department_id,
       COUNT(*),
       AVG(salary)
FROM employee
GROUP BY department_id;
```

---

## Day 1 Evening (2 Hours) – Joins

- Inner Join
- Left Join
- Right Join
- Full Join
- Self Join
- Cross Join

```sql
SELECT *
FROM employee e
INNER JOIN department d
ON e.dept_id = d.id;
```

Practice using:
- Employee–Department
- Orders–Customers
- Batch–Staging Data

---

# Day 2 (Advanced Topics)

## Session 1 (2 Hours) – Subqueries

### Single Row Subquery

```sql
SELECT *
FROM employee
WHERE salary >
(
   SELECT AVG(salary)
   FROM employee
);
```

### Correlated Subquery

```sql
SELECT *
FROM employee e
WHERE salary >
(
  SELECT AVG(salary)
  FROM employee
  WHERE dept_id=e.dept_id
);
```

---

## Session 2 (2 Hours) – Window Functions

Learn:
- ROW_NUMBER
- RANK
- DENSE_RANK
- LEAD
- LAG
- NTILE
- Running Totals

```sql
SELECT *,
ROW_NUMBER() OVER
(PARTITION BY dept_id ORDER BY salary DESC)
FROM employee;
```

Practice:
- Highest salary per department
- Top 3 employees per department

---

## Session 3 (2 Hours) – Indexes

Learn:
- Clustered Index
- Non-Clustered Index
- Composite Index
- Covering Index
- B-Tree Structure
- Index Scan
- Full Table Scan

```sql
CREATE INDEX idx_emp_name
ON employee(emp_name);
```

---

## Session 4 (2 Hours) – Transactions

### ACID Properties
- Atomicity
- Consistency
- Isolation
- Durability

```sql
START TRANSACTION;

UPDATE account
SET balance = balance - 1000
WHERE id = 1;

UPDATE account
SET balance = balance + 1000
WHERE id = 2;

COMMIT;
```

Learn:
- COMMIT
- ROLLBACK
- SAVEPOINT

---

# Senior-Level Topics

## Normalization
- 1NF
- 2NF
- 3NF
- BCNF

Benefits:
- Reduced redundancy
- Better consistency
- Easier maintenance

---

## Denormalization

Used for:
- Reporting
- Analytics
- Performance optimization

---

## Execution Plans

### MySQL

```sql
EXPLAIN
SELECT *
FROM employee
WHERE emp_name='Suresh';
```

### Oracle

```sql
EXPLAIN PLAN FOR
SELECT * FROM employee;

SELECT *
FROM TABLE(DBMS_XPLAN.DISPLAY);
```

Understand:
- Table Scan
- Index Scan
- Cost
- Cardinality

---

## Stored Procedures

### MySQL

```sql
DELIMITER //

CREATE PROCEDURE getEmployees()
BEGIN
   SELECT * FROM employee;
END //

DELIMITER ;
```

### Oracle

```sql
CREATE OR REPLACE PROCEDURE getEmployees
AS
BEGIN
   SELECT * FROM employee;
END;
/
```

---

## Views

```sql
CREATE VIEW employee_view AS
SELECT emp_id, emp_name
FROM employee;
```

---

## Triggers

Learn:
- Before Insert
- After Insert
- Before Update
- After Update

---

# Oracle Concepts

## Oracle Architecture
- Instance
- Database
- Tablespaces
- Data Files
- Redo Logs
- Undo
- Control Files

## Oracle Objects
- Table
- View
- Sequence
- Synonym
- Materialized View
- Package
- Function
- Procedure
- Trigger

### Sequence

```sql
CREATE SEQUENCE emp_seq
START WITH 1
INCREMENT BY 1;
```

---

## Oracle Performance
Learn:
- Explain Plan
- AWR Reports
- SQL Tuning
- Partitioning
- Indexing
- Hints

```sql
SELECT /*+ INDEX(employee idx_emp) */
*
FROM employee;
```

---

# Spring Boot + Database Topics

- JDBC
- Connection Pooling (HikariCP)
- JPA
- Hibernate
- Lazy Loading
- Eager Loading
- N+1 Problem
- Batch Processing
- Optimistic Locking
- Pessimistic Locking

---

# Practice Resources

- MySQL Documentation
- Oracle Database Documentation
- SQLBolt
- HackerRank SQL
- LeetCode SQL

---

# Final Checklist

✅ Joins

✅ Subqueries

✅ Window Functions

✅ Indexing

✅ Execution Plans

✅ Transactions & ACID

✅ Normalization / Denormalization

✅ Stored Procedures

✅ Views & Triggers

✅ Locking Mechanisms

✅ Oracle Architecture

✅ SQL Optimization

✅ JPA / Hibernate Performance

✅ Database Design Patterns

## Recommended Focus
- SQL Fundamentals: 30%
- Performance Tuning & Indexing: 40%
- Oracle Architecture & Transactions: 30%
