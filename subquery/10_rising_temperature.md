## Rising Temperature

### Problem Statement

Table: Weather

```postgresql
Create table If Not Exists Weather (id int, recordDate date, temperature int)
```

id is the column with unique values for this table. There are no different rows with the same recordDate. This table contains information about the temperature on a certain day.

Write a solution to find all dates' id with higher temperatures compared to its previous dates (yesterday).

Return the result table in any order.

### Solution

```postgresql
SELECT current_weather.id
FROM weather AS current_weather
WHERE current_weather.temperature > (
    SELECT yesterday_weather.temperature
    FROM weather AS yesterday_weather
    WHERE current_weather.recordDate - yesterday_weather.recordDate = 1
);
```

### Notes

#### Explanation

For every row in the weather table, the query will check whether that row's temperature value is greater than the result of the subquery. The subquery will select the row in the weather table with a recordDate value that is one less than the recordDate value of the current row.