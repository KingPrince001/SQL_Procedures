# Procedures in SQL

 Procedures are sets of SQL statements that are grouped together to perform a specific task or a series of tasks. They provide a way to `encapsulate` and `reuse SQL code`, `enhancing code modularity`, `reusability`, and `security. `

## We shall learn how to:

1. manage stored procedures in SQL Server including;
a.` creating, `
b.` executing, `
c. `modifying, `
d. `deleting stored procedures.`
## Creating a Procedure

To create a procedure in SQL, use the `CREATE PROCEDURE` statement. [Shorthand:`CREATE PROC`]

```sql
CREATE PROCEDURE procedureName (parameters)
AS
BEGIN
  SELECT 
  -- some code goes here
   FROM 
   -- some code goes here
   WHERE 
   -- some code goes here
END;
```
### The following `SELECT` statement returns a list of products from the shoes table in the shoe company database:

```sql
SELECT
  shoe_name,
  shoe_price
FROM
  shoes
ORDER BY
  shoe_name;
  ```
### Lets create a `stored procedure` that wraps this query.

```sql
  CREATE PROCEDURE shoeList
AS
BEGIN
    SELECT 
        shoe_name, 
        price
    FROM 
        shoes
    ORDER BY 
        shoe_name;
END;
```
### In this syntax:
=> The `shoeList` is the name of the stored procedure.
=> The `AS` keyword separates the heading and the body of the stored procedure.
=> If the stored procedure has one statement, the` BEGIN` and` END `keywords surrounding the statement are optional.

## Executing a stored procedure.

To execute a stored procedure, you use the `EXECUTE` or `EXEC` statement followed by the name of the stored procedure:

### Here is an example.

```sql
EXECUTE shoeList;
```
or

```sql
EXEC shoeList;
```

## Modifying a stored procedure.