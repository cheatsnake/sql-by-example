# SQL by example

[`English`](README.md) [`Russian`](README_RUS.md)

SQL (Structured Query Language) is a special query language for working with relational databases (e.g. [MySQL](https://en.wikipedia.org/wiki/MySQL), [PostgreSQL](https://en.wikipedia.org/wiki/PostgreSQL), [Oracle](https://en.wikipedia.org/wiki/Oracle_Database), [MariaDB](https://en.wikipedia.org/wiki/MariaDB)). SQL queries are constructed from a set of operators, which are the usual words of the English language.

> All of the above examples were successfully tested in PostgreSQL version 15.

-   [Basics of SQL](#basics-of-sql)
    -   [Creating a new database](#creating-a-new-database)
    -   [Creating a table](#creating-a-table)
    -   [Data types](#data-types)
        -   [Numeric types](#numeric-types)
        -   [Symbol types](#symbol-types)
        -   [Date and time](#date-and-time)
        -   [Geometric types](#geometric-types)
        -   [Other](#other)
    -   [Attributes](#attributes)
    -   [Adding data](#adding-data)
    -   [Data extraction](#data-extraction)
    -   [Searching for pattern data](#searching-for-pattern-data)
    -   [Sorting data](#sorting-data)
    -   [Updating data](#updating-data)
    -   [Deleting data](#deleting-data)
    -   [Aliases](#aliases)
    -   [Editing tables](#editing-tables)
    -   [Aggregate functions](#aggregate-functions)
    -   [Grouping](#grouping)

## Basics of SQL

This section describes examples of basic operations (create/read/update/delete) for working with data in SQL tables.

### Creating a new database

```sql
CREATE DATABASE store;
```

You can define an unlimited number of tables in the database, which will store the necessary data.

> Note that each SQL query ends with a semicolon.

### Creating a table

At the table creation stage, data types are specified and various attributes are defined for all columns.

```sql
CREATE TABLE clients (
    id SERIAL PRIMARY KEY,
    firstName VARCHAR(50) NOT NULL,
    lastName VARCHAR(50) NOT NULL,
    email VARCHAR(50) UNIQUE,
    phone VARCHAR(20) UNIQUE,
    age SMALLINT NOT NULL,
    gender VARCHAR(6) NOT NULL,
    isMarried BOOLEAN,
    createdAt TIMESTAMP,
    updatedAt TIMESTAMP
);
```

### Data types

Below is a list of the main types for a PostgreSQL database.

> Other databases may have slightly different data types and descriptions. Therefore, if you encounter errors, refer to the documentation.

#### Numeric types

| Type                        | Values                                                                           | Description                                                                                                                                                                 |
| :-------------------------- | :------------------------------------------------------------------------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `smallint` `int2`           | Numbers from -32768 to +32767                                                    | Takes up 2 bytes.                                                                                                                                                           |
| `integer` `int4` `int`      | Numbers from -2147483648 to +2147483647                                          | Takes up 4 bytes.                                                                                                                                                           |
| `bigint` `int8`             | Numbers from -9223372036854775808 to +9223372036854775807                        | Takes up 8 bytes.                                                                                                                                                           |
| `numeric` `decimal`         | Numbers with an integer part up to 131072 digits and up to 131072 decimal places | Accepts 2 parameters precision (total number of digits) and scale (number of digits after the decimal point). <br> _numeric(5, 3) – 22,725_ <br> _decimal(10, 1) – 52538,4_ |
| `real` `float4`             | Numbers from 1E-37 to 1E+37                                                      | Takes up 4 bytes.                                                                                                                                                           |
| `double precision` `float8` | Numbers from 1E-307 to 1E+308                                                    | Takes up 8 bytes.                                                                                                                                                           |
| `serial`                    | Auto incrementing numeric values from 1 to 2147483647                            | It takes 4 bytes. The value for this type is selected automatically, depending on the values of the previous element. Great for unique IDs.                                 |
| `smallserial`               | Auto incrementing numeric values from 1 to 32767                                 | Занимает 2 байта.                                                                                                                                                           |
| `bigserial`                 | Auto incrementing numeric values from 1 to 9223372036854775807                   | Takes up 8 bytes.                                                                                                                                                           |

#### Symbol types

| Type                          | Values                  | Description                                                                                                                 |
| :---------------------------- | :---------------------- | :-------------------------------------------------------------------------------------------------------------------------- |
| `character` `char`            | Fixed length strings    | Accepts a parameter that specifies the number of characters in the string. <br> _char(5) – hello_                           |
| `character varying` `varchar` | Variable length strings | Accepts a parameter that specifies the **maximum** number of characters in the string. <br> _varchar(5) – abc, abcd, abcde_ |
| `text`                        | Free length text        | Suitable for storing text articles, reviews, descriptions.                                                                  |

#### Date and time

| Type                       | Values                                                               | Description        |
| :------------------------- | :------------------------------------------------------------------- | :----------------- |
| `timestamp`                | Date and time from 4713 B.C. to 294276 A.D.                          | Takes up 8 bytes.  |
| `timestamp with time zone` | Date and time from 4713 B.C. to 294276 A.D. including time zone data | Takes up 8 bytes.  |
| `date`                     | Dates from 4713 B.C. to 5874897 A.D.                                 | Takes up 4 bytes.  |
| `time`                     | Time from 00:00:00 to 24:00:00                                       | Takes up 8 bytes.  |
| `time with time zone`      | Time from 00:00:00+1459 to 24:00:00-1459                             | Takes up 12 bytes. |

#### Geometric types

| Type      | Values                                                | Description            |
| :-------- | :---------------------------------------------------- | :--------------------- |
| `point`   | Format point (x,y)                                    | Takes up 16 bytes.     |
| `line`    | Line in the format {A,B,C}                            | Takes up 32 bytes.     |
| `lseg`    | A segment in the format ((x1,y1),(x2,y2))             | Takes up 32 bytes.     |
| `box`     | Rectangle in the format ((x1,y1),(x2,y2))             | Takes up 32 bytes.     |
| `path`    | A set of connected points in the format ((x1,y1),...) | Takes up 16+16n bytes. |
| `polygon` | A polygon in the format ((x1,y1),...)                 | Takes up 40+16n bytes. |
| `circle`  | Circle in the format <(x,y),r>                        | Takes up 24 bytes.     |

#### Other

| Type      | Values                                                    | Description                                                                                                                                                    |
| :-------- | :-------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `boolean` | true / false                                              | The following values can be specified instead of true: TRUE, 't', 'true', 'y', 'yes', 'on', '1'. Instead of FALSE: FALSE, 'f', 'false', 'n', 'no', 'off', '0'. |
| `bytea`   | Data as binary strings                                    |                                                                                                                                                                |
| `json`    | JSON in text form                                         |                                                                                                                                                                |
| `jsonb`   | JSON in binary format                                     |                                                                                                                                                                |
| `uuid`    | Stores [UUID](https://en.wikipedia.org/wiki/UUID) strings |                                                                                                                                                                |
| `xml`     | Data in XML format                                        |                                                                                                                                                                |

### Attributes

Attributes allow you to specify additional properties for table columns.

-   `PRIMARY KEY` – indicates that the column stores a unique identifier.

```sql
CREATE TABLE test (
    id SERIAL PRIMARY KEY
);
```

-   `UNIQUE` – indicates that each element in the column will be unique.

```sql
CREATE TABLE emails (
    email VARCHAR(50) UNIQUE
);
```

-   `NULL` – indicates that the value in the column may be missing (by default, all columns except `PRIMARY KEY` allow no value, so you do not need to specify it explicitly.)

-   `NOT NULL` – indicates that the value in the column cannot be empty.

```sql
CREATE TABLE users (
    firstName VARCHAR(30) NOT NULL,
    lastName VARCHAR(30) NOT NULL
);
```

-   `DEFAULT` – specifies the value to be assigned by default.

```sql
CREATE TABLE messages (
    text VARCHAR(200) DEFAULT 'Hello World'
);
```

-   `CHECK` – specifies the range of values that can be stored in the column.

```sql
CREATE TABLE users (
    firstName VARCHAR(50),
    age INTEGER NOT NULL CHECK(age > 0 AND age < 100)
);
```

### Adding data

```sql
INSERT INTO clients (firstName, lastName, age, gender, isMarried)
    VALUES ('Alex', 'Smith', 25, 'male', false);
```

You can insert multiple elements at once by listing the values for the new element in new brackets:

```sql
INSERT INTO messages (title, body) VALUES
    ('MSG-1', 'Hello World'),
    ('MSG-2', 'SQL is awesome'),
    ('MSG-3', 'Have a nice day!');
```

### Data extraction

> Remember that many operators can be combined with each other.

Get all elements of the table with values of all its columns:

```sql
SELECT * FROM clients;
```

Get all elements of a table with values of certain columns:

```sql
SELECT firstName, lastName, phone FROM clients;
```

Get the first `20` elements of the table:

```sql
SELECT * FROM clients LIMIT 20;
```

Get the first `10` table elements starting from position `50` (pagination):

```sql
SELECT * FROM clients LIMIT 10 OFFSET 50;
```

Get all items where the `gender` column is equal to the value "male":

```sql
SELECT * FROM clients WHERE gender = 'male';
```

Get all elements where the `age` column is 25 and the `isMarried` column is false:

```sql
SELECT * FROM clients WHERE age = 25 AND isMarried = false;
```

Get all items where the `firstName` column is "Alex" or the `lastName` column is "Smith":

```sql
SELECT * FROM clients WHERE firstName = 'Alex' OR lastName = 'Smith';
```

Get all table elements where the `firstName` column can have one of the listed values: "John", "Mike", "Kane":

```sql
SELECT * FROM clients WHERE firstName IN ('John', 'Mike', 'Kane');
```

Get all items where the `age` column values are between 20 and 30:

```sql
SELECT * FROM clients WHERE age BETWEEN 20 AND 30;
```

Get all items where the `phone` column values are not empty:

```sql
SELECT * FROM clients WHERE phone IS NOT NULL;
```

Get all values of the `lastName` column without repetitions (i.e., only unique values):

```sql
SELECT DISTINCT(lastName) FROM clients;
```

### Searching for pattern data

The `LIKE` and `NOT LIKE` operators are used to search for pattern data.
The templates themselves use special wildcards:

-   `%` – a wildcard, which indicates that any number of characters can be in its place.
-   `_` – the wildcard, which indicates that there can only be one character in its place.

Get all elements of the table, where the value of the `firstName` column starts with the character "A":

```sql
SELECT * FROM clients WHERE firstName LIKE 'A%';
```

Get all elements of the table, where the value of the `firstName` column starts with one of the following characters: "A", "B", "C":

```sql
SELECT * FROM clients WHERE firstName LIKE '[ABC]%';
```

Get all elements of the table, where the 2nd character in the `firstName` column is not equal to "o":

```sql
SELECT * FROM clients WHERE firstName NOT LIKE '_o%';
```

### Sorting data

Get all elements of the table sorted by column `firstName` in ascending order:

```sql
SELECT * FROM clients ORDER BY firstName ASC;
```

Get all elements of the table sorted by the `age` column in descending order:

```sql
SELECT * FROM clients ORDER BY age DESC;
```

Get all elements of the table sorted by the `lastName` column in descending order, and then by the `id` column in ascending order:

```sql
SELECT * FROM clients ORDER BY lastName DESC, id ASC;
```

### Updating data

Change the value of the `phone` column in the element with an `id` column value of 42:

```sql
UPDATE clients SET phone = '+123987654' WHERE id = 42;
```

Change the values of the `city` and `age` columns with the values of `gender` = "female" and `name` = "Sophia":

```sql
UPDATE clients SET city = 'Paris', age = 33 WHERE gender = 'famale' AND name = "Sophia";
```

### Deleting data

Remove the item from the table where the `id` column value = 1:

```sql
DELETE FROM clients WHERE id = 137;
```

Remove elements from the table, where the column values `city` = "Prague" and `age` = 22:

```sql
DELETE FROM clients WHERE city = 'Prague' AND age = 22;
```

### Aliases

```sql
SELECT first_name AS name, last_name AS surname FROM clients;
```

```sh
    name     | surname
-------------+----------
 Fowler      | Ebbutt
 Huntley     | Giabucci
 Michel      | Cogman
 Bartholomew | Mecco
 Donelle     | Lambin
```

### Editing tables

Add a new column `city` to the table `clients`:

```sql
ALTER TABLE clients ADD COLUMN city VARCHAR(50);
```

Delete the `isMarried` column from the `clients` table:

```sql
ALTER TABLE clients DROP COLUMN isMarried;
```

Rename column `firstName` to `fName` in table `clients`:

```sql
ALTER TABLE clients RENAME COLUMN firstName TO fName;
```

Rename the table `clients` to `users`

```sql
ALTER TABLE clients RENAME TO users;
```

### Aggregate functions

Aggregate functions are used to summarize/count data.

Count the total number of elements in the table:

```sql
SELECT COUNT(*) FROM clients;
```

Get the maximum/minimum value of the `age` column:

```sql
SELECT MAX(age) FROM clients;
```

```sql
SELECT MIN(age) FROM clients;
```

Calculate the total sum of all elements in the `age` column:

```sql
SELECT SUM(age) FROM clients;
```

Calculate the average value of the elements of the `age` column:

```sql
SELECT AVG(age) FROM clients;
```

### Grouping

Group the data from the table `clients` by column `gender` and output in the column `total` the total number of elements for each value of `gender`:

```sql
SELECT gender, COUNT(gender) AS total FROM clients GROUP BY gender;
```

> Instead of the name of the column on which the grouping, you can specify its sequence number in the `SELECT` statement:

```sql
SELECT gender, COUNT(gender) AS total FROM clients GROUP BY 1; # Similar to the query above
```

```sh
   gender    | total
-------------+-------
 Male        |   368
 Female      |   245
```

Group the data from the table `clients` by column `gender` and then by column `age`, display for each resulting element the average value of the column `balance` and sort everything in ascending order by column `age`:

```sql
SELECT gender, age, AVG(balance) AS avg_money FROM clients GROUP BY gender, age ORDER BY age;
```

```sh
   gender    | age |       avg_money
-------------+-----+------------------------
 Male        |  18 |     31699.250000000000
 Female      |  18 |     21025.000000000000
 Male        |  19 |     16963.166666666667
 Female      |  19 |     25118.400000000000
 Male        |  20 |     23203.500000000000
 Female      |  20 |     22956.875000000000
 Male        |  21 |     19032.400000000000
 Female      |  21 |     27047.800000000000
```