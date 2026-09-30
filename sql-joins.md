 # SQL JOINs — Complete Guide

 > A practical and detailed guide to SQL `INNER JOIN`, `LEFT JOIN`, `RIGHT JOIN`, `FULL OUTER JOIN`, `SELF JOIN`, and `CROSS JOIN` with examples.

---

 ## Table of Contents

 1. What is a JOIN?
2. Sample Database
3. Creating Tables
4. Sample Data
5. Understanding Relationships
6. INNER JOIN
7. LEFT JOIN
8. RIGHT JOIN
9. FULL OUTER JOIN
10. SELF JOIN
11. CROSS JOIN
12. INNER JOIN vs LEFT JOIN
13. LEFT JOIN vs RIGHT JOIN
14. FULL OUTER JOIN vs LEFT JOIN
15. JOIN with WHERE
16. JOIN Conditions
17. Multiple JOINs
18. JOIN with GROUP BY
19. JOIN with HAVING
20. JOIN with ORDER BY
21. JOIN and NULL
22. Finding Unmatched Records
23. SELF JOIN in Detail
24. CROSS JOIN in Detail
25. FULL OUTER JOIN in MySQL
26. Aliases
27. Common JOIN Mistakes
28. Performance Considerations
29. Real-World Examples
30. JOIN Cheat Sheet
31. Interview Questions
32. Practice Exercises
33. Final Summary

---

 # 1\. What is a JOIN?

 A SQL `JOIN` is used to combine rows from two or more tables based on a related column.

 For example, suppose we have:

 ### Employees

 | employee\_id | employee\_name | department\_id |
| --- | --- | --- |
| 1 | John | 10 |
| 2 | Alice | 20 |
| 3 | Bob | 10 |
| 4 | David | 30 |

### Departments

 | department\_id | department\_name |
| --- | --- |
| 10 | IT |
| 20 | HR |
| 30 | Finance |

Both tables have:

```
department_id
```

 We can use this column to connect the tables.

```
employees
    |
    | department_id
    |
    v
departments
```

 Example:

```
SELECT
    e.employee_name,
    d.department_name
FROM employees e
JOIN departments d
    ON e.department_id = d.department_id;
```

 Result:

 | employee\_name | department\_name |
| --- | --- |
| John | IT |
| Alice | HR |
| Bob | IT |
| David | Finance |

---

 # 2\. Sample Database

 We will use the following database throughout this guide.

 We have two main tables:

```
employees
departments
```

 We will also use additional tables later for advanced examples.

---

 ## 2.1 Employees Table

 | employee\_id | employee\_name | department\_id | manager\_id | salary |
| --- | --- | --- | --- | --- |
| 1 | John | 10 | NULL | 60000 |
| 2 | Alice | 20 | 1 | 75000 |
| 3 | Bob | 10 | 1 | 55000 |
| 4 | David | 30 | 2 | 65000 |
| 5 | Emma | NULL | 2 | 50000 |

---

 ## 2.2 Departments Table

 | department\_id | department\_name |
| --- | --- |
| 10 | IT |
| 20 | HR |
| 30 | Finance |
| 40 | Marketing |

Notice two important things:

 ### Employee without department

 Emma has:

```
department_id = NULL
```

 ### Department without employee

 Marketing has:

```
department_id = 40
```

 but no employee belongs to department `40`.

 These two unmatched records will help us understand JOINs.

---

 # 3\. Creating Tables

```
CREATE TABLE departments (
    department_id INT PRIMARY KEY,
    department_name VARCHAR(100)
);

CREATE TABLE employees (
    employee_id INT PRIMARY KEY,
    employee_name VARCHAR(100),
    department_id INT,
    manager_id INT,
    salary DECIMAL(10, 2)
);
```

---

 # 4\. Sample Data

 ## Insert Departments

```
INSERT INTO departments (
    department_id,
    department_name
)
VALUES
    (10, 'IT'),
    (20, 'HR'),
    (30, 'Finance'),
    (40, 'Marketing');
```

---

 ## Insert Employees

```
INSERT INTO employees (
    employee_id,
    employee_name,
    department_id,
    manager_id,
    salary
)
VALUES
    (1, 'John', 10, NULL, 60000),
    (2, 'Alice', 20, 1, 75000),
    (3, 'Bob', 10, 1, 55000),
    (4, 'David', 30, 2, 65000),
    (5, 'Emma', NULL, 2, 50000);
```

---

 # 5\. Understanding Relationships

 Our tables are related using:

```
employees.department_id
        =
departments.department_id
```

 The relationship looks like this:

```
+------------------+              +------------------+
|    employees     |              |   departments    |
+------------------+              +------------------+
| employee_id      |              | department_id    |
| employee_name    |              | department_name  |
| department_id    | ------------ |                  |
| manager_id       |              |                  |
| salary           |              |                  |
+------------------+              +------------------+
```

 The JOIN condition is:

```
ON e.department_id = d.department_id
```

---

 # 6\. INNER JOIN

 ## 6.1 Definition

 `INNER JOIN` returns only rows that have a matching row in both tables.

 Conceptually:

```
Table A              Table B

+---------+          +---------+
|         |          |         |
|         +----------+         |
|         | MATCH    |         |
|         +----------+         |
|         |          |         |
+---------+          +---------+

       Only matching rows
```

---

 ## 6.2 Syntax

```
SELECT columns
FROM table1
INNER JOIN table2
    ON table1.column = table2.column;
```

 `INNER` is optional:

```
SELECT columns
FROM table1
JOIN table2
    ON table1.column = table2.column;
```

---

 ## 6.3 Basic Example

```
SELECT
    e.employee_id,
    e.employee_name,
    d.department_name
FROM employees e
INNER JOIN departments d
    ON e.department_id = d.department_id;
```

---

 ## 6.4 Result

 | employee\_id | employee\_name | department\_name |
| --- | --- | --- |
| 1 | John | IT |
| 2 | Alice | HR |
| 3 | Bob | IT |
| 4 | David | Finance |

Emma is not returned because:

```
Emma.department_id = NULL
```

 There is no matching department.

 Marketing is also not returned because no employee has:

