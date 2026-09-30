
 - `=`
- `<>` / `!=`
- `AND`
- `OR`
- `BETWEEN`
- `IN`
- `NOT IN`
- `LIKE`
- `IS NULL`
- `IS NOT NULL`

 SQL WHERE Clause — Complete Practice Notes

# SQL WHERE Clause — Complete Practice Notes

 These examples use the `customers` table.

 ## Table Structure

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

 # 1\. Equal To `=`

 Used to find rows where a value exactly matches.

 ### Query

```
SELECT *
FROM customers
WHERE age = 28;
```

 ### Meaning

 Find customers whose age is **28**.

 ### Output

 | customer\_id | name | age | city |
| --- | --- | --- | --- |
| 1 | Rahul Sharma | 28 | Hyderabad |

---

 # 2\. Not Equal `<>`

 Used to find rows where a value is **not equal** to something.

 ### Query

```
SELECT *
FROM customers
WHERE age <> 25;
```

 ### Meaning

 Find customers whose age is **not 25**.

 `<>` means **not equal to**.

---

 # 3\. Not Equal `!=`

 `!=` also means **not equal**.

 ### Query

```
SELECT *
FROM customers
WHERE age != 29;
```

 ### Meaning

 Find customers whose age is **not 29**.

 Both are valid:

```
WHERE age <> 29;
```

```
WHERE age != 29;
```

 ### Easy way to remember

```
<>  → Not equal
!=  → Not equal
```

---

 # 4\. `AND`

 `AND` means **all conditions must be true**.

 ### Query

```
SELECT *
FROM customers
WHERE age <> 25
  AND age != 29;
```

 ### Meaning

 The customer must:

 - Not be 25 years old
- AND not be 29 years old

 Both conditions must be true.

---

 # 5\. `OR`

 `OR` means **at least one condition must be true**.

 ### Query

```
SELECT *
FROM customers
WHERE age = 28
   OR age = 25;
```

 ### Meaning

 Find customers whose age is:

 **28 OR 25**

 ### Output

 | customer\_id | name | age | city |
| --- | --- | --- | --- |
| 1 | Rahul Sharma | 28 | Hyderabad |

There is no customer with age 25 in our current data.

 ### Easy way to remember

```
AND → ALL conditions
OR  → ANY condition
```

---

 # 6\. `BETWEEN`

 `BETWEEN` checks whether a value is inside a range.

 ### Query

```
SELECT *
FROM customers
WHERE age BETWEEN 20 AND 30;
```

 ### Meaning

 Find customers whose age is from **20 through 30**.

 `BETWEEN` includes both values:

```
20 ≤ age ≤ 30
```

 ### Output

 | customer\_id | name | age | city |
| --- | --- | --- | --- |
| 1 | Rahul Sharma | 28 | Hyderabad |
| 6 | Ananya Rao | 24 | Hyderabad |
| 8 | Meera Iyer | 27 | Chennai |
| 10 | Neha Gupta | 30 | Delhi |
| 11 | Aditya Joshi | 26 | Mumbai |
| 14 | Pooja Shah | 23 | Ahmedabad |

---

 # 7\. `IN`

 `IN` is used when you want to match **multiple possible values**.

 Instead of writing:

```
SELECT *
FROM customers
WHERE city = 'Hyderabad'
   OR city = 'Chennai'
   OR city = 'Delhi';
```

 You can use:

```
SELECT *
FROM customers
WHERE city IN ('Hyderabad', 'Chennai', 'Delhi');
```

 ### Meaning

 Find customers whose city is:

 - Hyderabad
- Chennai
- OR Delhi

 ### Output

 | customer\_id | name | age | city |
| --- | --- | --- | --- |
| 1 | Rahul Sharma | 28 | Hyderabad |
| 3 | Arjun Kumar | 32 | Chennai |
| 5 | Vikram Singh | 35 | Delhi |
| 6 | Ananya Rao | 24 | Hyderabad |
| 8 | Meera Iyer | 27 | Chennai |
| 10 | Neha Gupta | 30 | Delhi |
| 13 | Suresh Babu | 41 | Hyderabad |

### Easy way to remember

```
IN → I want values from this list
```

 ### Another example

```
SELECT *
FROM customers
WHERE age IN (23, 28, 35);
```

 This means:

```
age = 23
OR age = 28
OR age = 35
```

---

 # 8\. `NOT IN`

 `NOT IN` is used to **exclude multiple values**.

 ### Query

```
SELECT *
FROM customers
WHERE city NOT IN ('Hyderabad', 'Chennai', 'Delhi');
```

 ### Meaning

 Find customers whose city is **not**:

 - Hyderabad
