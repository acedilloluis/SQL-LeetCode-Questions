## Department Highest Salary

### Problem Statement

Table: Employee

```sql
Create table If Not Exists Employee (id int, name varchar(255), salary int, departmentId int)
```

id is the primary key (column with unique values) for this table.
departmentId is a foreign key (reference columns) of the ID from the Department table. Each row of this table indicates the ID, name, and salary of an employee. It also contains the ID of their department.

Table: Department

```sql
Create table If Not Exists Department (id int, name varchar(255))
```

id is the primary key (column with unique values) for this table. It is guaranteed that department name is not NULL. Each row of this table indicates the ID of a department and its name.

Write a solution to find employees who have the highest salary in each of the departments.

Return the result table in any order.

### Solution

```sql
SELECT 
    department.name AS department, 
    employee.name AS employee, 
    employee.salary
FROM employee
INNER JOIN department
    ON employee.departmentId = department.id
WHERE (department, salary) IN (
    SELECT department, MAX(salary)
    FROM employee
    GROUP BY departmentId
);
```

### Notes

#### Explanation

The query will first join the employee table and the department table by matching rows where the departmentId value of the employee table matches the id value of the department table. Inner join is used to only get rows where there is a matching departmentId value and id value. Then for every row of this intermediate table, a subquery is run to check whether the department and salary value is in the result of this subquery. The subquery returns the results of the max salary for every department listed in the department table. 