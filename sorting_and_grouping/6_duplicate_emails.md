## Duplicate Emails

### Problem Statement

Table: Person

```postgresql
Create table If Not Exists Person (id int, email varchar(255))
```

id is the primary key (column with unique values) for this table.
Each row of this table contains an email. The emails will not contain uppercase letters.

Write a solution to report all the duplicate emails. Note that it's guaranteed that the email field is not NULL.

Return the result table in any order.

### Solution

```postgresql
SELECT email
FROM person
GROUP BY email
HAVING COUNT(email) > 1;
```

### Notes

#### Explanation

Query will first group all the emails. Then it will count the number of times each distinct email appears in the data. It will then return only those emails where the amount of times the email appeared in the data is more than one.

#### Other Solutions

```postgresql
SELECT DISTINCT person.email AS email
FROM person
LEFT JOIN person AS duplicate
    ON  person.email = duplicate.email
WHERE person.id != duplicate.id;
```

This query will use a self join to add a row with the same email as another row to that row. The query will then filter the data to select only those rows where id the values of the original table are different from those in the duplicate table. In other words, it selects only those rows that have the same email but are owned by a different person. It then selects the distinct emails from those filtered rows.