- Chennai
- Delhi

 ### Output

 | customer\_id | name | age | city |
| --- | --- | --- | --- |
| 7 | Karthik Nair | 31 | Kochi |
| 9 | Rohit Verma | 38 | Pune |
| 11 | Aditya Joshi | 26 | Mumbai |
| 12 | Divya Menon | 33 | Kochi |
| 14 | Pooja Shah | 23 | Ahmedabad |
| 15 | Manish Agarwal | 36 | Pune |

### Easy way to remember

```
NOT IN → I don't want values from this list
```

---

 # 9\. `IN` vs `OR`

 These two can often produce the same result.

 ### Using `OR`

```
SELECT *
FROM customers
WHERE city = 'Hyderabad'
   OR city = 'Chennai'
   OR city = 'Delhi';
```

 ### Using `IN`

```
SELECT *
FROM customers
WHERE city IN ('Hyderabad', 'Chennai', 'Delhi');
```

 The second version is shorter and easier to read when checking multiple values.

---

 # 10\. `LIKE`

 `LIKE` is used for **pattern matching**, mainly with text.

 ## 10.1 Starts With

```
SELECT *
FROM customers
WHERE name LIKE 'Rahul%';
```

 ### Meaning

 Name starts with `Rahul`.

```
Rahul Sharma
^^^^^
starts with Rahul
```

 ### Output

 | customer\_id | name | age | city |
| --- | --- | --- | --- |
| 1 | Rahul Sharma | 28 | Hyderabad |

---

 ## 10.2 Ends With

```
SELECT *
FROM customers
WHERE name LIKE '%Sharma';
```

 ### Meaning

 Name ends with `Sharma`.

```
Rahul Sharma
      ^^^^^^
      ends with Sharma
```

---

 ## 10.3 Contains

```
SELECT *
FROM customers
WHERE name LIKE '%a%';
```

 ### Meaning

 Name contains the letter `a` anywhere.

```
%a%
 ↑
a can appear anywhere
```

---

 ## 10.4 Single Character `_`

 `_` represents **exactly one character**.

```
SELECT *
FROM customers
WHERE name LIKE 'R_hul%';
```

 Here:

```
R_hul
 ^
one character
```

 The `_` can represent the `a` in `Rahul`.

---

 # 11\. `IS NULL`

 `NULL` means the value is **missing or unknown**.

 To find missing values, use `IS NULL`.

 ### Query

```
SELECT *
FROM customers
WHERE phone IS NULL;
```

 ### Meaning

 Find customers who **do not have a phone number**.

 ### Current Data

 Every customer in our current data has a phone number.

 Therefore:

```
No rows returned
```

 ### Important

 Do NOT use:

```
WHERE phone = NULL;
```

 Use:

```
WHERE phone IS NULL;
```

---

 # 12\. `IS NOT NULL`

 Used to find rows where a value **exists**.

 ### Query

```
SELECT *
FROM customers
WHERE phone IS NOT NULL;
```

 ### Meaning

 Find customers who have a phone number.

 In our current data, all 13 customers have a phone number.

---

 # 13\. Combining `IN` \+ `AND`

 You can combine `IN` with other conditions.

 ### Query

```
SELECT *
FROM customers
WHERE city IN ('Hyderabad', 'Chennai')
  AND age > 25;
```

 ### Meaning

 Find customers who:

 1. Live in Hyderabad OR Chennai
2. AND are older than 25

 ### Output

 | customer\_id | name | age | city |
| --- | --- | --- | --- |
| 1 | Rahul Sharma | 28 | Hyderabad |
| 3 | Arjun Kumar | 32 | Chennai |
| 8 | Meera Iyer | 27 | Chennai |
| 13 | Suresh Babu | 41 | Hyderabad |

---

 # 14\. Combining `NOT IN` \+ `AND`

 ### Query

```
SELECT *
FROM customers
WHERE city NOT IN ('Hyderabad', 'Delhi')
  AND age > 30;
```

 ### Meaning

 Find customers who:

 - Are NOT from Hyderabad
- Are NOT from Delhi
- AND are older than 30

 ### Output

 | customer\_id | name | age | city |
| --- | --- | --- | --- |
| 3 | Arjun Kumar | 32 | Chennai |
| 7 | Karthik Nair | 31 | Kochi |
| 9 | Rohit Verma | 38 | Pune |
| 12 | Divya Menon | 33 | Kochi |
| 15 | Manish Agarwal | 36 | Pune |

---

 # 15\. Combining `BETWEEN` \+ `AND`

 ### Query

```
SELECT *
FROM customers
WHERE age BETWEEN 20 AND 35
  AND city = 'Hyderabad';
```

 ### Meaning

 Find customers who:

 - Are between 20 and 35 years old
