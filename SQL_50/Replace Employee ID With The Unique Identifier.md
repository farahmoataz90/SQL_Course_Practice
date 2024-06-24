## Problem Description

Given two tables, `Employees` and `EmployeeUNI`:

### Employees Table

```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| name          | varchar |
+---------------+---------+
```

- `id` is the primary key (column with unique values) for this table.
- Each row of this table contains the `id` and the `name` of an employee in a company.

### EmployeeUNI Table

```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| unique_id     | int     |
+---------------+---------+
```

- `(id, unique_id)` is the primary key (combination of columns with unique values) for this table.
- Each row of this table contains the `id` and the corresponding `unique_id` of an employee in the company.

Write a solution to show the `unique_id` of each user. If a user does not have a `unique_id`, show `null` instead.

Return the result table in any order.

### Example

**Input:**

`Employees` table:
```
+----+----------+
| id | name     |
+----+----------+
| 1  | Alice    |
| 7  | Bob      |
| 11 | Meir     |
| 90 | Winston  |
| 3  | Jonathan |
+----+----------+
```

`EmployeeUNI` table:
```
+----+-----------+
| id | unique_id |
+----+-----------+
| 3  | 1         |
| 11 | 2         |
| 90 | 3         |
+----+-----------+
```

**Output:**
```
+-----------+----------+
| unique_id | name     |
+-----------+----------+
| null      | Alice    |
| null      | Bob      |
| 2         | Meir     |
| 3         | Winston  |
| 1         | Jonathan |
+-----------+----------+
```

**Explanation:** 
- Alice and Bob do not have a `unique_id`, so we show `null` instead.
- The `unique_id` of Meir is 2.
- The `unique_id` of Winston is 3.
- The `unique_id` of Jonathan is 1.

## Solution

```sql
SELECT 
    Euni.unique_id, 
    E.name 
FROM  
    Employees AS E
LEFT JOIN 
    EmployeeUNI AS Euni
ON 
    E.id = Euni.id;
```

This query selects the `unique_id` from the `EmployeeUNI` table and the `name` from the `Employees` table using a left join on the `id` column, ensuring that all employees are listed even if they do not have a corresponding `unique_id`.