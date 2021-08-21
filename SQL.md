# SQL

Answers to ask when designing up a database 
```md
1. What kind of thing are we storing ?
2. What properties does this thing have ?
3. What type of data does each of those properties contain.
```

1. What kind of thing are we storing --> Create Table
2. What properties does this thing have ? -> Create columns
3. What type of data does each of those properties contain. -> Data types of columns

examples of columns -> id, name,country,area,population
examples of data types -> VARCHAR ,INTEGER etc

### Keywords and Identifiers

<u><i>Keywords</i></u> </u> -> Tell the database that we want to do something. Always written out in capital letters. example: `CREATE TABLE`
<u><i>Identifiers</i></u> -> Tells the DB what thing we want to act on. Always written out in lower case letters. exg: students, name,country etc

### Column Data Types
VARCHAR -> Variable length character. Text!
INTEGET -> Numbers without a decimal. -2,147,483,647 to +2,147,483,647 (2 billion)

#### QUERIES AND STATEMENTS
A query is a single request to the database server and a query can contain multiple statements seperated by `;`

#### CREATE TABLE
```sql
CREATE TABLE table_name (column1 VARCHAR(50),column2 INTEGER);

```
#### INSERT INTO TABLE
```sql
INSERT INTO table_name (column1, column2)
VALUES ('values in single quotes',20000);
```
The order of the columns have to match the values entered. They can be in a different order but the values entered have to match their columns

#### INSERT INTO -> Multiple rows
```sql
INSERT INTO cities (name,country,population,area) 
VALUES
  ('Tokyo','Japan',39105000,8223),
  ('Shanghai','China',22125000,4015),
  ('Sao Paulo','Brazil',20935000,3043);
```

#### SELECT
Get everything with `*`
```sql
SELECT * FROM table_name
```

Get specific columns from the table by explicitly stating columns

```sql
SELECT name,population FROM cities
```

Listing columns multiple times is also valid. They are some thing 

```sql
SELECT name,name,name FROM cities
```

### We can also do operations on the values in the table

#### Integer operations
```sql
SELECT population/area FROM cities
```

This is like a virtual column and has no name. So we can use the `AS` keyword to name these virtual columns.

```sql
SELECT population/area AS population_density FROM cities
```

#### String operations


##### Pipe operator
```sql
SELECT name || country AS city_population FROM cities
gives you
```
gives you rows as

```
city_polulation

Tokyo, 38505000
Delhi, 28125000
Shanghai, 22125000
Sao Paulo, 20935000
```

#### Using functions
##### Concat function

```sql
SELECT CONCAT(name,country) AS location FROM cities
```
gives the same result as above

you can also add strings in between
```sql
SELECT name || ', ' || country FROM cities;
SELECT CONCAT(name, ', ' , country ) FROM cities ;
```
These will add a comma and a space between name and country

```
city_polulation

Tokyo, 38505000
Delhi, 28125000
Shanghai, 22125000
Sao Paulo, 20935000
```

##### LOWER AND UPPER
They mostly act like javascript functions.
```sql
SELECT LOWER(name) FROM cities
```
will give your names in lower case

```sql
SELECT UPPER(name) FROM cities
```
will ofcourse uppercase the names.
##### LENGTH returns the length of the string

```sql
SELECT LENGTH(name) FROM cities
```
will give the length of the names of each **cities**


#### Nesting multiple function calls
We can nest functions in sql
```
SELECT UPPER(CONCAT(name,', ', country)) FROM cities
```
will give the result as in concat function above but in uppercase