```
department_id = 40
```

---

 ## 6.5 INNER JOIN Diagram

```
Employees                    Departments

John ----------------------> IT
Alice ---------------------> HR
Bob -----------------------> IT
David ---------------------> Finance

Emma ----------------------> No Match
Marketing <---------------- No Match
```

 Only matching relationships are returned.

---

 ## 6.6 When to Use INNER JOIN

 Use `INNER JOIN` when you only want records that have a match.

 Examples:

 - Employees who belong to a department.
- Orders that have a customer.
- Products that have a category.
- Students enrolled in courses.
- Payments associated with invoices.

---

 # 7\. LEFT JOIN

 ## 7.1 Definition

 `LEFT JOIN` returns:

 1. All rows from the left table.
2. Matching rows from the right table.
3. `NULL` for right-table columns when there is no match.

---

 ## 7.2 Syntax

```
SELECT columns
FROM table1
LEFT JOIN table2
    ON table1.column = table2.column;
```

---

 ## 7.3 Example

```
SELECT
    e.employee_id,
    e.employee_name,
    d.department_name
FROM employees e
LEFT JOIN departments d
    ON e.department_id = d.department_id;
```

---

 ## 7.4 Result

 | employee\_id | employee\_name | department\_name |
| --- | --- | --- |
| 1 | John | IT |
| 2 | Alice | HR |
| 3 | Bob | IT |
| 4 | David | Finance |
| 5 | Emma | NULL |

Emma appears even though she doesn't have a department.

 Why?

 Because `employees` is the left table:

```
FROM employees e
LEFT JOIN departments d
```

 The LEFT JOIN says:

 > Keep every employee and attach department information when available.

---

 ## 7.5 LEFT JOIN Diagram

```
Employees                         Departments

+------------------+              +------------------+
| John             | -----------> | IT               |
| Alice            | -----------> | HR               |
| Bob              | -----------> | IT               |
| David            | -----------> | Finance          |
| Emma             | -----------> | NULL             |
+------------------+              +------------------+

All employees are preserved.
```

---

 ## 7.6 Find Employees Without Departments

 This is one of the most common LEFT JOIN patterns.

```
SELECT
    e.employee_id,
    e.employee_name
FROM employees e
LEFT JOIN departments d
    ON e.department_id = d.department_id
WHERE d.department_id IS NULL;
```

 Result:

 | employee\_id | employee\_name |
| --- | --- |
| 5 | Emma |

Pattern:

```
LEFT JOIN ...
WHERE right_table.id IS NULL
```

 means:

 > Find records in the left table that don't have a matching record in the right table.

---

 ## 7.7 When to Use LEFT JOIN

 Use LEFT JOIN when all records from your main table must be returned.

 Examples:

 - All customers, including customers with no orders.
- All employees, including employees without departments.
- All products, including products with no sales.
- All students, including students who haven't enrolled in a course.

---

 # 8\. RIGHT JOIN

 ## 8.1 Definition

 `RIGHT JOIN` is the opposite of `LEFT JOIN`.

 It returns:

 1. All rows from the right table.
2. Matching rows from the left table.
3. `NULL` for left-table columns when there is no match.

---

 ## 8.2 Syntax

```
SELECT columns
FROM table1
RIGHT JOIN table2
    ON table1.column = table2.column;
```

---

 ## 8.3 Example

```
SELECT
    e.employee_name,
    d.department_id,
    d.department_name
FROM employees e
RIGHT JOIN departments d
    ON e.department_id = d.department_id;
```

---

 ## 8.4 Result

 | employee\_name | department\_id | department\_name |
| --- | --- | --- |
| John | 10 | IT |
| Bob | 10 | IT |
| Alice | 20 | HR |
| David | 30 | Finance |
| NULL | 40 | Marketing |

Marketing appears even though it doesn't have an employee.

 Why?

 Because `departments` is the right table.

```
FROM employees e
RIGHT JOIN departments d
```

 Therefore:

 > Keep every department and attach employee information when available.

---

 ## 8.5 Find Departments Without Employees

```
SELECT
    d.department_id,
    d.department_name
FROM employees e
RIGHT JOIN departments d
    ON e.department_id = d.department_id
WHERE e.employee_id IS NULL;
```

 Result:

 | department\_id | department\_name |
| --- | --- |
| 40 | Marketing |

---

 # 9\. FULL OUTER JOIN

 ## 9.1 Definition

 `FULL OUTER JOIN` returns:

 - Matching rows from both tables.
- Unmatched rows from the left table.
- Unmatched rows from the right table.

 In simple terms:

 > Keep everything from both tables.

---

 ## 9.2 Syntax

```
SELECT columns
FROM table1
FULL OUTER JOIN table2
    ON table1.column = table2.column;
```

---

 ## 9.3 Example

```
SELECT
    e.employee_name,
    d.department_name
FROM employees e
FULL OUTER JOIN departments d
    ON e.department_id = d.department_id;
```

---

 ## 9.4 Result

 Conceptually:

 | employee\_name | department\_name |
| --- | --- |
| John | IT |
| Alice | HR |
| Bob | IT |
| David | Finance |
| Emma | NULL |
| NULL | Marketing |

Both unmatched records are preserved:

```
Emma       -> no department
Marketing  -> no employee
```

---

 ## 9.5 FULL OUTER JOIN Diagram

```
Employees                 Departments

+-----------+             +-----------+
|           |             |           |
|   LEFT    |-------------|   RIGHT   |
|           |   MATCH     |           |
+-----------+             +-----------+

Everything from both sides is retained.
```

---

 # 10\. SELF JOIN

 ## 10.1 Definition

 A `SELF JOIN` means joining a table to itself.

 There is no special:

```
SELF JOIN
```

 keyword.

 Instead, we use a normal JOIN and give the same table different aliases.

---

 ## 10.2 Why Do We Need SELF JOIN?

 Look at the employees table:

 | employee\_id | employee\_name | manager\_id |
| --- | --- | --- |
| 1 | John | NULL |
| 2 | Alice | 1 |
| 3 | Bob | 1 |
| 4 | David | 2 |
| 5 | Emma | 2 |

For Alice:

```
Alice.manager_id = 1
```

 Employee `1` is John.

 Therefore:

```
Alice -> John
```

 For David:

```
David.manager_id = 2
```

 Employee `2` is Alice.

 Therefore:

```
David -> Alice
```

---

 ## 10.3 SELF JOIN Query

```
SELECT
    e.employee_name AS employee,
    m.employee_name AS manager
FROM employees e
LEFT JOIN employees m
    ON e.manager_id = m.employee_id;
```

 Notice:

```
employees e
```

 and:

```
employees m
```

 are the same table.

---

 ## 10.4 Result

 | employee | manager |
| --- | --- |
| John | NULL |
| Alice | John |
| Bob | John |
| David | Alice |
| Emma | Alice |

---

 ## 10.5 How SELF JOIN Works

 Think of the table as two copies:

```
             employees
             /       \
            /         \
           /           \
     employee          manager
        e                 m
```

 The JOIN condition:

```
ON e.manager_id = m.employee_id
```

 means:

 > Find the employee record whose ID is stored as this employee's manager ID.

---

 ## 10.6 Find Employees Who Earn More Than Their Manager

```
SELECT
    e.employee_name AS employee,
    e.salary AS employee_salary,
    m.employee_name AS manager,
    m.salary AS manager_salary
FROM employees e
JOIN employees m
    ON e.manager_id = m.employee_id
WHERE e.salary > m.salary;
```

 This compares two rows from the same table.

---

 ## 10.7 Common SELF JOIN Use Cases

 SELF JOIN is useful for:

 - Employee-manager relationships.
- Parent-child categories.
- Organizational hierarchies.
- Referral systems.
- Social relationships.
- Category/subcategory structures.
- Comparing records within the same table.

---

 # 11\. CROSS JOIN

 ## 11.1 Definition

 `CROSS JOIN` returns the Cartesian product of two tables.

 Every row from table A is combined with every row from table B.

---

 ## 11.2 Simple Example

 Table A:

 | color |
| --- |
| Red |
| Blue |

Table B:

 | size |
| --- |
| Small |
| Medium |
| Large |

CROSS JOIN result:

 | color | size |
| --- | --- |
| Red | Small |
| Red | Medium |
| Red | Large |
| Blue | Small |
| Blue | Medium |
| Blue | Large |

Number of rows:

```
2 × 3 = 6
```

---

 ## 11.3 Syntax

```
SELECT *
FROM colors
CROSS JOIN sizes;
```

 No `ON` condition is required.

---

 ## 11.4 CROSS JOIN with Employees and Departments

 We have:

```
5 employees
4 departments
```

 Therefore:

```
5 × 4 = 20 rows
```

 Query:

```
SELECT
    e.employee_name,
    d.department_name
FROM employees e
CROSS JOIN departments d;
```

 The result contains combinations such as:

```
John   -> IT
John   -> HR
John   -> Finance
John   -> Marketing

Alice  -> IT
Alice  -> HR
Alice  -> Finance
Alice  -> Marketing

Bob    -> IT
Bob    -> HR
...
```

---

 ## 11.5 CROSS JOIN Use Cases

 CROSS JOIN can be useful for:

 - Generating combinations.
- Product variants.
- Sizes and colors.
- Calendar generation.
- Test data.
- Configuration combinations.
- Generating possible schedules.

---

 ## 11.6 CROSS JOIN Warning

 CROSS JOIN can create a very large result.

 For example:

```
10,000 employees
×
10,000 products
=
100,000,000 rows
```

 Always use CROSS JOIN intentionally.

---

 # 12\. INNER JOIN vs LEFT JOIN

 Consider:

```
SELECT *
FROM employees e
INNER JOIN departments d
    ON e.department_id = d.department_id;
```

 and:

```
SELECT *
FROM employees e
LEFT JOIN departments d
    ON e.department_id = d.department_id;
```

---

 ## INNER JOIN

 Emma is removed:

```
Emma -> NULL -> excluded
```

 Result contains:

```
John
Alice
Bob
David
```

---

 ## LEFT JOIN

 Emma remains:

```
Emma -> NULL
```

 Result contains:

```
John
Alice
Bob
David
Emma
```

---

 ## Rule

 Use:

```
INNER JOIN
```

 when unmatched rows should be removed.

 Use:

```
LEFT JOIN
```

 when all rows from the left table must remain.

---

 # 13\. LEFT JOIN vs RIGHT JOIN

 These are mirror images.

```
A LEFT JOIN B
```

 means:

```
Keep everything from A.
```

 While:

```
A RIGHT JOIN B
```

 means:

```
Keep everything from B.
```

---

 ## Example

 This:

```
SELECT *
FROM employees e
RIGHT JOIN departments d
    ON e.department_id = d.department_id;
```

 can usually be rewritten as:

```
SELECT *
FROM departments d
LEFT JOIN employees e
    ON e.department_id = d.department_id;
```

 Both preserve all departments.

 For readability, many developers prefer LEFT JOIN and simply put the table they want to preserve on the left.

---

 # 14\. FULL OUTER JOIN vs LEFT JOIN

 LEFT JOIN:

```
All LEFT rows
+
Matching RIGHT rows
```

 FULL OUTER JOIN:

```
All LEFT rows
+
Matching RIGHT rows
+
Unmatched RIGHT rows
```

 Example:

```
Employees:

John
Alice
Bob
David
Emma

Departments:

IT
HR
Finance
Marketing
```

 LEFT JOIN:

```
John      IT
Alice     HR
Bob       IT
David     Finance
Emma      NULL
```

 FULL OUTER JOIN:

```
John      IT
Alice     HR
Bob       IT
David     Finance
Emma      NULL
NULL      Marketing
```

---

 # 15\. JOIN with WHERE

 The `ON` clause and `WHERE` clause have different purposes.

 Example:

```
SELECT
    e.employee_name,
    d.department_name
FROM employees e
INNER JOIN departments d
    ON e.department_id = d.department_id
WHERE e.salary > 60000;
```

 The JOIN condition:

```
ON e.department_id = d.department_id
```

 determines how rows are matched.

 The WHERE condition:

