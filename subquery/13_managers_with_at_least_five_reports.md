## Managers With At Least 5 Direct Reports

### Problem Statement

Table: Employee

```sql
Create table If Not Exists Employee (id int, name varchar(255), department varchar(255), managerId int)
```

id is the primary key (column with unique values) for this table. Each row of this table indicates the name of an employee, their department, and the id of their manager. If managerId is null, then the employee does not have a manager. No employee will be the manager of themselves.

Write a solution to find managers with at least five direct reports.

Return the result table in any order.

### Solution

```sql
SELECT name
FROM employee
INNER JOIN (
    SELECT managerId, COUNT(managerId) AS number_subordinates
    FROM employee
    GROUP BY managerId
    HAVING managerId IS NOT NULL
) AS managers
    ON managers.managerId = employee.id
WHERE number_subordinates >= 5;
```

### Notes

#### Explanation

A subquery is used to first count the number of subordinates of each manager. It takes the employee table and groups the rows by managerId. It filters those groups to contain only those rows where managerId is not NULL, in other words, that employee has a manager. Then, for every managerId, it counts the number of times it appears in the rows. Since each row corresponds to an employee and it only counts those employees who have a manager, the subquery correctly finds the total number of subordinates of each manager. It then joins this intermediate table to the employee table by matching the managerId value to the id value of the employee table. Since not every employee is a manager and we only care about those employees are managers, inner join is used to keep only those employees who are managers. The query then filters the data to return only those managers who have at least five subordinates.
