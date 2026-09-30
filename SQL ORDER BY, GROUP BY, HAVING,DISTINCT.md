 SQL ORDER BY, GROUP BY, HAVING, DISTINCT

# SQL ORDER BY, GROUP BY, HAVING, DISTINCT

 These examples use the `customers` table.

---

 # Table Structure

 | Column | Description |
| --- | --- |
| `customer_id` | Unique customer ID |
| `name` | Customer name |
| `email` | Customer email |
| `phone` | Customer phone number |
| `age` | Customer age |
| `city` | Customer city |
| `created_at` | Record creation date/time |

---

 # 1\. ORDER BY

 `ORDER BY` is used to **sort the result**.

 By default, sorting is ascending (`ASC`).

 ## 1.1 Sort by Age — Ascending

 ### Query

```
SELECT *
FROM customers
ORDER BY age ASC;
```

 ### Meaning

 Sort customers from **youngest to oldest**.

 ### Output

 | customer\_id | name | age | city |
| --- | --- | --- | --- |
| 14 | Pooja Shah | 23 | Ahmedabad |
| 6 | Ananya Rao | 24 | Hyderabad |
| 11 | Aditya Joshi | 26 | Mumbai |
| 8 | Meera Iyer | 27 | Chennai |
| 1 | Rahul Sharma | 28 | Hyderabad |
| 10 | Neha Gupta | 30 | Delhi |
| 7 | Karthik Nair | 31 | Kochi |
| 3 | Arjun Kumar | 32 | Chennai |
| 12 | Divya Menon | 33 | Kochi |
| 5 | Vikram Singh | 35 | Delhi |
| 15 | Manish Agarwal | 36 | Pune |
| 9 | Rohit Verma | 38 | Pune |
| 13 | Suresh Babu | 41 | Hyderabad |

### Remember

```
ASC → Small to large
```

 For text:

```
A → Z
```

---

 ## 1.2 Sort by Age — Descending

 ### Query

```
SELECT *
FROM customers
ORDER BY age DESC;
```

 ### Meaning

 Sort customers from **oldest to youngest**.

 ### Output

 | customer\_id | name | age | city |
| --- | --- | --- | --- |
| 13 | Suresh Babu | 41 | Hyderabad |
| 9 | Rohit Verma | 38 | Pune |
| 15 | Manish Agarwal | 36 | Pune |
| 5 | Vikram Singh | 35 | Delhi |
| 12 | Divya Menon | 33 | Kochi |
| 3 | Arjun Kumar | 32 | Chennai |
| 7 | Karthik Nair | 31 | Kochi |
| 10 | Neha Gupta | 30 | Delhi |
| 1 | Rahul Sharma | 28 | Hyderabad |
| 8 | Meera Iyer | 27 | Chennai |
| 11 | Aditya Joshi | 26 | Mumbai |
| 6 | Ananya Rao | 24 | Hyderabad |
| 14 | Pooja Shah | 23 | Ahmedabad |

### Remember

```
DESC → Large to small
```

 For text:

```
Z → A
```

---

 ## 1.3 ORDER BY Without ASC

 This:

```
SELECT *
FROM customers
ORDER BY age;
```

 is the same as:

```
SELECT *
FROM customers
ORDER BY age ASC;
```

 `ASC` is the default.

---

 ## 1.4 ORDER BY Multiple Columns

 You can sort by more than one column.

 ### Query

```
SELECT *
FROM customers
ORDER BY city ASC, age DESC;
```

 ### Meaning

 First:

```
Sort cities A → Z
```

 Then, within each city:

```
Sort age highest → lowest
```

 For example, Hyderabad customers:

 | customer\_id | name | age | city |
| --- | --- | --- | --- |
| 13 | Suresh Babu | 41 | Hyderabad |
| 1 | Rahul Sharma | 28 | Hyderabad |
| 6 | Ananya Rao | 24 | Hyderabad |

---

 # 2\. DISTINCT

 `DISTINCT` is used to **remove duplicate results**.

 ## 2.1 DISTINCT City

 ### Query

```
SELECT DISTINCT city
FROM customers;
```

 ### Meaning

 Show each city only once.

 ### Output

 | city |
| --- |
| Hyderabad |
| Chennai |
| Delhi |
| Kochi |
| Pune |
| Mumbai |
| Ahmedabad |

Even though some cities have multiple customers, each city appears only once.

---

 ## 2.2 DISTINCT Age

 ### Query

