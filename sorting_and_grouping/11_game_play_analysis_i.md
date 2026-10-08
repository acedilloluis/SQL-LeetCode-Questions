## Game Play Analysis I

### Problem Statement

Table: Activity

```sql
Create table If Not Exists Activity (player_id int, device_id int, event_date date, games_played int)
```

(player_id, event_date) is the primary key (combination of columns with unique values) of this table. This table shows the activity of players of some games. Each row is a record of a player who logged in and played a number of games (possibly 0) before logging out on someday using some device.

Write a solution to find the first login date for each player.

Return the result table in any order.

### Solution

```sql
SELECT player_id, MIN(event_date) AS first_login
FROM activity
GROUP BY player_id;
```

### Notes

#### Explanation

In PostgreSQL the MIN function can be used to find the earliest date. The query groups by player_id and finds the earliest date in the event_date column for that player_id.
