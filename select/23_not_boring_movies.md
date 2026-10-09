## Not Boring Movies

### Problem Statement

Table: Cinema

```sql
Create table If Not Exists cinema (id int, movie varchar(255), description varchar(255), rating float(2, 1))
```

id is the primary key (column with unique values) for this table. Each row contains information about the name of a movie, its genre, and its rating. rating is a 2 decimal places float in the range [0, 10].

Write a solution to report the movies with an odd-numbered ID and a description that is not "boring".

Return the result table ordered by rating in descending order.

### Solution

```sql
SELECT *
FROM cinema
WHERE cinema.id % 2 = 1
    AND cinema.description NOT LIKE 'boring'
ORDER BY cinema.rating DESC;
```

### Notes

#### NOT LIKE

Note that NOT LIKE is case insensitive.