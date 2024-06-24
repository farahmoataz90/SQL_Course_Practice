

## Problem Description

You have a table named `Activity` that records user activities for a factory website involving machines and processes.

### Activity Table

```
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| machine_id     | int     |
| process_id     | int     |
| activity_type  | enum    |
| timestamp      | float   |
+----------------+---------+
```

- `(machine_id, process_id, activity_type)` is the primary key (combination of columns with unique values) of this table.
- `machine_id` is the ID of a machine.
- `process_id` is the ID of a process running on the machine with ID `machine_id`.
- `activity_type` is an ENUM ('start', 'end') indicating whether the activity is starting or ending.
- `timestamp` is a float representing the current time in seconds.

Each 'start' activity is followed by an 'end' activity for the same `(machine_id, process_id)` pair, and the 'start' timestamp is always before the 'end' timestamp.

### Objective

Write a SQL query to find the average time each machine takes to complete a process.

The processing time for each process is calculated as the difference between the 'end' timestamp and the 'start' timestamp. The average processing time for each machine should be rounded to 3 decimal places.

### Example

**Input:**

Activity table:
```
+------------+------------+---------------+-----------+
| machine_id | process_id | activity_type | timestamp |
+------------+------------+---------------+-----------+
| 0          | 0          | start         | 0.712     |
| 0          | 0          | end           | 1.520     |
| 0          | 1          | start         | 3.140     |
| 0          | 1          | end           | 4.120     |
| 1          | 0          | start         | 0.550     |
| 1          | 0          | end           | 1.550     |
| 1          | 1          | start         | 0.430     |
| 1          | 1          | end           | 1.420     |
| 2          | 0          | start         | 4.100     |
| 2          | 0          | end           | 4.512     |
| 2          | 1          | start         | 2.500     |
| 2          | 1          | end           | 5.000     |
+------------+------------+---------------+-----------+
```

**Output:**
```
+------------+-----------------+
| machine_id | processing_time |
+------------+-----------------+
| 0          | 0.894           |
| 1          | 0.995           |
| 2          | 1.456           |
+------------+-----------------+
```

**Explanation:**
- Machine 0's average time is ((1.520 - 0.712) + (4.120 - 3.140)) / 2 = 0.894
- Machine 1's average time is ((1.550 - 0.550) + (1.420 - 0.430)) / 2 = 0.995
- Machine 2's average time is ((4.512 - 4.100) + (5.000 - 2.500)) / 2 = 1.456

## Solution

```sql
SELECT A1.machine_id, ROUND(AVG(A2.timestamp - A1.timestamp), 3) AS processing_time
FROM Activity AS A1
JOIN Activity AS A2
ON A1.machine_id = A2.machine_id 
    AND A1.process_id = A2.process_id 
    AND A1.activity_type = 'start' 
    AND A2.activity_type = 'end'
GROUP BY A1.machine_id;
```

This SQL query calculates the average processing time (`processing_time`) for each machine (`machine_id`). It achieves this by joining the `Activity` table (`A1`) with itself (`A2`) on the `machine_id`, `process_id`, and activity types (`start` and `end`). It computes the difference in timestamps between the 'end' and 'start' activities and rounds the average processing time to 3 decimal places. The result is grouped by `machine_id` to obtain the average processing time for each machine.
