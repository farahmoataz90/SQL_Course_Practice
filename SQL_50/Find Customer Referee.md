
## Problem Description

Given a `Customer` table:

```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| name        | varchar |
| referee_id  | int     |
+-------------+---------+
```

- `id` is the primary key column for this table.
- Each row of this table indicates the `id` of a customer, their `name`, and the `id` of the customer who referred them.

Find the names of the customers that are not referred by the customer with `id = 2`.

Return the result table in any order.

### Example

**Input:**

`Customer` table:
```
+----+------+------------+
| id | name | referee_id |
+----+------+------------+
| 1  | Will | null       |
| 2  | Jane | null       |
| 3  | Alex | 2          |
| 4  | Bill | null       |
| 5  | Zack | 1          |
| 6  | Mark | 2          |
+----+------+------------+
```

**Output:**
```
+------+
| name |
+------+
| Will |
| Jane |
| Bill |
| Zack |
+------+
```

**Explanation:** Only customers Alex and Mark are referred by customer with id 2, so their names are not included in the output.

## Solution

```sql
SELECT 
    name
FROM 
    Customer 
WHERE 
    referee_id != 2 
    OR referee_id IS NULL;
```

This query selects the `name` from the `Customer` table where the `referee_id` is not equal to 2 or is `NULL`.