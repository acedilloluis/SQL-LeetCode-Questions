## Biggest Number

### Problem Statement

Table: MyNumbers

```sql
Create table If Not Exists MyNumbers (num int)
```

This table may contain duplicates (In other words, there is no primary key for this table in SQL). Each row of this table contains an integer.

A single number is a number that appeared only once in the MyNumbers table.

Find the largest single number. If there is no single number, report null.

### Solution

```sql
SELECT MAX(num) AS num
FROM (
    SELECT num, COUNT(num)
    FROM mynumbers
    GROUP BY num
)
WHERE count = 1;
```

### Notes

#### Changes

It is also possible to use a HAVING clause rather than a WHERE clause. e.g.

```sql
SELECT MAX(num) AS num
FROM (
    SELECT num
    FROM mynumbers
    GROUP BY num
    HAVING COUNT(num) = 1
);
```