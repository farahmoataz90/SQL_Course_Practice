
## Problem Description

You have two tables, `Employee` and `Bonus`, containing information about employees in a company and their respective bonuses.

### Employee Table

```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| empId       | int     |
| name        | varchar |
| supervisor  | int     |
| salary      | int     |
+-------------+---------+
```

- `empId` is the primary key for this table.
- `name` is the name of the employee.
- `supervisor` is the ID of the supervisor for the employee.
- `salary` is the salary of the employee.

### Bonus Table

```
+-------------+------+
| Column Name | Type |
+-------------+------+
| empId       | int  |
| bonus       | int  |
+-------------+------+
```

- `empId` is the primary key for this table and a foreign key referencing the `empId` in the `Employee` table.
- `bonus` is the bonus amount for the employee.

### Objective

Write a SQL query to report the name and bonus amount of each employee who has a bonus less than 1000. 

### Example

**Input:**

Employee table:
```
+-------+--------+------------+--------+
| empId | name   | supervisor | salary |
+-------+--------+------------+--------+
| 3     | Brad   | null       | 4000   |
| 1     | John   | 3          | 1000   |
| 2     | Dan    | 3          | 2000   |
| 4     | Thomas | 3          | 4000   |
+-------+--------+------------+--------+
```

Bonus table:
```
+-------+-------+
| empId | bonus |
+-------+-------+
| 2     | 500   |
| 4     | 2000  |
+-------+-------+
```

**Output:**
```
+------+-------+
| name | bonus |
+------+-------+
| Brad | null  |
| John | null  |
| Dan  | 500   |
+------+-------+
```

**Explanation:**
- Brad does not have a bonus listed, so it is considered as null.
- John does not have a bonus listed, so it is considered as null.
- Dan has a bonus of 500, which is less than 1000.
- Thomas has a bonus of 2000, which is not included in the result as it is not less than 1000.

## Solution

```sql
SELECT E.name, B.bonus
FROM Employee AS E
LEFT JOIN Bonus AS B
ON E.empId = B.empId
WHERE IFNULL(B.bonus, 0) < 1000;
```

### Explanation

1. **Join the Tables**: We perform a LEFT JOIN between the `Employee` and `Bonus` tables to ensure all employees are included in the result, even if they do not have a corresponding entry in the `Bonus` table.
2. **Conditional Filtering**: Use `IFNULL(B.bonus, 0) < 1000` to include employees with a bonus less than 1000. The `IFNULL` function ensures that employees without a bonus entry are treated as having a bonus of 0.
3. **Select Columns**: The query selects the employee name and bonus amount to be included in the result.

This query ensures that you get a list of all employees with their bonuses, including those without a bonus (considered as null), filtered to only include bonuses less than 1000.