```
WHERE e.salary > 60000
```

 filters the result.

---

 ## Example Result

 Employees with salary greater than `60000`:

 | employee\_name | department\_name |
| --- | --- |
| Alice | HR |
| David | Finance |

---

 # 16\. JOIN Conditions

 The most common JOIN condition is equality:

```
ON e.department_id = d.department_id
```

 But JOIN conditions can contain multiple conditions.

---

 ## 16.1 Multiple Conditions

```
SELECT
    e.employee_name,
    d.department_name
FROM employees e
JOIN departments d
    ON e.department_id = d.department_id
    AND e.salary > 50000;
```

 The JOIN requires:

```
department_id must match
AND
salary must be greater than 50,000
```

---

 ## 16.2 JOIN Using Different Column Names

 Suppose:

```
employees.dept_id
departments.department_id
```

 Then:

```
SELECT *
FROM employees e
JOIN departments d
    ON e.dept_id = d.department_id;
```

 The column names don't have to be identical.

 They only need to represent a valid relationship.

---

 ## 16.3 JOIN Using Non-Equality Conditions

 JOIN conditions don't always have to use `=`.

 Example:

```
SELECT
    e.employee_name,
    e.salary,
    g.grade
FROM employees e
JOIN salary_grades g
    ON e.salary BETWEEN g.min_salary AND g.max_salary;
```

 This is sometimes called a range or non-equi JOIN.

---

 # 17\. Multiple JOINs

 You can join more than two tables.

 Suppose we have:

```
employees
departments
locations
```

 Relationship:

```
employees
    |
    v
departments
    |
    v
locations
```

 Query:

```
SELECT
    e.employee_name,
    d.department_name,
    l.location_name
FROM employees e
JOIN departments d
    ON e.department_id = d.department_id
JOIN locations l
    ON d.location_id = l.location_id;
```

---

 ## Another Example

 Suppose we have:

```
customers
orders
products
```

 We can write:

```
SELECT
    c.customer_name,
    o.order_id,
    p.product_name
FROM customers c
JOIN orders o
    ON c.customer_id = o.customer_id
JOIN products p
    ON o.product_id = p.product_id;
```

---

 # 18\. JOIN with GROUP BY

 JOINs are frequently combined with aggregation.

 Example:

 > Count employees in each department.

```
SELECT
    d.department_name,
    COUNT(e.employee_id) AS employee_count
FROM departments d
LEFT JOIN employees e
    ON d.department_id = e.department_id
GROUP BY
    d.department_name;
```

 Result:

 | department\_name | employee\_count |
| --- | --- |
| IT | 2 |
| HR | 1 |
| Finance | 1 |
| Marketing | 0 |

Notice why `LEFT JOIN` is important.

 Marketing has no employees, but we still want Marketing to appear with:

```
employee_count = 0
```

---

 # 19\. JOIN with HAVING

 Suppose we want departments with more than one employee.

```
SELECT
    d.department_name,
    COUNT(e.employee_id) AS employee_count
FROM departments d
LEFT JOIN employees e
    ON d.department_id = e.department_id
GROUP BY
    d.department_name
HAVING COUNT(e.employee_id) > 1;
```

 Result:

 | department\_name | employee\_count |
| --- | --- |
| IT | 2 |

---

 ## WHERE vs HAVING

 `WHERE` filters rows before grouping.

 `HAVING` filters groups after aggregation.

 Example:

```
WHERE e.salary > 50000
```

 filters individual employee rows.

 While:

```
HAVING COUNT(e.employee_id) > 1
```

 filters groups.

---

 # 20\. JOIN with ORDER BY

 Example:

```
SELECT
    e.employee_name,
    d.department_name,
    e.salary
FROM employees e
LEFT JOIN departments d
    ON e.department_id = d.department_id
ORDER BY e.salary DESC;
```

 Result is ordered by salary from highest to lowest.

---

 # 21\. JOIN and NULL

 Understanding `NULL` is extremely important when working with JOINs.

 Emma has:

```
department_id = NULL
```

 When using:

```
LEFT JOIN
```

 Emma is preserved.

 Her department columns become:

```
NULL
```

 Example:

 | employee\_name | department\_name |
| --- | --- |
| Emma | NULL |

---

 ## 21.1 NULL Cannot Be Compared Using `=`

 Incorrect:

```
WHERE department_id = NULL
```

 Correct:

```
WHERE department_id IS NULL
```

 And:

```
WHERE department_id IS NOT NULL
```

---

 # 22\. Finding Unmatched Records

 One of the most useful JOIN techniques is finding records that don't have a match.

---

 ## 22.1 Employees Without Departments

```
SELECT
    e.employee_id,
    e.employee_name
FROM employees e
LEFT JOIN departments d
    ON e.department_id = d.department_id
WHERE d.department_id IS NULL;
```

 Result:

```
Emma
```

---

 ## 22.2 Departments Without Employees

```
SELECT
    d.department_id,
    d.department_name
FROM departments d
LEFT JOIN employees e
    ON d.department_id = e.department_id
WHERE e.employee_id IS NULL;
```

 Result:

```
Marketing
```

---

 ## 22.3 Why This Works

 For a LEFT JOIN:

```
Left record exists
+
Right record doesn't exist
=
Right columns become NULL
```

 Therefore:

```
WHERE d.department_id IS NULL
```

 can identify missing matches.

---

 # 23\. SELF JOIN in Detail

 Let's look at the employee hierarchy again.

 | employee\_id | employee\_name | manager\_id |
| --- | --- | --- |
| 1 | John | NULL |
| 2 | Alice | 1 |
| 3 | Bob | 1 |
| 4 | David | 2 |
| 5 | Emma | 2 |

Hierarchy:

```
John
├── Alice
│   ├── David
│   └── Emma
└── Bob
```

 We can retrieve this using a SELF JOIN.

```
SELECT
    e.employee_name AS employee,
    m.employee_name AS manager
FROM employees e
LEFT JOIN employees m
    ON e.manager_id = m.employee_id;
```

---

 ## 23.1 Result

```
John   -> NULL
Alice  -> John
Bob    -> John
David  -> Alice
Emma   -> Alice
```

