## Problem Description

You are given two tables, `Project` and `Employee`, with the following structures:

### Project Table

```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| project_id  | int     |
| employee_id | int     |
+-------------+---------+
```

- `(project_id, employee_id)` is the primary key.
- Each row indicates that the employee with `employee_id` is working on the project with `project_id`.

### Employee Table

```
+------------------+---------+
| Column Name      | Type    |
+------------------+---------+
| employee_id      | int     |
| name             | varchar |
| experience_years | int     |
+------------------+---------+
```

- `employee_id` is the primary key.
- Each row contains information about one employee.
- `experience_years` is guaranteed to be not NULL.

### Objective

Write a SQL query to report the average experience years of all the employees for each project, rounded to 2 decimal places.

### Example

**Input:**

`Project` table:
```
+-------------+-------------+
| project_id  | employee_id |
+-------------+-------------+
| 1           | 1           |
| 1           | 2           |
| 1           | 3           |
| 2           | 1           |
| 2           | 4           |
+-------------+-------------+
```

`Employee` table:
```
+-------------+--------+------------------+
| employee_id | name   | experience_years |
+-------------+--------+------------------+
| 1           | Khaled | 3                |
| 2           | Ali    | 2                |
| 3           | John   | 1                |
| 4           | Doe    | 2                |
+-------------+--------+------------------+
```

**Output:**
```
+-------------+---------------+
| project_id  | average_years |
+-------------+---------------+
| 1           | 2.00          |
| 2           | 2.50          |
+-------------+---------------+
```

**Explanation:**

The average experience years for the first project is (3 + 2 + 1) / 3 = 2.00, and for the second project is (3 + 2) / 2 = 2.50.

## Solution

```sql
SELECT 
    P.project_id, 
    ROUND(SUM(E.experience_years) / COUNT(P.project_id), 2) AS average_years 
FROM 
    Project AS P
LEFT JOIN 
    Employee AS E
ON 
    P.employee_id = E.employee_id 
GROUP BY 
    P.project_id;
```

### Explanation

1. **SELECT P.project_id, ROUND(SUM(E.experience_years) / COUNT(P.project_id), 2) AS average_years**:
   - Select the `project_id` and calculate the average experience years.
   - Use `ROUND` to round the result to 2 decimal places.
   - Use `SUM(E.experience_years) / COUNT(P.project_id)` to calculate the average.

2. **FROM Project AS P**:
   - Select from the `Project` table with alias `P`.

3. **LEFT JOIN Employee AS E**:
   - Perform a left join with the `Employee` table using alias `E`.
   - Join on `employee_id`.

4. **GROUP BY P.project_id**:
   - Group the results by `project_id` to calculate the average experience years for each project.

This query will return the average experience years for each project, rounded to two decimal places.