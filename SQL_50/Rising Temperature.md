## Problem Description

You have a table named `Weather` that contains information about the temperature on different dates.

### Weather Table

```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| recordDate    | date    |
| temperature   | int     |
+---------------+---------+
```

- `id` is the column with unique values for this table.
- Each row represents the temperature recorded on a specific `recordDate`.

Write a solution to find all dates' `id` where the temperature was higher compared to the previous day.

Return the result table in any order.

### Example

**Input:**

Weather table:
```
+----+------------+-------------+
| id | recordDate | temperature |
+----+------------+-------------+
| 1  | 2015-01-01 | 10          |
| 2  | 2015-01-02 | 25          |
| 3  | 2015-01-03 | 20          |
| 4  | 2015-01-04 | 30          |
+----+------------+-------------+
```

**Output:**
```
+----+
| id |
+----+
| 2  |
| 4  |
+----+
```

**Explanation:**
- On `2015-01-02`, the temperature was higher than the previous day (10 -> 25).
- On `2015-01-04`, the temperature was higher than the previous day (20 -> 30).

## Solution

```sql
SELECT W.id 
FROM Weather AS W
JOIN Weather AS W2
ON W2.temperature < W.temperature AND DATEDIFF(W.recordDate, W2.recordDate)= 1
```

This SQL query retrieves the `id` of each date where the temperature (`W.temperature`) was higher than the temperature of the previous day (`W2.temperature`). It achieves this by joining the `Weather` table (`W`) with itself (`W2`) based on consecutive dates and comparing the temperatures. The result is returned in any order as specified.
