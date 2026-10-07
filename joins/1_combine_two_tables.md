## Combine Two Tables

### Problem Statement

Table: Person

```sql
Create table If Not Exists Person (personId int, firstName varchar(255), lastName varchar(255))
```

personId is the primary key (column with unique values) for this table.
This table contains information about the ID of some persons and their first and last names.

Table: Address

```sql
Create table If Not Exists Address (addressId int, personId int, city varchar(255), state varchar(255))
```

addressId is the primary key (column with unique values) for this table.
Each row of this table contains information about the city and state of one person with ID = PersonId.

Write a solution to report the first name, last name, city, and state of each person in the Person table. If the address of a personId is not present in the Address table, report null instead.

### Solution

```sql
SELECT firstName, lastName, city, state
FROM person
LEFT JOIN address
    ON person.personId = address.personId;
```

### Notes

#### Why use left join?

Left join preserves every row of the person table. Since the problem statement asks to report null if the address of a person is not present in the address table, left join will preserve every person in the person table even if that person does not have an address in the address table.