## Introduction

Window Functions perform calculations across a set of table rows related to the current row without collapsing the result set.

Unlike `GROUP BY`, window functions return individual rows along with calculated values.

### Sample Employee Table

```sql
CREATE TABLE employees (
    emp_id INT,
    emp_name VARCHAR(50),
    department VARCHAR(50),
    salary DECIMAL(10,2),
    joining_date DATE
);
```

| emp_id | emp_name | department | salary |
| ------ | -------- | ---------- | ------ |
| 1      | Rahul    | IT         | 50000  |
| 2      | Priya    | IT         | 60000  |
| 3      | Suresh   | IT         | 60000  |
| 4      | Kavya    | HR         | 45000  |
| 5      | Arjun    | HR         | 55000  |
| 6      | Neha     | Finance    | 70000  |

---

# 1. ROW_NUMBER()

## Definition

Assigns a unique sequential number to each row within a partition.

Even if two rows have the same value, the row numbers will be different.

## Syntax

```sql
ROW_NUMBER() OVER (
    PARTITION BY column_name
    ORDER BY column_name
)
```

## Example

```sql
SELECT
    emp_name,
    department,
    salary,
    ROW_NUMBER() OVER (
        PARTITION BY department
        ORDER BY salary DESC
    ) AS row_num
FROM employees;
```

## Output

| Employee | Dept | Salary | Row Number |
| -------- | ---- | ------ | ---------- |
| Priya    | IT   | 60000  | 1          |
| Suresh   | IT   | 60000  | 2          |
| Rahul    | IT   | 50000  | 3          |
| Arjun    | HR   | 55000  | 1          |
| Kavya    | HR   | 45000  | 2          |

## Use Cases

* Pagination
* Removing duplicates
* Finding top N records

### Top 2 Salaries Per Department

```sql
SELECT *
FROM (
    SELECT *,
           ROW_NUMBER() OVER (
               PARTITION BY department
               ORDER BY salary DESC
           ) rn
    FROM employees
) t
WHERE rn <= 2;
```

---

# 2. RANK()

## Definition

Assigns ranks to rows.

If duplicate values exist, the same rank is assigned and the next rank is skipped.

## Syntax

```sql
RANK() OVER (
    PARTITION BY column_name
    ORDER BY column_name
)
```

## Example

```sql
SELECT
    emp_name,
    salary,
    RANK() OVER (
        ORDER BY salary DESC
    ) rank_no
FROM employees;
```

## Output

| Employee | Salary | Rank |
| -------- | ------ | ---- |
| Neha     | 70000  | 1    |
| Priya    | 60000  | 2    |
| Suresh   | 60000  | 2    |
| Arjun    | 55000  | 4    |
| Rahul    | 50000  | 5    |

Notice Rank 3 is skipped.

## Use Cases

* Competition ranking
* Leaderboards
* Exam results

---

# 3. DENSE_RANK()

## Definition

Assigns ranks without gaps.

Duplicate values get the same rank, but the next rank is not skipped.

## Example

```sql
SELECT
    emp_name,
    salary,
    DENSE_RANK() OVER (
        ORDER BY salary DESC
    ) dense_rank_no
FROM employees;
```

## Output

| Employee | Salary | Dense Rank |
| -------- | ------ | ---------- |
| Neha     | 70000  | 1          |
| Priya    | 60000  | 2          |
| Suresh   | 60000  | 2          |
| Arjun    | 55000  | 3          |
| Rahul    | 50000  | 4          |

## Difference Between RANK and DENSE_RANK

| Salary | Rank | Dense Rank |
| ------ | ---- | ---------- |
| 70000  | 1    | 1          |
| 60000  | 2    | 2          |
| 60000  | 2    | 2          |
| 55000  | 4    | 3          |
| 50000  | 5    | 4          |

## Use Cases

* Top salary analysis
* Product rankings
* Scoreboards

---

# 4. LEAD()

## Definition

Accesses the next row's value without using a self join.

## Syntax

```sql
LEAD(column_name, offset, default_value)
OVER (
    ORDER BY column_name
)
```

## Example

```sql
SELECT
    emp_name,
    salary,
    LEAD(salary)
    OVER (ORDER BY salary) AS next_salary
FROM employees;
```

## Output

| Employee | Salary | Next Salary |
| -------- | ------ | ----------- |
| Kavya    | 45000  | 50000       |
| Rahul    | 50000  | 55000       |
| Arjun    | 55000  | 60000       |
| Priya    | 60000  | 60000       |
| Suresh   | 60000  | 70000       |
| Neha     | 70000  | NULL        |

## Salary Difference

