## Delete Duplicate Emails

### Problem Statement

Table: Person

```postgresql
Create table If Not Exists Person (Id int, Email varchar(255))
```

id is the primary key (column with unique values) for this table.
Each row of this table contains an email. The emails will not contain uppercase letters.

Write a solution to delete all duplicate emails, keeping only one unique email with the smallest id.

### Solution

```postgresql
DELETE
FROM person
WHERE id NOT IN (
    SELECT MIN(id)
    FROM person
    GROUP BY email
);
```

### Notes

#### Explanation

For every row of person, the query will delete that row if the id value of that row is not within the results of the subquery. The subquery will group the rows of the person table by email and find the smallest id value for that email.