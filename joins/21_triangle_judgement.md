## Triangle Judgement

### Problem Statement

Table: Triangle

```sql
Create table If Not Exists Triangle (x int, y int, z int)
```

(x, y, z) is the primary key column for this table. Each row of this table contains the lengths of three line segments.

Report for every three line segments whether they can form a triangle.

Return the result table in any order.

### Solution

```sql
SELECT 
    x, y, z,
    CASE 
        WHEN x + y > z 
        AND x + z > y 
        AND y + z > x THEN 'Yes' 
        ELSE 'No'
    END AS triangle
FROM triangle;
```

### Notes

#### Explanation

The query uses the triangle inequality to test whether the values of x, y, z form a triangle. The triangle inequality tests whether the sum of any two values is greater than the third. If not, then no triangle is formed. The query uses a case statement to test every combination of the inequality and creates a new column in the triangle table with the desired value.