```
SELECT DISTINCT age
FROM customers
ORDER BY age;
```

 ### Meaning

 Show each age only once and sort them from low to high.

 ### Output

 | age |
| --- |
| 23 |
| 24 |
| 26 |
| 27 |
| 28 |
| 30 |
| 31 |
| 32 |
| 33 |
| 35 |
| 36 |
| 38 |
| 41 |

In our current data, every age happens to be unique.

---

 ## 2.3 DISTINCT on Multiple Columns

 You can use `DISTINCT` with multiple columns.

 ### Query

```
SELECT DISTINCT city, age
FROM customers;
```

 This removes duplicate **`city + age` combinations**.

 For example, if the data contained:

 | city | age |
| --- | --- |
| Hyderabad | 28 |
| Hyderabad | 28 |
| Hyderabad | 24 |
| Chennai | 28 |

Then:

```
SELECT DISTINCT city, age
FROM customers;
```

 would return:

 | city | age |
| --- | --- |
| Hyderabad | 28 |
| Hyderabad | 24 |
| Chennai | 28 |

### Important

```
DISTINCT city
```

 means:

 > Unique cities.

 But:

```
DISTINCT city, age
```

 means:

 > Unique combinations of city and age.

 It is similar to the **idea of a composite key**, but `DISTINCT` does **not** create a key or a database constraint.

---

 # 3\. GROUP BY

 `GROUP BY` is used to **group rows that have the same value**.

 It is commonly used with aggregate functions such as:

```
COUNT()
SUM()
AVG()
MIN()
MAX()
```

---

 ## 3.1 GROUP BY City

 ### Query

```
SELECT city
FROM customers
GROUP BY city;
```

 ### Meaning

 Create one group for each city.

 The result is similar to:

```
SELECT DISTINCT city
FROM customers;
```

 But `GROUP BY` becomes much more useful when combined with aggregate functions.

---

 ## 3.2 COUNT Customers in Each City

 ### Query

```
SELECT city, COUNT(*) AS customer_count
FROM customers
GROUP BY city;
```

 ### Meaning

 Count how many customers are in each city.

 ### Output

 | city | customer\_count |
| --- | --- |
| Ahmedabad | 1 |
| Chennai | 2 |
| Delhi | 2 |
| Hyderabad | 3 |
| Kochi | 2 |
| Mumbai | 1 |
| Pune | 2 |

### Think of it as

```
GROUP BY city
       ↓
Put same cities together
       ↓
COUNT(*)
       ↓
Count customers in each group
```

---

 # 4\. GROUP BY + AVG

 Calculate the average age for each city.

 ### Query

```
SELECT city, AVG(age) AS average_age
FROM customers
GROUP BY city;
```

 ### Output

 | city | average\_age |
| --- | --- |
| Ahmedabad | 23.00 |
| Chennai | 29.50 |
| Delhi | 32.50 |
| Hyderabad | 31.00 |
| Kochi | 32.00 |
| Mumbai | 26.00 |
| Pune | 37.00 |

### Example

 Hyderabad has:

```
Rahul Sharma → 28
Ananya Rao   → 24
Suresh Babu  → 41
```

 Average:

```
(28 + 24 + 41) / 3 = 31
```

---

 # 5\. GROUP BY + MIN

 Find the **youngest age** in each city.

 ### Query

```
SELECT city, MIN(age) AS youngest_age
FROM customers
GROUP BY city;
```

 ### Output

 | city | youngest\_age |
| --- | --- |
| Ahmedabad | 23 |
| Chennai | 27 |
| Delhi | 30 |
| Hyderabad | 24 |
| Kochi | 31 |
| Mumbai | 26 |
| Pune | 36 |

---

 # 6\. GROUP BY + MAX

 Find the **oldest age** in each city.

 ### Query

```
SELECT city, MAX(age) AS oldest_age
FROM customers
GROUP BY city;
```

 ### Output

 | city | oldest\_age |
| --- | --- |
| Ahmedabad | 23 |
| Chennai | 32 |
| Delhi | 35 |
| Hyderabad | 41 |
| Kochi | 33 |
| Mumbai | 26 |
| Pune | 38 |

---

 # 7\. HAVING

 `HAVING` is used to **filter groups after `GROUP BY`**.

 This is one of the most important differences:

```
WHERE  → filters individual rows

HAVING → filters groups
```

---

 ## 7.1 HAVING with COUNT

 Find cities that have **more than 1 customer**.

 ### Query

```
SELECT city, COUNT(*) AS customer_count
FROM customers
GROUP BY city
HAVING COUNT(*) > 1;
```

 ### Meaning

 First:

```
GROUP BY city
```

 Then count customers in each city.

 Finally:

```
HAVING COUNT(*) > 1
```

 Keep only cities having more than one customer.

 ### Output

 | city | customer\_count |
| --- | --- |
| Chennai | 2 |
| Delhi | 2 |
| Hyderabad | 3 |
| Kochi | 2 |
| Pune | 2 |

Cities with only one customer are removed:

```
Ahmedabad → 1
Mumbai    → 1
```

---

 # 8\. HAVING with AVG

 Find cities where the **average age is greater than 30**.

 ### Query

```
SELECT city, AVG(age) AS average_age
FROM customers
GROUP BY city
HAVING AVG(age) > 30;
```

 ### Output

 | city | average\_age |
| --- | --- |
| Delhi | 32.50 |
| Hyderabad | 31.00 |
| Kochi | 32.00 |
| Pune | 37.00 |

---

 # 9\. WHERE vs HAVING

 This is extremely important.

 ## WHERE

 `WHERE` filters **rows before grouping**.

 ### Example

```
SELECT city, COUNT(*) AS customer_count
FROM customers
WHERE age > 30
GROUP BY city;
```

 ### Meaning

```
1. Take customers older than 30
2. Group them by city
3. Count them
```

---

 ## HAVING

 `HAVING` filters **groups after grouping**.

 ### Example

```
SELECT city, COUNT(*) AS customer_count
FROM customers
GROUP BY city
HAVING COUNT(*) > 1;
```

 ### Meaning

```
1. Group customers by city
2. Count customers in each city
3. Keep groups having more than 1 customer
```

 ### Easy Memory Trick

```
WHERE
 ↓
Filter rows

GROUP BY
 ↓
Create groups

HAVING
 ↓
Filter groups
```

---

 # 10\. WHERE + GROUP BY + HAVING

 You can use all three together.

 ### Query

```
SELECT city, COUNT(*) AS customer_count
FROM customers
WHERE age > 25
GROUP BY city
HAVING COUNT(*) > 1;
```

 ### Meaning

 ### Step 1 — WHERE

 Only customers older than 25.

 ### Step 2 — GROUP BY

 Group those customers by city.

 ### Step 3 — COUNT

 Count customers in each city.

 ### Step 4 — HAVING

 Keep only cities with more than one matching customer.

---

 # 11\. ORDER BY with GROUP BY

 You can sort grouped results.

 ### Query

```
SELECT city, COUNT(*) AS customer_count
FROM customers
GROUP BY city
ORDER BY customer_count DESC;
```

 ### Meaning

 Count customers in each city and show the cities from **highest customer count to lowest**.

 ### Output

 | city | customer\_count |
| --- | --- |
| Hyderabad | 3 |
| Chennai | 2 |
| Delhi | 2 |
| Kochi | 2 |
| Pune | 2 |
| Ahmedabad | 1 |
| Mumbai | 1 |

---

 # 12\. GROUP BY + HAVING + ORDER BY

 A common real-world query:

```
SELECT city, COUNT(*) AS customer_count
FROM customers
GROUP BY city
HAVING COUNT(*) > 1
ORDER BY customer_count DESC;
```

 ### Meaning

```
GROUP BY
↓
Group customers by city

HAVING
↓
Keep cities with more than 1 customer

ORDER BY
↓
Sort highest customer count first
```

 ### Output

 | city | customer\_count |
| --- | --- |
| Hyderabad | 3 |
| Chennai | 2 |
| Delhi | 2 |
| Kochi | 2 |
| Pune | 2 |

---

 # 13\. Common Aggregate Functions

 These are commonly used with `GROUP BY`.

 | Function | Meaning | Example |
| --- | --- | --- |
| `COUNT()` | Counts rows/values | `COUNT(*)` |
| `SUM()` | Adds values | `SUM(age)` |
| `AVG()` | Calculates average | `AVG(age)` |
| `MIN()` | Finds minimum | `MIN(age)` |
| `MAX()` | Finds maximum | `MAX(age)` |

---

 # 14\. Complete SQL Query Order

 For writing a SQL statement, remember this order:

```
SELECT
FROM
WHERE
GROUP BY
HAVING
ORDER BY
```

 ### Example

```
SELECT city, COUNT(*) AS customer_count
FROM customers
WHERE age > 25
GROUP BY city
HAVING COUNT(*) > 1
ORDER BY customer_count DESC;
```

 ### Logical way to understand it

 A useful simplified mental model is:

```
FROM
 ↓
WHERE
 ↓
GROUP BY
 ↓
HAVING
 ↓
SELECT
 ↓
ORDER BY
```

 ### Important

 There are two things to distinguish:

 **SQL writing order:**

```
SELECT
FROM
WHERE
GROUP BY
HAVING
ORDER BY
```

 **Simplified logical processing order:**

```
FROM
 ↓
WHERE
 ↓
GROUP BY
 ↓
HAVING
 ↓
SELECT
 ↓
ORDER BY
```

---

 # 15\. DISTINCT vs GROUP BY

 They can sometimes produce similar results, but their purposes are different.

 ## DISTINCT

 Used to remove duplicate results.

```
SELECT DISTINCT city
FROM customers;
```

 Think:

 > "Give me unique cities."

---

 ## GROUP BY

 Used to create groups, usually for calculations.

```
SELECT city, COUNT(*)
FROM customers
GROUP BY city;
```

 Think:

 > "Group customers by city and calculate something."

---

 # 16\. Quick Cheat Sheet

 | Keyword | Purpose | Example |
| --- | --- | --- |
| `ORDER BY` | Sort results | `ORDER BY age DESC` |
| `DISTINCT` | Remove duplicate results | `SELECT DISTINCT city` |
| `GROUP BY` | Create groups | `GROUP BY city` |
| `HAVING` | Filter groups | `HAVING COUNT(*) > 1` |
| `WHERE` | Filter rows | `WHERE age > 30` |

---

 # 17\. Easy Memory Trick

```
DISTINCT
↓
Remove duplicate results

WHERE
↓
Filter rows

GROUP BY
↓
Create groups

HAVING
↓
Filter groups

ORDER BY
↓
Sort results
```

---

 # 18\. Practice Questions

 Try solving these yourself.

 ## Practice 1

 Show customers from youngest to oldest.

```
SELECT *
FROM customers
ORDER BY age ASC;
```

---

 ## Practice 2

 Show customers from oldest to youngest.

```
SELECT *
FROM customers
ORDER BY age DESC;
```

---

 ## Practice 3

 Show all unique cities.

```
SELECT DISTINCT city
FROM customers;
```

---

 ## Practice 4

 Count customers in each city.

```
SELECT city, COUNT(*) AS customer_count
FROM customers
GROUP BY city;
```

---

 ## Practice 5

 Find the average age in each city.

```
SELECT city, AVG(age) AS average_age
FROM customers
GROUP BY city;
```

---

 ## Practice 6

 Find cities with more than one customer.

```
SELECT city, COUNT(*) AS customer_count
FROM customers
GROUP BY city
HAVING COUNT(*) > 1;
```

---

 ## Practice 7

 Find cities where the average age is greater than 30.

```
SELECT city, AVG(age) AS average_age
FROM customers
GROUP BY city
HAVING AVG(age) > 30;
```

---

 ## Practice 8

 Count customers per city and sort by highest count.

```
SELECT city, COUNT(*) AS customer_count
FROM customers
GROUP BY city
ORDER BY customer_count DESC;
```

---

 # 19\. Final Summary

 ## ORDER BY

```
Sort the result
```

 Example:

```
ORDER BY age DESC;
```

---

 ## DISTINCT

```
Remove duplicate results
```

 Example:

```
SELECT DISTINCT city
FROM customers;
```

---

 ## GROUP BY

```
Group rows with the same value
```

 Example:

```
GROUP BY city;
```

 Usually used with:

```
COUNT()
SUM()
AVG()
MIN()
MAX()
```

---

 ## HAVING

```
Filter groups
```

 Example:

```
HAVING COUNT(*) > 1;
```

---

 # Most Important Difference

```
WHERE
  ↓
Filters ROWS

GROUP BY
  ↓
Creates GROUPS

HAVING
  ↓
Filters GROUPS

ORDER BY
  ↓
Sorts RESULT

DISTINCT
  ↓
Removes DUPLICATE RESULTS
```

---

 # Common SQL Pattern

 A very common pattern is:

```
SELECT city, COUNT(*) AS customer_count
FROM customers
WHERE age > 25
GROUP BY city
HAVING COUNT(*) > 1
ORDER BY customer_count DESC;
```

 Break it down:

```
FROM
↓
Which table?

WHERE
↓
Which rows?

GROUP BY
↓
How should rows be grouped?

HAVING
↓
Which groups should remain?

SELECT
↓
What should we display?

ORDER BY
↓
How should the result be sorted?
```