---

 ## 23.2 Find John’s Direct Reports

```
SELECT
    e.employee_name
FROM employees e
JOIN employees m
    ON e.manager_id = m.employee_id
WHERE m.employee_name = 'John';
```

 Result:

```
Alice
Bob
```

---

 ## 23.3 Find Alice’s Direct Reports

```
SELECT
    e.employee_name
FROM employees e
JOIN employees m
    ON e.manager_id = m.employee_id
WHERE m.employee_name = 'Alice';
```

 Result:

```
David
Emma
```

---

 # 24\. CROSS JOIN in Detail

 Let's create two small tables.

 ## Colors

```
CREATE TABLE colors (
    color_id INT,
    color_name VARCHAR(50)
);
```

 Insert:

```
INSERT INTO colors
VALUES
    (1, 'Red'),
    (2, 'Blue');
```

---

 ## Sizes

```
CREATE TABLE sizes (
    size_id INT,
    size_name VARCHAR(50)
);
```

 Insert:

```
INSERT INTO sizes
VALUES
    (1, 'Small'),
    (2, 'Medium'),
    (3, 'Large');
```

---

 ## CROSS JOIN

```
SELECT
    c.color_name,
    s.size_name
FROM colors c
CROSS JOIN sizes s;
```

 Result:

 | color\_name | size\_name |
| --- | --- |
| Red | Small |
| Red | Medium |
| Red | Large |
| Blue | Small |
| Blue | Medium |
| Blue | Large |

Total:

```
2 × 3 = 6 rows
```

---

 ## CROSS JOIN Formula

 If:

```
Table A = M rows
Table B = N rows
```

 Then:

```
CROSS JOIN result = M × N rows
```

 Example:

```
5 × 4 = 20
```

---

 # 25\. FULL OUTER JOIN in MySQL

 Some databases support:

```
FULL OUTER JOIN
```

 However, MySQL does not directly provide a `FULL OUTER JOIN` syntax.

 A common approach is:

```
LEFT JOIN
+
RIGHT JOIN
+
UNION
```

 Example:

```
SELECT
    e.employee_name,
    d.department_name
FROM employees e
LEFT JOIN departments d
    ON e.department_id = d.department_id

UNION

SELECT
    e.employee_name,
    d.department_name
FROM employees e
RIGHT JOIN departments d
    ON e.department_id = d.department_id;
```

 `UNION` removes duplicate rows.

---

 # 26\. Aliases

 Aliases make JOIN queries shorter and easier to read.

 Without aliases:

```
SELECT
    employees.employee_name,
    departments.department_name
FROM employees
JOIN departments
    ON employees.department_id = departments.department_id;
```

 With aliases:

```
SELECT
    e.employee_name,
    d.department_name
FROM employees e
JOIN departments d
    ON e.department_id = d.department_id;
```

 Here:

```
e = employees
d = departments
```

---

 ## SELF JOIN Requires Aliases

 Aliases are especially important for SELF JOIN.

```
SELECT
    e.employee_name AS employee,
    m.employee_name AS manager
FROM employees e
JOIN employees m
    ON e.manager_id = m.employee_id;
```

 Without aliases, it would be difficult to distinguish the two references to the same table.

---

 # 27\. Common JOIN Mistakes

 ## 27.1 Missing JOIN Condition

 Incorrect:

```
SELECT *
FROM employees e
JOIN departments d;
```

 For a normal JOIN, specify the relationship:

```
SELECT *
FROM employees e
JOIN departments d
    ON e.department_id = d.department_id;
```

---

 # 27.2 Wrong JOIN Condition

 Suppose we write:

```
ON e.employee_id = d.department_id
```

 instead of:

```
ON e.department_id = d.department_id
```

 The query may execute successfully but return incorrect results.

 Always understand the relationship between columns.

---

 # 27.3 Accidentally Creating Too Many Rows

 Suppose:

```
Employees = 10,000
Products = 10,000
```

 A CROSS JOIN produces:

```
10,000 × 10,000
=
100,000,000 rows
```

 Be careful when JOIN conditions are missing or incorrect.

---

 # 27.4 Using INNER JOIN When You Need All Records

 Suppose you need all customers, including customers without orders.

 This may be wrong:

```
SELECT
    c.customer_name,
    o.order_id
FROM customers c
INNER JOIN orders o
    ON c.customer_id = o.customer_id;
```

 Customers without orders disappear.

 Use:

```
SELECT
    c.customer_name,
    o.order_id
FROM customers c
LEFT JOIN orders o
    ON c.customer_id = o.customer_id;
```

---

 # 27.5 Filtering a LEFT JOIN in WHERE

 Consider:

```
SELECT
    e.employee_name,
    d.department_name
FROM employees e
LEFT JOIN departments d
    ON e.department_id = d.department_id
WHERE d.department_name = 'IT';
```

 This removes rows where `d.department_name` is NULL.

 So the query behaves much more like an INNER JOIN for that condition.

 Sometimes that is intended; sometimes it is a mistake.

---

 ## Alternative: Put the Condition in ON

 If you want to preserve all employees while only matching IT departments:

```
SELECT
    e.employee_name,
    d.department_name
FROM employees e
LEFT JOIN departments d
    ON e.department_id = d.department_id
    AND d.department_name = 'IT';
```

 This keeps employees even when their department isn't IT.

---

 # 28\. Performance Considerations

 JOIN performance depends on many factors, including:

 - Table size.
- Indexes.
- JOIN conditions.
- Data distribution.
- Query structure.
- Database engine.
- Query optimizer.

---

 ## 28.1 Index JOIN Columns

 If you frequently join:

```
e.department_id = d.department_id
```

 indexes on the relevant columns can help the database find matching rows efficiently.

 For example:

```
CREATE INDEX idx_employees_department_id
ON employees(department_id);
```

 The exact indexing strategy should depend on your workload and database.

---

 ## 28.2 Select Only Required Columns

 Avoid:

```
SELECT *
```

 when you don't need every column.

 Prefer:

```
SELECT
    e.employee_name,
    d.department_name
FROM employees e
JOIN departments d
    ON e.department_id = d.department_id;
```

 Benefits include:

 - Less data transferred.
