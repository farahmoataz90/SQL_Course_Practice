
## Problem Description

Given a `Views` table:

```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| article_id    | int     |
| author_id     | int     |
| viewer_id     | int     |
| view_date     | date    |
+---------------+---------+
```

There is no primary key (column with unique values) for this table; the table may have duplicate rows. Each row of this table indicates that some viewer viewed an article (written by some author) on some date. Note that equal `author_id` and `viewer_id` indicate the same person.

Write a solution to find all the authors that viewed at least one of their own articles.

Return the result table sorted by `id` in ascending order.

### Example

**Input:**

`Views` table:
```
+------------+-----------+-----------+------------+
| article_id | author_id | viewer_id | view_date  |
+------------+-----------+-----------+------------+
| 1          | 3         | 5         | 2019-08-01 |
| 1          | 3         | 6         | 2019-08-02 |
| 2          | 7         | 7         | 2019-08-01 |
| 2          | 7         | 6         | 2019-08-02 |
| 4          | 7         | 1         | 2019-07-22 |
| 3          | 4         | 4         | 2019-07-21 |
| 3          | 4         | 4         | 2019-07-21 |
+------------+-----------+-----------+------------+
```

**Output:**
```
+------+
| id   |
+------+
| 4    |
| 7    |
+------+
```

**Explanation:** Authors 4 and 7 have viewed their own articles.

## Solution

```sql
SELECT DISTINCT 
    author_id AS id
FROM 
    Views 
WHERE 
    author_id = viewer_id  
ORDER BY 
    id ASC;
```

This query selects distinct `author_id` values from the `Views` table where the `author_id` is equal to the `viewer_id`, indicating that the author has viewed their own article. The result is then ordered by `id` in ascending order.