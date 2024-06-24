## Problem Description

Given a `World` table:

```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| name        | varchar |
| continent   | varchar |
| area        | int     |
| population  | int     |
| gdp         | bigint  |
+-------------+---------+
```

- `name` is the primary key column for this table.
- Each row of this table gives information about the name of a country, the continent to which it belongs, its area, the population, and its GDP value.

A country is considered big if:
- It has an area of at least three million (i.e., 3,000,000 km²), or
- It has a population of at least twenty-five million (i.e., 25,000,000).

Write a solution to find the name, population, and area of the big countries.

Return the result table in any order.

### Example

**Input:**

`World` table:
```
+-------------+-----------+---------+------------+--------------+
| name        | continent | area    | population | gdp          |
+-------------+-----------+---------+------------+--------------+
| Afghanistan | Asia      | 652230  | 25500100   | 20343000000  |
| Albania     | Europe    | 28748   | 2831741    | 12960000000  |
| Algeria     | Africa    | 2381741 | 37100000   | 188681000000 |
| Andorra     | Europe    | 468     | 78115      | 3712000000   |
| Angola      | Africa    | 1246700 | 20609294   | 100990000000 |
+-------------+-----------+---------+------------+--------------+
```

**Output:**
```
+-------------+------------+---------+
| name        | population | area    |
+-------------+------------+---------+
| Afghanistan | 25500100   | 652230  |
| Algeria     | 37100000   | 2381741 |
+-------------+------------+---------+
```

**Explanation:** Afghanistan and Algeria are considered big countries based on their population and area, respectively.

## Solution

```sql
SELECT 
    name, 
    population, 
    area
FROM 
    World 
WHERE 
    area >= 3000000 
    OR population >= 25000000;
```

This query selects the `name`, `population`, and `area` from the `World` table where the `area` is at least 3,000,000 km² or the `population` is at least 25,000,000.