- Easier-to-read queries.
- Clearer intent.
- Potentially better performance.

---

 ## 28.3 Filter Data When Appropriate

 Instead of retrieving everything:

```
SELECT *
FROM employees e
JOIN departments d
    ON e.department_id = d.department_id;
```

 you may filter:

```
SELECT
    e.employee_name,
    d.department_name
FROM employees e
JOIN departments d
    ON e.department_id = d.department_id
WHERE e.salary > 60000;
```

---

 # 29\. Real-World Examples

 ## 29.1 Customers and Orders

 Tables:

```
customers
---------
customer_id
customer_name

orders
------
order_id
customer_id
order_date
```

---

 ### Find Customers With Orders

```
SELECT
    c.customer_name,
    o.order_id
FROM customers c
INNER JOIN orders o
    ON c.customer_id = o.customer_id;
```

---

 ### Find All Customers

```
SELECT
    c.customer_name,
    o.order_id
FROM customers c
LEFT JOIN orders o
    ON c.customer_id = o.customer_id;
```

---

 ### Find Customers Without Orders

```
SELECT
    c.customer_name
FROM customers c
LEFT JOIN orders o
    ON c.customer_id = o.customer_id
WHERE o.order_id IS NULL;
```

---

 # 29.2 Products and Categories

 Tables:

```
products
--------
product_id
product_name
category_id

categories
----------
category_id
category_name
```

 Query:

```
SELECT
    p.product_name,
    c.category_name
FROM products p
INNER JOIN categories c
    ON p.category_id = c.category_id;
```

---

 # 29.3 Students and Courses

 Tables:

```
students
--------
student_id
student_name

courses
-------
course_id
course_name

enrollments
-----------
student_id
course_id
```

 Query:

```
SELECT
    s.student_name,
    c.course_name
FROM students s
JOIN enrollments e
    ON s.student_id = e.student_id
JOIN courses c
    ON e.course_id = c.course_id;
```

 This is a multi-table JOIN.

---

 # 29.4 Employee and Manager

```
SELECT
    e.employee_name AS employee,
    m.employee_name AS manager
FROM employees e
LEFT JOIN employees m
    ON e.manager_id = m.employee_id;
```

 This is a SELF JOIN.

---

 # 29.5 Product Color and Size Combinations

```
SELECT
    c.color_name,
    s.size_name
FROM colors c
CROSS JOIN sizes s;
```

 This is a CROSS JOIN.

---

 # 30\. JOIN Cheat Sheet

 | JOIN | What It Returns |
| --- | --- |
| `INNER JOIN` | Only matching rows |
| `LEFT JOIN` | All left rows + matching right rows |
| `RIGHT JOIN` | All right rows + matching left rows |
| `FULL OUTER JOIN` | All rows from both tables |
| `SELF JOIN` | A table joined to itself |
| `CROSS JOIN` | Every combination of rows |

---

 # 31\. JOIN Syntax Cheat Sheet

 ## INNER JOIN

```
SELECT *
FROM A
INNER JOIN B
    ON A.id = B.id;
```

---

 ## LEFT JOIN

```
SELECT *
FROM A
LEFT JOIN B
    ON A.id = B.id;
```

---

 ## RIGHT JOIN

```
SELECT *
FROM A
RIGHT JOIN B
    ON A.id = B.id;
```

---

 ## FULL OUTER JOIN

```
SELECT *
FROM A
FULL OUTER JOIN B
    ON A.id = B.id;
```

---

 ## SELF JOIN

```
SELECT *
FROM employees e
JOIN employees m
    ON e.manager_id = m.employee_id;
```

---

 ## CROSS JOIN

```
SELECT *
FROM A
CROSS JOIN B;
```

---

 # 32\. Visual JOIN Cheat Sheet

 ## INNER JOIN

```
A       B
 \     /
  \___/
   MATCH

Only matching rows
```

---

 ## LEFT JOIN

```
A       B
████████████
    \
     █████

All A rows
+ matching B rows
```

---

 ## RIGHT JOIN

```
A       B
████     ████████████
          ████████████

All B rows
+ matching A rows
```

---

 ## FULL OUTER JOIN

```
A              B
████████████████████████

Everything from both
```

---

 ## SELF JOIN

```
        employees
        /       \
       /         \
 employee       manager
```

---

 ## CROSS JOIN

```
A1 -> B1
A1 -> B2
A1 -> B3

A2 -> B1
A2 -> B2
A2 -> B3
```

---

 # 33\. JOIN Decision Guide

 Before writing a JOIN, ask:

 > Which rows do I need to keep?

 ### Only matching records?

 Use:

```
INNER JOIN
```

 ### Every row from the first/main table?

 Use:

```
LEFT JOIN
```

 ### Every row from the second table?

 Use:

```
RIGHT JOIN
```

 ### Every row from both tables?

 Use:

```
FULL OUTER JOIN
```

 ### Same table has a hierarchical relationship?

 Use:

```
SELF JOIN
```

 ### Every possible combination?

 Use:

```
CROSS JOIN
```

---

 # 34\. One-Minute Interview Explanation

 If an interviewer asks:

 > What are SQL JOINs?

 A good answer is:

 > SQL JOINs are used to combine data from multiple tables based on a related column. INNER JOIN returns only matching rows. LEFT JOIN returns all rows from the left table and matching rows from the right table. RIGHT JOIN returns all rows from the right table and matching rows from the left table. FULL OUTER JOIN returns all rows from both tables. SELF JOIN joins a table with itself, which is useful for hierarchical relationships such as employees and managers. CROSS JOIN produces every possible combination of rows between two tables.

---

 # 35\. Interview Questions

 ## Question 1

 What is a JOIN in SQL?

 ### Answer

 A JOIN combines rows from multiple tables using a related column or condition.

---

 ## Question 2

 What is the difference between INNER JOIN and LEFT JOIN?

 ### Answer

 `INNER JOIN` returns only matching rows.

 `LEFT JOIN` returns all rows from the left table and matching rows from the right table. If there is no match, right-table columns contain `NULL`.

