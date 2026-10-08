## Employee Bonus

### Problem Statement

Table: Employee

```sql
Create table If Not Exists Employee (empId int, name varchar(255), supervisor int, salary int)
```

empId is the column with unique values for this table. Each row of this table indicates the name and the ID of an employee in addition to their salary and the id of their manager.

Table: Bonus

```sql
Create table If Not Exists Bonus (empId int, bonus int)
```

empId is the column of unique values for this table. empId is a foreign key (reference column) to empId from the Employee table. Each row of this table contains the id of an employee and their respective bonus.

Write a solution to report the name and bonus amount of each employee who satisfies either of the following:

- The employee has a bonus less than 1000.
- The employee did not get any bonus.

Return the result table in any order.

### Solution

```sql
SELECT name, bonus
FROM employee
LEFT JOIN bonus
    ON employee.empId = bonus.empId
WHERE bonus < 1000 OR bonus IS NULL;
```

### Notes

#### Explanation

The bonus table is joined to the employee table by matching empId values. Since the result should include any employees who did not receive a bonus, left join is used to assign a NULL value to any employee whose empId is not listed in the bonus table. It then filters the data to return only those employees who did not receive a bonus or received a bonus less than 1000. 