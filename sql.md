#SQL

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
