## Second Highest Salary

### Problem Statement

Table: Employee

```sql
Create table If Not Exists Employee (id int, salary int)
```

id is the primary key (column with unique values) for this table.
Each row of this table contains information about the salary of an employee.

Write a solution to find the second highest distinct salary from the Employee table. If there is no second highest salary, return null.

### Solution

```sql
SELECT (
    SELECT salary 
    FROM employee
    GROUP BY salary
    ORDER BY salary DESC
    LIMIT 1 OFFSET 1
) AS secondHighestSalary
```

### Notes

#### Why two SELECT statements?
The inner SELECT statement will return the second highest distinct salary. First, the query will group the data by salary so that only distinct salaries are considered. Second, it orders those salaries in descending order. Lastly, it will return only one row offset by one i.e. the second highest salary. The outer SELECT statement is to handle the case when there is no second highest salary. If the inner SELECT statement does not return a result, the outer SELECT statement will explicitly return NULL.

#### How to improve?
The query does not generalize to finding the nth highest salary. Need to change approach for that problem.