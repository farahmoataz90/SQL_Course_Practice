## Problem Description

You are given a table named `Cinema` with the following structure:

### Cinema Table

```
+----------------+----------+
| Column Name    | Type     |
+----------------+----------+
| id             | int      |
| movie          | varchar  |
| description    | varchar  |
| rating         | float    |
+----------------+----------+
```

- `id` is the primary key.
- Each row contains information about a movie, its description, and its rating.
- `rating` is a float with 2 decimal places in the range [0, 10].

### Objective

Write a SQL query to find movies with an odd-numbered `id` and a `description` that is not "boring". The result should be ordered by `rating` in descending order.

### Example

**Input:**

Cinema table:
```
+----+------------+-------------+--------+
| id | movie      | description | rating |
+----+------------+-------------+--------+
| 1  | War        | great 3D    | 8.9    |
| 2  | Science    | fiction     | 8.5    |
| 3  | irish      | boring      | 6.2    |
| 4  | Ice song   | Fantacy     | 8.6    |
| 5  | House card | Interesting | 9.1    |
+----+------------+-------------+--------+
```

**Output:**

```
+----+------------+-------------+--------+
| id | movie      | description | rating |
+----+------------+-------------+--------+
| 5  | House card | Interesting | 9.1    |
| 1  | War        | great 3D    | 8.9    |
+----+------------+-------------+--------+
```

**Explanation:**

We have three movies with odd-numbered IDs: 1, 3, and 5. The movie with ID = 3 is boring, so we do not include it in the answer. 

## Solution

```sql
SELECT id, movie, description, rating
FROM Cinema 
WHERE id % 2 != 0 AND description != 'boring'
ORDER BY rating DESC;
```

### Explanation

1. **SELECT id, movie, description, rating**: Select the columns `id`, `movie`, `description`, and `rating` from the `Cinema` table.
2. **WHERE id % 2 != 0 AND description != 'boring'**: Filter the rows to include only those with an odd-numbered `id` and a `description` that is not "boring".
3. **ORDER BY rating DESC**: Sort the result by `rating` in descending order.

This query will return all movies with odd-numbered IDs and non-boring descriptions, ordered by their ratings in descending order.