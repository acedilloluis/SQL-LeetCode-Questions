## Employees Earning More Than Their Managers

### Problem Statement

```postgresql
Create table If Not Exists Employee (id int, name varchar(255), salary int, managerId int)
```

id is the primary key (column with unique values) for this table.
Each row of this table indicates the ID of an employee, their name, salary, and the ID of their manager.

Write a solution to find the employees who earn more than their managers.

Return the result table in any order.

### Solution

```postgresql
SELECT employee.name as employee
FROM employee
INNER JOIN employee AS manager
    ON manager.id = employee.managerId
WHERE employee.salary > manager.salary;
```

### Notes

#### Explanation

For every employee A, the query joins the row data of the employee with the same employee id as the id listed under A's manager id column. Inner join is used so that an employee with manager id NULL is not included. Then, it filters the rows to only those where A's salary is greater than their managers and returns their name.

#### Other Solutions

```postgresql
SELECT name AS employee
FROM employee
WHERE salary > (
	SELECT salary
    FROM employee AS managers
    WHERE managers.id = employee.managerId
);
```

For every employee A, the query first uses a subquery to find the employee with the same employee id as the id listed under A's manager id column. It then compares whether A's salary is greater than that employee's salary. This solution has time complexity $O(n^2)$ whereas the first solution has time complexity $O(n)$. 