## Sales Person

### Problem Statement

Table: SalesPerson

```sql
Create table If Not Exists SalesPerson (sales_id int, name varchar(255), salary int, commission_rate int, hire_date date)
```

sales_id is the primary key (column with unique values) for this table. Each row of this table indicates the name and the ID of a salesperson alongside their salary, commission rate, and hire date.

Table: Company

```sql
Create table If Not Exists Company (com_id int, name varchar(255), city varchar(255))
```

com_id is the primary key (column with unique values) for this table. Each row of this table indicates the name and the ID of a company and the city in which the company is located. 

Table: Orders

```sql
Create table If Not Exists Orders (order_id int, order_date date, com_id int, sales_id int, amount int)
```

order_id is the primary key (column with unique values) for this table. com_id is a foreign key (reference column) to com_id from the Company table. sales_id is a foreign key (reference column) to sales_id from the SalesPerson table. Each row of this table contains information about one order. This includes the ID of the company, the ID of the salesperson, the date of the order, and the amount paid.

Write a solution to find the names of all the salespersons who did not have any orders related to the company with the name "RED".

Return the result table in any order.

### Solution

```sql
SELECT salesperson.name
FROM orders
INNER JOIN company
    ON orders.com_id = company.com_id
    AND company.name = 'RED'
RIGHT JOIN salesperson
    ON salesperson.sales_id = orders.sales_id
WHERE company.name IS NULL;
```

### Notes

#### Explanation

The query will join the company table to the order table by matching rows where the com_id values match and the name value in the company table is 'RED'. An INNER JOIN is used to only keep those rows in the orders table relating to the company 'RED'. Then, the salesperson table is joined to this intermediate table by matching rows with the same sales_id value. A RIGHT JOIN is used to keep every row in salesperson. If a salesperson did not have an order from the company 'RED', then the RIGHT JOIN will fill the name column in the intermediate table with a NULL value. It then returns those rows where the name value is NULL. In other words, that salesperson did not have an order relating to the company 'RED'. 