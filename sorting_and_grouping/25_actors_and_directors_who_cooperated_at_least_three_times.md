## Actors and Directors who Cooperated at Least Three Times

### Problem Statement

Table: ActorDirector

```sql
Create table If Not Exists ActorDirector (actor_id int, director_id int, timestamp int)
```

timestamp is the primary key (column with unique values) for this table.

Write a solution to find all the pairs (actor_id, director_id) where the actor has cooperated with the director at least three times.

Return the result table in any order.

### Solution

```sql
SELECT actor_id, director_id
FROM actordirector
GROUP BY actor_id, director_id
HAVING COUNT(*) >= 3;
```

### Notes

#### GROUP BY Multiple Columns

When using the GROUP BY clause with multiple columns, the query will group by every unique combination of the values of those columns. In this case, it will group by every unique combination of actor_id and director_id values. 