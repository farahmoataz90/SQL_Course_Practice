## Problem Description

You have two tables: `Visits` and `Transactions`.

### Visits Table

```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| visit_id    | int     |
| customer_id | int     |
+-------------+---------+
```

- `visit_id` is the column with unique values for this table.
- This table contains information about the customers who visited the mall.

### Transactions Table

```
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| transaction_id | int     |
| visit_id       | int     |
| amount         | int     |
+----------------+---------+
```

- `transaction_id` is a column with unique values for this table.
- This table contains information about the transactions made during each `visit_id`.

Write a solution to find the IDs of the users who visited without making any transactions and the number of times they made these types of visits.

Return the result table sorted in any order.

### Example

**Input:**

`Visits` Table:
```
+----------+-------------+
| visit_id | customer_id |
+----------+-------------+
| 1        | 23          |
| 2        | 9           |
| 4        | 30          |
| 5        | 54          |
| 6        | 96          |
| 7        | 54          |
| 8        | 54          |
+----------+-------------+
```

`Transactions` Table:
```
+----------------+----------+--------+
| transaction_id | visit_id | amount |
+----------------+----------+--------+
| 2              | 5        | 310    |
| 3              | 5        | 300    |
| 9              | 5        | 200    |
| 12             | 1        | 910    |
| 13             | 2        | 970    |
+----------------+----------+--------+
```

**Output:**
```
+-------------+----------------+
| customer_id | count_no_trans |
+-------------+----------------+
| 54          | 2              |
| 30          | 1              |
| 96          | 1              |
+-------------+----------------+
```

**Explanation:** 
- Customer with ID `30` visited the mall once and did not make any transactions.
- Customer with ID `96` visited the mall once and did not make any transactions.
- Customer with ID `54` visited the mall three times. During two visits, they did not make any transactions.

## Solution

```sql
SELECT
    v.customer_id,
    COUNT(*) AS count_no_trans
FROM
    Visits v
WHERE
    v.customer_id NOT IN (SELECT DISTINCT customer_id FROM Transactions)
GROUP BY
    v.customer_id;
```

This SQL query identifies customers who visited the mall without making any transactions and counts how many times each customer did so. It uses a `NOT IN` subquery to filter out visits where transactions were made. The result is then grouped by `customer_id` and sorted in any order as specified.