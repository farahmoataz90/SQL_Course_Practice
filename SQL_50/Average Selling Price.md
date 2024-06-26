## Problem Description

You are given two tables, `Prices` and `UnitsSold`, with the following structures:

### Prices Table

```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| product_id    | int     |
| start_date    | date    |
| end_date      | date    |
| price         | int     |
+---------------+---------+
```

- `(product_id, start_date, end_date)` is the primary key.
- Each row indicates the price of `product_id` during the period from `start_date` to `end_date`.
- There will be no overlapping periods for the same `product_id`.

### UnitsSold Table

```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| product_id    | int     |
| purchase_date | date    |
| units         | int     |
+---------------+---------+
```

- This table may contain duplicate rows.
- Each row indicates the date, units sold, and `product_id` of each product sold.

### Objective

Write a SQL query to find the average selling price for each product. The `average_price` should be rounded to 2 decimal places.

### Example

**Input:**

`Prices` table:
```
+------------+------------+------------+--------+
| product_id | start_date | end_date   | price  |
+------------+------------+------------+--------+
| 1          | 2019-02-17 | 2019-02-28 | 5      |
| 1          | 2019-03-01 | 2019-03-22 | 20     |
| 2          | 2019-02-01 | 2019-02-20 | 15     |
| 2          | 2019-02-21 | 2019-03-31 | 30     |
+------------+------------+------------+--------+
```

`UnitsSold` table:
```
+------------+---------------+-------+
| product_id | purchase_date | units |
+------------+---------------+-------+
| 1          | 2019-02-25    | 100   |
| 1          | 2019-03-01    | 15    |
| 2          | 2019-02-10    | 200   |
| 2          | 2019-03-22    | 30    |
+------------+---------------+-------+
```

**Output:**
```
+------------+---------------+
| product_id | average_price |
+------------+---------------+
| 1          | 6.96          |
| 2          | 16.96         |
+------------+---------------+
```

**Explanation:**

Average selling price = Total Price of Product / Number of products sold.
- Average selling price for product 1 = ((100 * 5) + (15 * 20)) / 115 = 6.96
- Average selling price for product 2 = ((200 * 15) + (30 * 30)) / 230 = 16.96

## Solution

```sql
SELECT P.product_id , IFNULL(ROUND((SUM(U.units*P.price)/SUM(U.units)),2),0) AS average_price 
FROM Prices AS P
LEFT JOIN UnitsSold AS U
ON IFNULL(P.product_id = U.product_id ,0)
WHERE (U.purchase_date BETWEEN P.start_date AND P.end_date) OR U.purchase_date IS NULL
GROUP BY P.product_id ;
```

### Explanation

1. **SELECT P.product_id, IFNULL(ROUND(SUM(U.units * P.price) / SUM(U.units), 2), 0) AS average_price**:
   - Select the `product_id` and calculate the average price.
   - Use `ROUND` to round the result to 2 decimal places.
   - Use `IFNULL` to handle cases where there might be no sales for a product.

2. **FROM Prices AS P**:
   - Select from the `Prices` table with alias `P`.

3. **LEFT JOIN UnitsSold AS U**:
   - Perform a left join with the `UnitsSold` table using alias `U`.
   - Join on `product_id` and ensure the `purchase_date` falls between `start_date` and `end_date`.

4. **GROUP BY P.product_id**:
   - Group the results by `product_id` to calculate the average price for each product.

This query will return the average selling price for each product, rounded to two decimal places.