```sql
SELECT
    emp_name,
    salary,
    LEAD(salary)
    OVER(ORDER BY salary) - salary AS diff
FROM employees;
```

## Use Cases

* Compare current and next row
* Trend analysis
* Stock market analysis

---

# 5. LAG()

## Definition

Accesses the previous row value without self join.

## Syntax

```sql
LAG(column_name, offset, default_value)
OVER (
    ORDER BY column_name
)
```

## Example

```sql
SELECT
    emp_name,
    salary,
    LAG(salary)
    OVER (ORDER BY salary) AS previous_salary
FROM employees;
```

## Output

| Employee | Salary | Previous Salary |
| -------- | ------ | --------------- |
| Kavya    | 45000  | NULL            |
| Rahul    | 50000  | 45000           |
| Arjun    | 55000  | 50000           |
| Priya    | 60000  | 55000           |
| Suresh   | 60000  | 60000           |
| Neha     | 70000  | 60000           |

## Use Cases

* Previous month comparison
* Revenue comparison
* Growth calculations

---

# 6. NTILE()

## Definition

Distributes rows into specified number of buckets.

## Syntax

```sql
NTILE(number_of_groups)
OVER (
    ORDER BY column_name
)
```

## Example

```sql
SELECT
    emp_name,
    salary,
    NTILE(4)
    OVER (ORDER BY salary DESC) AS quartile
FROM employees;
```

## Output

| Employee | Salary | Quartile |
| -------- | ------ | -------- |
| Neha     | 70000  | 1        |
| Priya    | 60000  | 1        |
| Suresh   | 60000  | 2        |
| Arjun    | 55000  | 2        |
| Rahul    | 50000  | 3        |
| Kavya    | 45000  | 4        |

## Use Cases

* Quartiles
* Percentiles
* Customer segmentation

### Top 25% Employees

```sql
SELECT *
FROM (
    SELECT *,
           NTILE(4)
           OVER (ORDER BY salary DESC) quartile
    FROM employees
) t
WHERE quartile = 1;
```

---

# 7. Running Totals (Cumulative Sum)

## Definition

Calculates cumulative total from the first row to the current row.

## Example Table

| Month | Sales |
| ----- | ----- |
| Jan   | 1000  |
| Feb   | 1500  |
| Mar   | 2000  |
| Apr   | 2500  |

## Query

```sql
SELECT
    month,
    sales,
    SUM(sales)
    OVER (
        ORDER BY month
    ) AS running_total
FROM monthly_sales;
```

## Output

| Month | Sales | Running Total |
| ----- | ----- | ------------- |
| Jan   | 1000  | 1000          |
| Feb   | 1500  | 2500          |
| Mar   | 2000  | 4500          |
| Apr   | 2500  | 7000          |

## Explicit Window Frame

```sql
SELECT
    month,
    sales,
    SUM(sales)
    OVER (
        ORDER BY month
        ROWS BETWEEN UNBOUNDED PRECEDING
        AND CURRENT ROW
    ) AS running_total
FROM monthly_sales;
```

## Use Cases

* Account balance calculation
* Revenue tracking
* Financial reports
* Inventory tracking

---

# Interview Questions

### Q1: Difference Between ROW_NUMBER, RANK, and DENSE_RANK?

| Function   | Duplicate Rank | Gap |
| ---------- | -------------- | --- |
| ROW_NUMBER | No             | No  |
| RANK       | Yes            | Yes |
| DENSE_RANK | Yes            | No  |

---

### Q2: Difference Between LEAD and LAG?

| Function | Purpose            |
| -------- | ------------------ |
| LEAD     | Next row value     |
| LAG      | Previous row value |

---

### Q3: Can Window Functions Replace GROUP BY?

No.

`GROUP BY` reduces rows.

Window Functions retain all rows and add calculated values.

---

### Q4: Which Window Functions Are Most Asked in Interviews?

1. ROW_NUMBER()
2. RANK()
3. DENSE_RANK()
4. LEAD()
5. LAG()
6. Running Totals
7. Moving Average
8. FIRST_VALUE()
9. LAST_VALUE()
10. NTILE()

---

# Summary

| Function   | Purpose                  |
| ---------- | ------------------------ |
| ROW_NUMBER | Unique sequence number   |
| RANK       | Ranking with gaps        |
| DENSE_RANK | Ranking without gaps     |
| LEAD       | Next row value           |
| LAG        | Previous row value       |
| NTILE      | Divide rows into buckets |
| SUM OVER   | Running total            |

These functions are heavily used in reporting, analytics, ETL processes, data warehousing, and SQL interviews.
