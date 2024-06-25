## Problem Description

You have three tables: `Students`, `Subjects`, and `Examinations`. The `Students` table contains information about the students, the `Subjects` table contains the subjects offered in the school, and the `Examinations` table contains records of students attending exams.

### Students Table

```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| student_id    | int     |
| student_name  | varchar |
+---------------+---------+
```

- `student_id` is the primary key for this table.
- `student_name` is the name of the student.

### Subjects Table

```
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| subject_name | varchar |
+--------------+---------+
```

- `subject_name` is the primary key for this table and contains the name of the subject.

### Examinations Table

```
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| student_id   | int     |
| subject_name | varchar |
+--------------+---------+
```

- This table does not have a primary key and may contain duplicates.
- `student_id` references the student ID from the `Students` table.
- `subject_name` references the subject name from the `Subjects` table.

### Objective

Write a SQL query to find the number of times each student attended each exam. The result should include all students and all subjects

### Example

**Input:**

Students table:
```
+------------+--------------+
| student_id | student_name |
+------------+--------------+
| 1          | Alice        |
| 2          | Bob          |
| 13         | John         |
| 6          | Alex         |
+------------+--------------+
```

Subjects table:
```
+--------------+
| subject_name |
+--------------+
| Math         |
| Physics      |
| Programming  |
+--------------+
```

Examinations table:
```
+------------+--------------+
| student_id | subject_name |
+------------+--------------+
| 1          | Math         |
| 1          | Physics      |
| 1          | Programming  |
| 2          | Programming  |
| 1          | Physics      |
| 1          | Math         |
| 13         | Math         |
| 13         | Programming  |
| 13         | Physics      |
| 2          | Math         |
| 1          | Math         |
+------------+--------------+
```

**Output:**
```
+------------+--------------+--------------+----------------+
| student_id | student_name | subject_name | attended_exams |
+------------+--------------+--------------+----------------+
| 1          | Alice        | Math         | 3              |
| 1          | Alice        | Physics      | 2              |
| 1          | Alice        | Programming  | 1              |
| 2          | Bob          | Math         | 1              |
| 2          | Bob          | Physics      | 0              |
| 2          | Bob          | Programming  | 1              |
| 6          | Alex         | Math         | 0              |
| 6          | Alex         | Physics      | 0              |
| 6          | Alex         | Programming  | 0              |
| 13         | John         | Math         | 1              |
| 13         | John         | Physics      | 1              |
| 13         | John         | Programming  | 1              |
+------------+--------------+--------------+----------------+
```

**Explanation:**
- Alice attended the Math exam 3 times, the Physics exam 2 times, and the Programming exam 1 time.
- Bob attended the Math exam 1 time, the Programming exam 1 time, and did not attend the Physics exam.
- Alex did not attend any exams.
- John attended the Math exam 1 time, the Physics exam 1 time, and the Programming exam 1 time.

## Solution

```sql
SELECT ST.student_id, ST.student_name, SUB.subject_name, COUNT(E.subject_name) AS attended_exams
FROM Students AS ST
CROSS JOIN Subjects AS SUB
LEFT JOIN Examinations AS E
ON ST.student_id = E.student_id AND SUB.subject_name = E.subject_name
GROUP BY ST.student_name, SUB.subject_name, ST.student_id 
ORDER BY ST.student_id, SUB.subject_name;
```

### Explanation

1. **CROSS JOIN**: This join ensures that every student is paired with every subject, creating a complete list of student-subject combinations.
2. **LEFT JOIN**: This join ensures that even if there are no corresponding examination records for a student-subject combination, the student and subject will still appear in the result.
3. **COUNT**: Counts the number of times each student attended each exam. If a student did not attend an exam for a subject, the count will be 0.
4. **GROUP BY**: Groups the results by student name, subject name, and student ID to aggregate the counts correctly.
5. **ORDER BY**: Orders the result by student ID and subject name to match the expected output format.