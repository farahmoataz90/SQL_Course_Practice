## Problem Description

You are given two tables: `Signups` and `Confirmations`.

### Signups Table

```
+----------------+----------+
| Column Name    | Type     |
+----------------+----------+
| user_id        | int      |
| time_stamp     | datetime |
+----------------+----------+
```

- `user_id` is the primary key.
- Each row contains information about the signup time for the user with ID `user_id`.

### Confirmations Table

```
+----------------+----------+
| Column Name    | Type     |
+----------------+----------+
| user_id        | int      |
| time_stamp     | datetime |
| action         | ENUM     |
+----------------+----------+
```

- `(user_id, time_stamp)` is the primary key.
- `user_id` is a foreign key referencing the `Signups` table.
- `action` is an ENUM of type (`'confirmed'`, `'timeout'`).
- Each row indicates that the user with ID `user_id` requested a confirmation message at `time_stamp`, which was either confirmed or expired without confirming.

### Objective

Write a SQL query to find the confirmation rate of each user. The confirmation rate is the number of `'confirmed'` messages divided by the total number of requested confirmation messages. If a user did not request any confirmation messages, their confirmation rate is 0. Round the confirmation rate to two decimal places.

### Example

**Input:**

Signups table:
```
+---------+---------------------+
| user_id | time_stamp          |
+---------+---------------------+
| 3       | 2020-03-21 10:16:13 |
| 7       | 2020-01-04 13:57:59 |
| 2       | 2020-07-29 23:09:44 |
| 6       | 2020-12-09 10:39:37 |
+---------+---------------------+
```

Confirmations table:
```
+---------+---------------------+-----------+
| user_id | time_stamp          | action    |
+---------+---------------------+-----------+
| 3       | 2021-01-06 03:30:46 | timeout   |
| 3       | 2021-07-14 14:00:00 | timeout   |
| 7       | 2021-06-12 11:57:29 | confirmed |
| 7       | 2021-06-13 12:58:28 | confirmed |
| 7       | 2021-06-14 13:59:27 | confirmed |
| 2       | 2021-01-22 00:00:00 | confirmed |
| 2       | 2021-02-28 23:59:59 | timeout   |
+---------+---------------------+-----------+
```

**Output:**

```
+---------+-------------------+
| user_id | confirmation_rate |
+---------+-------------------+
| 6       | 0.00              |
| 3       | 0.00              |
| 7       | 1.00              |
| 2       | 0.50              |
+---------+-------------------+
```

**Explanation:**

- User 6 did not request any confirmation messages, so the confirmation rate is 0.
- User 3 made 2 requests and both timed out, so the confirmation rate is 0.
- User 7 made 3 requests and all were confirmed, so the confirmation rate is 1.
- User 2 made 2 requests: one was confirmed and the other timed out. The confirmation rate is 1 / 2 = 0.5.

## Solution

```sql
SELECT S.user_id, 
       ROUND(IFNULL(AVG(C.action = 'confirmed'), 0), 2) AS confirmation_rate
FROM Signups AS S
LEFT JOIN Confirmations AS C 
ON S.user_id = C.user_id 
GROUP BY S.user_id;
```

### Explanation

1. **LEFT JOIN**: Join the `Signups` table with the `Confirmations` table on `user_id` to get all users and their corresponding confirmation actions.
2. **IFNULL**: Use `IFNULL` to handle cases where a user has no confirmation requests. If a user has no entries in the `Confirmations` table, their confirmation rate is set to 0.
3. **AVG**: Calculate the average of the confirmation status for each user, where `C.action = 'confirmed'` is treated as 1 and other values as 0.
4. **ROUND**: Round the confirmation rate to two decimal places.
5. **GROUP BY**: Group the results by `user_id` to get the confirmation rate for each user.

This query will provide the required confirmation rates for all users in the `Signups` table, including those who did not request any confirmation messages.