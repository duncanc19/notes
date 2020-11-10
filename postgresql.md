CREATE DATABASE my_database

USE my_database - switch to a different database, this lets us execute queries against postgres without specifying the database in each query

\dt - show all tables in our database

Create a schema - CREATE TABLE:
```sql
CREATE TABLE fruit ( 
  id SERIAL PRIMARY KEY,
  name VARCHAR NOT NULL 
);
```

VARCHAR refers to a data type of a field (or column) in a Database Management System which can hold letters and numbers

PRIMARY KEY - a key in a relational database that is unique for each record

NOT NULL - required

SERIAL - data type allows you to automatically generate unique integer

\dt will now display this:
```sql
+------+-----+-----+--------+
|Schema|Name |Type |Owner   |
|------+-----+-----+--------|
|public|fruit|table|workshop|
+------+-----+-----+--------+
```
Naming convention - put table name in the singular to represent what each row is, i.e. each row is one fruit, so the table is fruit

```sql
INSERT INTO fruit ( name ) 
  VALUES (
    'Orange' 
  );
  
SELECT * FROM fruit;

CREATE TABLE harvest ( 
  id SERIAL PRIMARY KEY,
  fruit_id INT NOT NULL REFERENCES fruit(id),
  date DATE NOT NULL,
  yield INT NOT NULL
);
```
REFERENCES fruit(id) - links the variable to the id variable in the fruit table

```sql
INSERT INTO harvest ( fruit_id, date, yield ) 
  VALUES (
    1, '2018-01-01', 102919
  );
```  
  
Where style command
```sql
SELECT * FROM harvest, fruit
WHERE fruit.id = harvest.fruit_id;

SELECT * FROM harvest
INNER JOIN fruit ON fruit.id = harvest.fruit_id;
```

#### Aliases

```sql
SELECT 
  harvest.date as harvest_date, 
  harvest.yield as harvest_yield, 
  fruit.name as fruit_name 
FROM harvest, fruit 
WHERE fruit.id = harvest.fruit_id; 
```

Add several rows into a table at once:

```sql
INSERT INTO fruit ( name ) 
  VALUES ('Kiwi'), 
         ('Mango'), 
         ('Pineapple'), 
         ('Guava'), 
         ('Tomato');
 ```        
         
 INNER JOIN is the most restrictive type of join - it will only display rows which match on the joined columns
 
(INNER) JOIN: Returns records that have matching values in both tables
LEFT (OUTER) JOIN: Returns all records from the left table, and the matched records from the right table
RIGHT (OUTER) JOIN: Returns all records from the right table, and the matched records from the left table
FULL (OUTER) JOIN: Returns all records when there is a match in either left or right table

```sql
SELECT
   harvest.date as harvest_date,
   harvest.yield as harvest_yield,
   fruit.name as fruit_name
 FROM fruit
 LEFT JOIN harvest ON fruit.id = harvest.fruit_id;
+----------------+-----------------+--------------+
| harvest_date   | harvest_yield   | fruit_name   |
|----------------+-----------------+--------------|
| 2018-01-01     | 102919          | Orange       |
| 2019-01-08     | 141000          | Mango        |
| <null>         | <null>          | Guava        |
| <null>         | <null>          | Mango        |
| <null>         | <null>          | Pineapple    |
| <null>         | <null>          | Kiwi         |
| <null>         | <null>          | Lemon        |
| <null>         | <null>          | Banana       |
| <null>         | <null>          | Tomato       |
+----------------+-----------------+--------------+

my_database> SELECT
   harvest.date as harvest_date,
   harvest.yield as harvest_yield,
   fruit.name as fruit_name
 FROM harvest
 RIGHT JOIN fruit ON fruit.id = harvest.fruit_id;
```

Returns the same because harvest and fruit have been switched, so harvest is the left and fruit is the right

Delete TABLE table_name - DROP table

ALTER TABLE table_name

Add column

ALTER TABLE fruit
  ADD COLUMN discontinued_on DATE NULL;
  

### Indexes

By default PRIMARY KEY columns have indexes, it does not create an index for any foreign key REFERENCES.

It is good practice to ensure there are indexes on any columns use in WHERE or JOIN .. ON ... clauses.

Adding an index:

CREATE INDEX ON harvest (fruit_id);

Get count of rows in table:

SELECT count(*) FROM fruit;

We can find out how postgres is going to “plan” to perform a query by prepending EXPLAIN ANALYSE to a query.

EXPLAIN ANALYSE SELECT * FROM fruit 
  WHERE discontinued_on > '2020-01-01';
  
This command, without an index, will plan a Seq Scan on fruit

If you add an index on discontinued_on, it will switch to an index scan - Index Scan using fruit_discontinued_on_idx on fruit:
CREATE INDEX ON fruit (discontinued_on);

By default ADD INDEX will lock a table for writes.
For large tables, adding an index could take several hours. This is problematic as this is effectively downtime from the perspective of your customers.

Always add indexes as early as possible as it’s easier to add indexes to small datasets.

Postgres does provide a way to CREATE INDEX CONCURRENTLY which doesn’t lock the table. There are a few caveats to this approach.

Postgres may silently fail to create the index.
These broken indexes still cause a penalty to writes
Creating an index concurrently is more expensive and may slow down queries (since it uses CPU/Memory).
