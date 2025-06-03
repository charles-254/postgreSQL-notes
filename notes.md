# PostgreSQL Insert Data

To insert data into a table in PostgreSQL, we use the INSERT INTO statement.

Lets create a  working table.

```sql
CREATE TABLE students (name VARCHAR(150) , regNO VARCHAR(50), idNO INT(8), course VARCHAR(150), age INT(3));
```

## Inserting a single row

To insert data into the table we use the **INSERT INTO** keyword. Below is the syntax for inserting the data

```sql
INSERT INTO table_name (column1, column2, ...) VALUES (column1, column2, ...);
```
For example 

```sql
INSERT INTO students (name, regNO, idNO, course, age) 
VALUES ('Doe', 'sc212/0055/2021', 12345678, 'software engineering', 23);
```

In the above example, we inserted a single row into the table.

## Inserting multiple rows

To insert multiple rows into the table we use the following syntax.

```sql
INSERT INTO table_name (column1, column2, ...)
VALUES (values1, value2, ...),
       (values3, value4, ...),
       (value5, value6, ...);
```
For example

```sql
INSERT INTO students (name, regNO, idNO, course, age)
VALUES ('Doe', 'sc212/0055/2021', 12345678, 'software engineering', 21),
       ('John', 'sc200/0444/2022', 45342334, 'computer science', 24),
       ('Ann', 'sc211/03433/2023', 23457698, 'IT', 22);
```

# PostgreSQL UPDATE

To modify the value in existing records in a table, we use the keyword **UPDATE** with the following syntax.

```sql
UPDATE table_name SET column_name = value WHERE filter;
```
UPDATE specifies the table you want to update.
SET specifies the columns and the new value to be assigned.
WHERE is used to filter the row you want to update.

For example.

```sql
UPDATE students SET regNO = 'sc212/1234/2022' WHERE name = 'Doe';
```
To update multiple columns we use the following syntax:

```sql
UPDATE table_name SET column1_name = value, column2_name = value WHERE filter;
```

For example.

```sql
UPDATE students SET regNO = 'sc212/1234/2022', idNo = 90876543 WHERE name = 'Doe';
```

# PostgreSQL DELETE

## Deleting a single row
To delete records from a table we use the **DELETE** keyword with the **WHERE** keyword to specify the row or rows to be delete.

```sql
DELETE FROM table_name WHERE filter;
```
For example 

```sql
DELETE FROM students WHERE name = 'Doe';
```

## Deleting multiple rows

### Delete by list
To delete by list we use the following syntax.

```sql
DELETE FROM table_name WHERE column_name IN ('value1', 'value2', 'value3' ...);
```
For example.

```sql
DELETE FROM students WHERE name IN ('Doe', 'John', 'Ann');
```
This will delete all rows where the column **name** has values , Doe, John and Ann

### Delete by condition

Deleting by condition we use the following syntax.

```sql
DELETE FROM table_name WHERE condition;
```

For example.
```sql
DELETE FROM students WHERE  age < 18;
```
## Delete all rows

Omiting the **WHERE** keyword will delete all rows in the table.
```sql
DELETE FROM students;
```
This functions similar to **TRUNCATE TABLE** statement, which is also used to delete all rows in a table.

```sql
TRUNCATE TABLE students;
```

# PostgreSQL Upserting Data.

Upserting data in PostgreSQL involves inserting a new row into a table if it does not exist, or updating an existing row if it does. 

For example when inserting a row in the tha students table where the row to be inserted has an idNO of 1234 if a row with the idNO 1234 does not exist then postgreSQL will insert the row into the table , but is there is a row with the idNO 1234, then postgreSQL will update the row.

Below is the syntax:

```sql
INSERT INTO table_name (column1, column2, ...)
VALUES (value1, value2, ...)
ON CONFLICT (conflict_target)
DO NOTHING | DO UPDATE SET column1 = value1, column2 = value2, ...
```

For example

```sql
INSERT INTO students (name, regNO, idNO, course, age) 
VALUES ('Doe', 'sc212/0055/2021', '12345678', 'software engineering', 30)
ON CONFLICT (idNO)
DO UPDATE SET age = EXCLUDED.age
```

We can also check conflict on multiple columns and update nultiple rows 

```sql
INSERT INTO students (name, regNO, idNO, course, age) 
VALUES ('Doe', 'sc212/0055/2021', '12345678', 'software engineering', 30)
ON CONFLICT (idNO, regNo)
DO UPDATE SET age = EXCLUDED.age, name = EXCLUDED.name
```

If you do not want to update anything after a conflict has occurred, you can simply do this:

```sql
INSERT INTO students (name, regNO, idNO, course, age) 
VALUES ('Doe', 'sc212/0055/2021', '12345678', 'software engineering', 30)
ON CONFLICT (idNO, regNO)
DO NOTHING
```