---

 ## Question 3

 What is the difference between LEFT JOIN and RIGHT JOIN?

 ### Answer

 LEFT JOIN preserves all rows from the left table.

 RIGHT JOIN preserves all rows from the right table.

---

 ## Question 4

 What is FULL OUTER JOIN?

 ### Answer

 FULL OUTER JOIN returns all rows from both tables, including unmatched rows.

---

 ## Question 5

 What is a SELF JOIN?

 ### Answer

 A SELF JOIN joins a table with itself. It is commonly used for hierarchical data such as employee-manager relationships.

---

 ## Question 6

 What is a CROSS JOIN?

 ### Answer

 A CROSS JOIN produces the Cartesian product of two tables. Every row from the first table is combined with every row from the second table.

---

 ## Question 7

 How do you find employees without departments?

 ### Answer

```
SELECT
    e.employee_name
FROM employees e
LEFT JOIN departments d
    ON e.department_id = d.department_id
WHERE d.department_id IS NULL;
```

---

 ## Question 8

 How do you find departments without employees?

 ### Answer

```
SELECT
    d.department_name
FROM departments d
LEFT JOIN employees e
    ON d.department_id = e.department_id
WHERE e.employee_id IS NULL;
```

---

 ## Question 9

 Can a JOIN have multiple conditions?

 ### Answer

 Yes.

 Example:

```
SELECT *
FROM employees e
JOIN departments d
    ON e.department_id = d.department_id
    AND e.salary > 50000;
```

---

 ## Question 10

 Can we JOIN more than two tables?

 ### Answer

 Yes.

 Example:

```
SELECT *
FROM employees e
JOIN departments d
    ON e.department_id = d.department_id
JOIN locations l
    ON d.location_id = l.location_id;
```

---

 # 36\. Practice Exercises

 Use the following tables:

```
employees
departments
```

---

 ## Exercise 1

 Display:

```
employee_name
department_name
```

 for employees who belong to a department.

 **Expected JOIN:**

```
INNER JOIN
```

---

 ## Exercise 2

 Display all employees and their department names.

 Employees without departments should also appear.

 **Expected JOIN:**

```
LEFT JOIN
```

---

 ## Exercise 3

 Display all departments and their employees.

 Departments without employees should also appear.

 **Expected JOIN:**

```
LEFT JOIN
```

 with departments as the left table.

---

 ## Exercise 4

 Find employees who don't belong to any department.

 **Hint:**

```
LEFT JOIN
WHERE ... IS NULL
```

---

 ## Exercise 5

 Find departments that don't have any employees.

 **Hint:**

```
LEFT JOIN
WHERE ... IS NULL
```

---

 ## Exercise 6

 Display:

```
employee
manager
```

 using the employees table.

 **Expected JOIN:**

```
SELF JOIN
```

---

 ## Exercise 7

 Find employees whose salary is greater than their manager's salary.

 **Expected JOIN:**

```
SELF JOIN
```

---

 ## Exercise 8

 Create two tables:

```
colors
sizes
```

 Generate every color-size combination.

 **Expected JOIN:**

```
CROSS JOIN
```

---

 ## Exercise 9

 Count employees in each department.

 **Expected concepts:**

```
LEFT JOIN
GROUP BY
COUNT
```

---

 ## Exercise 10

 Find departments having more than one employee.

 **Expected concepts:**

```
LEFT JOIN
GROUP BY
HAVING
COUNT
```

---

 # 37\. Practice Solutions

 ## Solution 1

```
SELECT
    e.employee_name,
    d.department_name
FROM employees e
INNER JOIN departments d
    ON e.department_id = d.department_id;
```

---

 ## Solution 2

```
SELECT
    e.employee_name,
    d.department_name
FROM employees e
LEFT JOIN departments d
    ON e.department_id = d.department_id;
```

---

 ## Solution 3

```
SELECT
    d.department_name,
    e.employee_name
FROM departments d
LEFT JOIN employees e
    ON d.department_id = e.department_id;
```

---

 ## Solution 4

```
SELECT
    e.employee_id,
    e.employee_name
FROM employees e
LEFT JOIN departments d
    ON e.department_id = d.department_id
WHERE d.department_id IS NULL;
```

---

 ## Solution 5

```
SELECT
    d.department_id,
    d.department_name
FROM departments d
LEFT JOIN employees e
    ON d.department_id = e.department_id
WHERE e.employee_id IS NULL;
```

---

 ## Solution 6

```
SELECT
    e.employee_name AS employee,
    m.employee_name AS manager
FROM employees e
LEFT JOIN employees m
    ON e.manager_id = m.employee_id;
```

---

 ## Solution 7

```
SELECT
    e.employee_name AS employee,
    e.salary AS employee_salary,
    m.employee_name AS manager,
    m.salary AS manager_salary
FROM employees e
JOIN employees m
    ON e.manager_id = m.employee_id
WHERE e.salary > m.salary;
```

---

 ## Solution 8

```
SELECT
    c.color_name,
    s.size_name
FROM colors c
CROSS JOIN sizes s;
```

---

 ## Solution 9

```
SELECT
    d.department_name,
    COUNT(e.employee_id) AS employee_count
FROM departments d
LEFT JOIN employees e
    ON d.department_id = e.department_id
GROUP BY
    d.department_name;
```

---

 ## Solution 10

```
SELECT
    d.department_name,
    COUNT(e.employee_id) AS employee_count
FROM departments d
LEFT JOIN employees e
    ON d.department_id = e.department_id
GROUP BY
    d.department_name
HAVING COUNT(e.employee_id) > 1;
```

---

 # 38\. Complete Example Script

 The following script can be executed as a starting point.