- AND live in Hyderabad

 ### Output

 | customer\_id | name | age | city |
| --- | --- | --- | --- |
| 1 | Rahul Sharma | 28 | Hyderabad |
| 6 | Ananya Rao | 24 | Hyderabad |

---

 # 16\. Combining `LIKE` \+ `AND`

 ### Query

```
SELECT *
FROM customers
WHERE name LIKE 'A%'
  AND age > 30;
```

 ### Meaning

 Find customers who:

 - Have a name starting with `A`
- AND are older than 30

 ### Output

 | customer\_id | name | age | city |
| --- | --- | --- | --- |
| 3 | Arjun Kumar | 32 | Chennai |

---

 # 17\. Combining `IS NOT NULL` \+ `AND`

 ### Query

```
SELECT *
FROM customers
WHERE phone IS NOT NULL
  AND age > 30;
```

 ### Meaning

 Find customers who:

 - Have a phone number
- AND are older than 30

---

 # 18\. Full Example

 You can combine several operators in one query.

 ### Query

```
SELECT *
FROM customers
WHERE city IN ('Hyderabad', 'Chennai')
  AND age BETWEEN 25 AND 40
  AND name LIKE '%a%';
```

 ### Meaning

 All of these conditions must be satisfied:

```
City
 ↓
Hyderabad OR Chennai

AND

Age
 ↓
25 through 40

AND

Name
 ↓
contains the letter "a"
```

 This is how real-world SQL queries are commonly built: **multiple filtering conditions combined together**.

---

 # 19\. Quick Cheat Sheet

 | Operator | Meaning | Example |
| --- | --- | --- |
| `=` | Equal to | `age = 28` |
| `<>` | Not equal to | `age <> 25` |
| `!=` | Not equal to | `age != 25` |
| `AND` | All conditions must be true | `age > 20 AND age < 30` |
| `OR` | At least one condition is true | `age = 20 OR age = 30` |
| `BETWEEN` | Value within a range | `age BETWEEN 20 AND 30` |
| `IN` | Match values from a list | `city IN ('Delhi', 'Pune')` |
| `NOT IN` | Exclude values from a list | `city NOT IN ('Delhi', 'Pune')` |
| `LIKE` | Pattern matching | `name LIKE 'Rahul%'` |
| `%` | Zero or more characters | `name LIKE '%a%'` |
| `_` | Exactly one character | `name LIKE 'R_hul'` |
| `IS NULL` | Value is missing | `phone IS NULL` |
| `IS NOT NULL` | Value exists | `phone IS NOT NULL` |

---

 # 20\. Easy Memory Trick

```
=             → Equal

<> / !=       → Not equal

AND           → ALL conditions

OR            → ANY condition

BETWEEN       → Range

IN            → From this list

NOT IN        → NOT from this list

LIKE          → Pattern matching

%             → Any number of characters

_             → Exactly one character

IS NULL       → Value is missing

IS NOT NULL   → Value exists
```

---

 # 21\. Practice Questions

 Try these yourself before looking at the answer.

 ### Question 1

 Find customers from Hyderabad.

```
SELECT *
FROM customers
WHERE city = 'Hyderabad';
```

 ### Question 2

 Find customers from Hyderabad or Pune.

```
SELECT *
FROM customers
WHERE city IN ('Hyderabad', 'Pune');
```

 ### Question 3

 Find customers who are not from Hyderabad or Pune.

```
SELECT *
FROM customers
WHERE city NOT IN ('Hyderabad', 'Pune');
```

 ### Question 4

 Find customers between 25 and 35 years old.

```
SELECT *
FROM customers
WHERE age BETWEEN 25 AND 35;
```

 ### Question 5

 Find customers whose name starts with `A`.

```
SELECT *
FROM customers
WHERE name LIKE 'A%';
```

 ### Question 6

 Find customers whose name contains `a`.

```
SELECT *
FROM customers
WHERE name LIKE '%a%';
```

 ### Question 7

 Find customers with no phone number.

```
SELECT *
FROM customers
WHERE phone IS NULL;
```

 ### Question 8

 Find customers who have a phone number.

```
SELECT *
FROM customers
WHERE phone IS NOT NULL;
```

---

 # Final Summary

 The `WHERE` clause is used to **filter rows**.

```
SELECT *
FROM customers
WHERE condition;
```

 The main operators covered are:

```
WHERE
│
├── =
├── <> / !=
├── AND
├── OR
├── BETWEEN
├── IN
├── NOT IN
├── LIKE
│   ├── %
│   └── _
├── IS NULL
└── IS NOT NULL
```

 Once you understand these operators, you can build increasingly complex filters by combining them with `AND` and `OR`.
