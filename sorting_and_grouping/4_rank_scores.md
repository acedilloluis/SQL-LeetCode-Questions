## Rank Scores

### Problem Statement

Table: Scores

```sql
Create table If Not Exists Scores (id int, score DECIMAL(3,2))
```

id is the primary key (column with unique values) for this table.
Each row of this table contains the score of a game. Score is a floating point value with two decimal places.

Write a solution to find the rank of the scores. The ranking should be calculated according to the following rules:

- The scores should be ranked from the highest to the lowest.
- If there is a tie between two scores, both should have the same ranking.
- After a tie, the next ranking number should be the next consecutive integer value. In other words, there should be no holes between ranks.

Return the result table ordered by score in descending order.

### Solution

```sql
SELECT s1.score, COUNT(DISTINCT s2.score) AS rank
FROM scores AS s1
LEFT JOIN scores AS s2
    ON s1.score <= s2.score
GROUP BY s1.id, s1.score
ORDER BY s1.score DESC;
```

### Notes

#### Explanation

The query will first append, to every score in the scores table, every score that is less than or equal to that score. Then the query will group the rows by every unique combination of score and id. Then for every score it will count the number of distinct scores that are less than or equal to that score, in other words its rank. It will then order the scores from highest to lowest and output the score along with its rank.

#### Using DENSE_RANK

```sql
SELECT
    score,
    DENSE_RANK() OVER (ORDER BY score DESC) AS rank
FROM scores 
```

#### See also Nth Highest Salary

The problem nth highest salary also implements a query to replicate the behavior of DENSE_RANK without actually using the window function.


