## Customers Who Never Order

### Problem Statement

Table: Customers

```sql
Create table If Not Exists Customers (id int, name varchar(255))
```

id is the primary key (column with unique values) for this table.
Each row of this table indicates the ID and name of a customer.

Table: Orders

```sql
Create table If Not Exists Orders (id int, customerId int)
```

id is the primary key (column with unique values) for this table.
customerId is a foreign key (reference columns) of the ID from the Customers table. Each row of this table indicates the ID of an order and the ID of the customer who ordered it.

Write a solution to find all customers who never order anything.

Return the result table in any order.

### Solution

```sql
SELECT name AS customers
FROM customers
WHERE id NOT IN (
    SELECT customerId
    FROM orders
);
```

### Notes

#### Explanation

For every row in the customers table, the query will run a subquery to check whether the id value of that row is not in the customerId column of the orders table. The time complexity for this algorithm is $O(n^2)$.

#### Other Solutions

```sql
SELECT customers.name AS customers
FROM customers
LEFT JOIN orders
    ON customers.id = orders.customerId
WHERE orders.customerId IS NULL;
``` 

This query will first add the orders table to the customers table by matching rows where the id value in the customers table is equal to the customerId value in the orders table. Left join is used to ensure that every customer in the customers table is preserved regardless of whether there is a matching id value in the orders table. The query will then filter the rows to check for NULL values in the customerId column. The time complexity for this algorithm is $O(n)$.

#### Additional info

User submitted solution claims the subquery approach may be more efficient depending on the database implementation. For large tables, the join solution would create a large intermediate table that is then filtered whereas the subquery approach checks the given tables directly. 