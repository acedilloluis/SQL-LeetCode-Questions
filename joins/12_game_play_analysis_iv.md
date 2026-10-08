## Game Play Analysis IV

### Problem Statement

Table: Activity

```sql
Create table If Not Exists Activity (player_id int, device_id int, event_date date, games_played int)
```

(player_id, event_date) is the primary key (combination of columns with unique values) of this table. This table shows the activity of players of some games. Each row is a record of a player who logged in and played a number of games (possibly 0) before logging out on someday using some device.

Write a solution to report the fraction of players that logged in again on the day after the day they first logged in, rounded to 2 decimal places. In other words, you need to determine the number of players who logged in on the day immediately following their initial login, and divide it by the number of total players.

### Solution

```sql
WITH total_players_login_day_after_first_login AS (
    SELECT COUNT(a.player_id) AS player_count
    FROM activity AS a
    INNER JOIN (
        SELECT b.player_id, MIN(b.event_date) AS earliest_login
        FROM activity AS b
        GROUP BY b.player_id
    ) AS earliest_logins
        ON a.player_id = earliest_logins.player_id
    WHERE a.event_date - earliest_login = 1
)

SELECT ROUND(
    player_count::decimal / (
            SELECT COUNT(DISTINCT o.player_id)
            FROM activity as o
        ), 2
) AS fraction
FROM total_players_login_day_after_first_login;
```

### Notes

#### Explanation

The query uses a CTE (common table expression) to find the total number of players who logged in the day after they first logged in. The CTE first joins the table earliest_logins, which contains the earliest login date for each player_id, to the activity table. An inner join is used since each player_id in the activity table will have a matching player_id in the earliest_login table. Then, the CTE will filter the table to only those rows where the difference between event_date value and the earliest_login value is one. In other words, the event_date value is the day after the earliest_login value. It then counts the number of player_id values where event_date - earliest_login = 1. The main query then uses a subquery to find the total number of distinct player_id's in the activity table and divides the result of the CTE by that value. The player_count value is first casted with decimal to ensure that integer division is not used. Finally, the ROUND function is used to round the result to two decimal places.

#### Other Solutions

```sql
SELECT ROUND(
    (
        SELECT COUNT(a.player_id) 
        FROM activity AS a
        INNER JOIN (
            SELECT b.player_id, MIN(b.event_date) AS earliest_login
            FROM activity AS b
            GROUP BY b.player_id
        ) AS earliest_logins
            ON a.player_id = earliest_logins.player_id
        WHERE a.event_date - earliest_login = 1
    ) / 
    (COUNT(DISTINCT o.player_id) * 1.0), 2
) AS fraction
FROM activity AS o;
```

The logic is the same as the above solution. However, using a CTE allows the result total_players_login_day_after_first_login to be used elsewhere in the query if needed.