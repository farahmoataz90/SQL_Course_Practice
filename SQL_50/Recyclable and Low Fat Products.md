Sure, here is the problem description and solution formatted for inclusion in a README file:

---

## Problem Description

Given a `Products` table:

```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| product_id  | int     |
| low_fats    | enum    |
| recyclable  | enum    |
+-------------+---------+
```

- `product_id` is the primary key (column with unique values) for this table.
- `low_fats` is an ENUM (category) of type ('Y', 'N') where 'Y' means this product is low fat and 'N' means it is not.
- `recyclable` is an ENUM (category) of type ('Y', 'N') where 'Y' means this product is recyclable and 'N' means it is not.

Write a solution to find the IDs of products that are both low fat and recyclable.

Return the result table in any order.

### Example

**Input:**

`Products` table:
```
+-------------+----------+------------+
| product_id  | low_fats | recyclable |
+-------------+----------+------------+
| 0           | Y        | N          |
| 1           | Y        | Y          |
| 2           | N        | Y          |
| 3           | Y        | Y          |
| 4           | N        | N          |
+-------------+----------+------------+
```

**Output:**
```
+-------------+
| product_id  |
+-------------+
| 1           |
| 3           |
+-------------+
```

**Explanation:** Only products 1 and 3 are both low fat and recyclable.

## Solution

```sql
SELECT 
    product_id 
FROM 
    Products
WHERE 
    low_fats = 'Y' 
    AND recyclable = 'Y';
```

This query selects the `product_id` from the `Products` table where the `low_fats` and `recyclable` columns both have a value of 'Y'.