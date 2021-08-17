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
