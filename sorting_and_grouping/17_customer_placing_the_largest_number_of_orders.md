## Customer Placing The Largest Number of Orders

### Problem Statement

Table: Orders

```sql
Create table If Not Exists orders (order_number int, customer_number int)
```

order_number is the primary key (column with unique values) for this table. This table contains information about the order ID and the customer ID.

Write a solution to find the customer_number for the customer who has placed the largest number of orders.

The test cases are generated so that exactly one customer will have placed more orders than any other customer.

### Solution

```sql
SELECT customer_number
FROM (
    SELECT customer_number, COUNT(customer_number) AS counts
    FROM orders
    GROUP BY customer_number
    ORDER BY counts DESC
)
LIMIT 1;
```

### Notes

#### Other Solutions

```sql
SELECT customer_number
FROM orders
GROUP BY customer_number
ORDER BY COUNT(customer_number) DESC
LIMIT 1;
```

*The ORDER BY clause accepts an aggregate function as an argument.* 

