## Investments In 2016

### Problem Statement

Table: Insurance

```sql
Create Table If Not Exists Insurance (pid int, tiv_2015 float, tiv_2016 float, lat float, lon float)
```

pid is the primary key (column with unique values) for this table. Each row of this table contains information about one policy where:
- pid is the policyholder's policy ID.
- tiv_2015 is the total investment value in 2015 and tiv_2016 is the total investment value in 2016.
- lat is the latitude of the policy holder's city. It's guaranteed that lat is not NULL.
- lon is the longitude of the policy holder's city. It's guaranteed that lon is not NULL.

Write a solution to report the sum of all total investment values in 2016 tiv_2016, for all policyholders who:
- have the same tiv_2015 value as one or more other policyholders
- are not located in the same city as any other policyholder (i.e., the (lat, lon) attribute pairs must be unique).

Round tiv_2016 to two decimal places.

### Solution

```sql
SELECT ROUND(SUM(tiv_2016)::DECIMAL, 2) AS tiv_2016
FROM insurance AS a
WHERE a.tiv_2015 IN (
    SELECT b.tiv_2015
    FROM insurance AS b
    WHERE a.pid != b.pid
)
    AND (a.lat, a.lon) NOT IN (
        SELECT c.lat, c.lon
        FROM insurance AS c
        WHERE a.pid != c.pid 
    );
```

### Notes

#### Explanation

For every row in the insurance table, two subqueries are used to check whether that row's tiv_2015 value appears in any other row of the insurance table and whether that rows lat and lon values are unique. Since pid is the primary key for the table, the subqueries check those rows where pid values differ.

#### Improvements

The time complexity for the above solution is $O(n^2)$. Another user submitted a solution using window functions and with time complexity $O(n log n)$ or $O(n)$ depending on the database engine's optimization of the PARTITION BY clause.

```sql
WITH CheckedRows AS (
    SELECT
        tiv_2016,
        COUNT(*) OVER (PARTITION BY tiv_2015) AS tiv_2015_count,
        COUNT(*) OVER (PARTITION BY lat, lon) AS loc_count
    FROM
        Insurance
)

SELECT
    ROUND(SUM(tiv_2016)::numeric, 2) AS tiv_2016
FROM
    CheckedRows
WHERE
    tiv_2015_count > 1
    AND loc_count = 1
```

This query counts the number of rows that share the tiv_2015 as the current row and the number of rows that share the same (lat, lon) values as the current row. It then sums the values of tiv_2016 for rows where it shares its tiv_2015 with another row and where its (lat, lon) pair is unique. 