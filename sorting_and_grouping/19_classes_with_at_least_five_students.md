## Classes With At Least Five Students

### Problem Statement

Table: Courses

```sql
Create table If Not Exists Courses (student varchar(255), class varchar(255))
```

(student, class) is the primary key (combination of columns with unique values) for this table. Each row of this table indicates the name of a student and the class in which they are enrolled.

Write a solution to find all the classes that have at least five students.

Return the result table in any order.

### Solution

```sql
SELECT class
FROM courses
GROUP BY class
HAVING COUNT(student) >= 5;
```

### Notes

#### Why use HAVING?

HAVING filters grouped rows. In this case, it filters those classes that have less than five students. 