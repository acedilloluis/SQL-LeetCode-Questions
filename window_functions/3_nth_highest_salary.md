## Nth Highest Salary

### Problem Statement

Table: Employee

```sql
Create table If Not Exists Employee (Id int, Salary int)
```

id is the primary key (column with unique values) for this table.
Each row of this table contains information about the salary of an employee.

Write a solution to find the nth highest distinct salary from the Employee table. If there are less than n distinct salaries, return null.

### Solution

```sql
CREATE OR REPLACE FUNCTION NthHighestSalary(N INT) RETURNS TABLE (Salary INT) AS $$
BEGIN
  RETURN QUERY (
    SELECT (
        SELECT DISTINCT rankedSalaries.salary 
        FROM (
            SELECT 
                employee.salary, 
                DENSE_RANK() OVER (ORDER BY employee.salary DESC) AS rnk
            FROM employee
        ) AS rankedSalaries
        WHERE rnk = N
    ) AS getNthHighestSalary
  );
END;
$$ LANGUAGE plpgsql;
```

Note: Only the two SELECT statements were written as part of the solution. The function statement was provided by LeetCode.

### Notes

#### Why DENSE_RANK()?

After solving the problem Second Highest Salary, I searched online for possible solutions to solving the generalized problem. A possible solution included the DENSE_RANK window function. The function takes data, in this case employee salaries, and assigns them a rank depending on a specified order, in this case salaries from highest to lowest. The DENSE_RANK function will assign the same rank to employee salaries that are tied and assign the next lowest salary the subsequent rank with no gaps. For example, if A has a salary of $100, B a salary of $90, C a salary of $90, and D a salary of $80, the DENSE_RANK function will assign A a rank of 1, B a rank of 2, C a rank of 2, and D a rank of 3, if the order specified is salaries from highest to lowest.

The DENSE_RANK function will correctly assign the ranks by not having gaps in the ranks if there are ties in the ordering. The query will therefore correctly find the nth highest distinct salary.

#### How to do Without DENSE_RANK

```sql
CREATE OR REPLACE FUNCTION NthHighestSalary(N INT) RETURNS TABLE (Salary INT) AS $$
BEGIN
  RETURN QUERY (
    SELECT (
        SELECT DISTINCT a.salary
        FROM employee AS a
        WHERE N = (
            SELECT COUNT(DISTINCT b.salary)
            FROM employee AS b
            WHERE b.salary >= a.salary
        )
    ) AS getNthHighestSalary
  );
END;
$$ LANGUAGE plpgsql
```

This solution does not use any window functions and instead uses a subquery to count the number of distinct salaries that are greater than or equal to a given salary. The query goes through every salary in the employee table. For every salary, it counts the number of distinct salaries that are greater than or equal to that salary and discards the salaries that do not have N distinct greater salaries. The query checks for N distinct greater salaries because there should not be gaps in the ranking if there are ties. Finally, the query selects the distinct salary that has N distinct greater salaries because the data may have ties for the nth highest salary. 

#### Why two SELECT statements?

The outer SELECT statement is used to return NULL if the inner SELECT statement does not return a value.

#### See also Rank Scores

The problem rank scores also implements a query to replicate the behavior of DENSE_RANK without actually using the window function.