```
-- ============================================
-- CREATE TABLES
-- ============================================

CREATE TABLE departments (
    department_id INT PRIMARY KEY,
    department_name VARCHAR(100)
);

CREATE TABLE employees (
    employee_id INT PRIMARY KEY,
    employee_name VARCHAR(100),
    department_id INT,
    manager_id INT,
    salary DECIMAL(10, 2)
);

-- ============================================
-- INSERT DEPARTMENTS
-- ============================================

INSERT INTO departments (
    department_id,
    department_name
)
VALUES
    (10, 'IT'),
    (20, 'HR'),
    (30, 'Finance'),
    (40, 'Marketing');

-- ============================================
-- INSERT EMPLOYEES
-- ============================================

INSERT INTO employees (
    employee_id,
    employee_name,
    department_id,
    manager_id,
    salary
)
VALUES
    (1, 'John', 10, NULL, 60000),
    (2, 'Alice', 20, 1, 75000),
    (3, 'Bob', 10, 1, 55000),
    (4, 'David', 30, 2, 65000),
    (5, 'Emma', NULL, 2, 50000);

-- ============================================
-- INNER JOIN
-- ============================================

SELECT
    e.employee_name,
    d.department_name
FROM employees e
INNER JOIN departments d
    ON e.department_id = d.department_id;

-- ============================================
-- LEFT JOIN
-- ============================================

SELECT
    e.employee_name,
    d.department_name
FROM employees e
LEFT JOIN departments d
    ON e.department_id = d.department_id;

-- ============================================
-- RIGHT JOIN
-- ============================================

SELECT
    e.employee_name,
    d.department_name
FROM employees e
RIGHT JOIN departments d
    ON e.department_id = d.department_id;

-- ============================================
-- FULL OUTER JOIN
-- ============================================

SELECT
    e.employee_name,
    d.department_name
FROM employees e
FULL OUTER JOIN departments d
    ON e.department_id = d.department_id;

-- ============================================
-- SELF JOIN
-- ============================================

SELECT
    e.employee_name AS employee,
    m.employee_name AS manager
FROM employees e
LEFT JOIN employees m
    ON e.manager_id = m.employee_id;

-- ============================================
-- FIND EMPLOYEES WITHOUT DEPARTMENTS
-- ============================================

SELECT
    e.employee_name
FROM employees e
LEFT JOIN departments d
    ON e.department_id = d.department_id
WHERE d.department_id IS NULL;

-- ============================================
-- FIND DEPARTMENTS WITHOUT EMPLOYEES
-- ============================================

SELECT
    d.department_name
FROM departments d
LEFT JOIN employees e
    ON d.department_id = e.department_id
WHERE e.employee_id IS NULL;

-- ============================================
-- EMPLOYEES WITH SALARY > 60000
-- ============================================

SELECT
    e.employee_name,
    d.department_name,
    e.salary
FROM employees e
LEFT JOIN departments d
    ON e.department_id = d.department_id
WHERE e.salary > 60000;

-- ============================================
-- COUNT EMPLOYEES PER DEPARTMENT
-- ============================================

SELECT
    d.department_name,
    COUNT(e.employee_id) AS employee_count
FROM departments d
LEFT JOIN employees e
    ON d.department_id = e.department_id
GROUP BY d.department_name;

-- ============================================
-- EMPLOYEES WHO EARN MORE THAN MANAGERS
-- ============================================

SELECT
    e.employee_name AS employee,
    e.salary AS employee_salary,
    m.employee_name AS manager,
    m.salary AS manager_salary
FROM employees e
JOIN employees m
    ON e.manager_id = m.employee_id
WHERE e.salary > m.salary;
```

---

 # 39\. Final Mental Model

 The easiest way to remember JOINs is:

```
                         SQL JOINs
                             |
          +------------------+------------------+
          |                  |                  |
      MATCHING           PRESERVING          SPECIAL
          |                  |                  |
          |                  |                  |
       INNER          LEFT / RIGHT / FULL     SELF
                                              CROSS
```

---

 ## INNER JOIN

```
Only matches
```

```
A ∩ B
```

---

 ## LEFT JOIN

```
Everything from LEFT
+
Matches from RIGHT
```

```
A + matching B
```

---

 ## RIGHT JOIN

```
Everything from RIGHT
+
Matches from LEFT
```

```
B + matching A
```

---

 ## FULL OUTER JOIN

```
Everything from LEFT
+
Everything from RIGHT
```

```
A ∪ B
```

---

 ## SELF JOIN

```
Table A
   |
   +---- Table A
```

 Same table is used twice.

---

 ## CROSS JOIN

```
Every row in A
×
Every row in B
```

 If:

```
A = 5 rows
B = 4 rows
```

 then:

```
5 × 4 = 20 rows
```

---

 # 40\. Final Quick Reference

 | JOIN | Matching | Unmatched Left | Unmatched Right |
| --- | --- | --- | --- |
| INNER JOIN | Yes | No | No |
| LEFT JOIN | Yes | Yes | No |
| RIGHT JOIN | Yes | No | Yes |
| FULL OUTER JOIN | Yes | Yes | Yes |
| SELF JOIN | Depends | Depends | Depends |
| CROSS JOIN | Not applicable | All combinations | All combinations |

---

 # 41\. The Most Important Rule

 When deciding which JOIN to use, ask:

```
"What records do I want to keep?"
```

 ### Want only matching records?

```
INNER JOIN
```

 ### Want every record from your main table?

```
LEFT JOIN
```

 ### Want every record from the second table?

```
RIGHT JOIN
```

 ### Want every record from both tables?

```
FULL OUTER JOIN
```

 ### Want to compare rows in the same table?

```
SELF JOIN
```

 ### Want every possible combination?

```
CROSS JOIN
```

---

 # 42\. Final Summary

 SQL JOINs are fundamental for working with relational databases.

 The six important JOIN types are:

```
1. INNER JOIN
2. LEFT JOIN
3. RIGHT JOIN
4. FULL OUTER JOIN
5. SELF JOIN
6. CROSS JOIN
```

 The simplest way to remember them:

```
INNER
  ↓
Only matching data

LEFT
  ↓
Everything from LEFT

RIGHT
  ↓
Everything from RIGHT

FULL
  ↓
Everything from BOTH

SELF
  ↓
Same table joined to itself

CROSS
  ↓
Every possible combination
```

 Once you understand:

```
JOIN
ON
WHERE
NULL
GROUP BY
HAVING
```

 you can solve a large number of real-world SQL problems.

---

 # End

 **File name:** `sql-joins.md`

 **Recommended companion file:** `sql-joins-practice.sql`
