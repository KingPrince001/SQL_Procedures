# Procedures in SQL

 Procedures are sets of SQL statements that are grouped together to perform a specific task or a series of tasks. They provide a way to `encapsulate` and `reuse SQL code`, `enhancing code modularity`, `reusability`, and `security. `

## We shall learn how to:

1. manage stored procedures in SQL Server including;  
a.` creating, `  
b.` executing, `  
c. `modifying, `  
d. `deleting stored procedures.`
## Creating a Procedure

=> To create a procedure in SQL, use the `CREATE PROCEDURE` statement. [Shorthand:`CREATE PROC`]

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
        shoe_price
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

=> To execute a stored procedure, you use the `EXECUTE` or `EXEC` statement followed by the name of the stored procedure:

### Here is an example.

```sql
EXECUTE shoeList;
```
or

```sql
EXEC shoeList;
```

## Modifying a stored procedure.

=> To modify an existing stored procedure, you use the `ALTER PROCEDURE` statement.

### Lets change the body of the stored procedure by sorting the shoes by `shoe_prices` instead of  `shoe_names`:

```sql
CREATE PROCEDURE shoeList
AS
BEGIN
    SELECT 
        shoe_name, 
        shoe_price
    FROM 
        shoes
    ORDER BY 
        shoe_price;
END;
```

## Deleting a stored procedure

=> To delete a stored procedure, you use the` DROP PROCEDURE` or `DROP PROC` statement:

## An example

```sql
DROP PROCEDURE shoeList;
```

```sql
DROP PROC shoeList;
```

## Stored Procedures Parameters.

=> We will extend a stored procedure which allows us to pass one or more values.  
=> The result of the stored procedure will change based on the values of the parameters.

### Creating a stored procedure with one parameter:

 => Lets create a stored procedure with a parameter to find the shoes whose `shoe_price` is greater than an `input price`.

### Lets first create a `findShoes` procedure:

```sql
CREATE PROC findShoes
AS
BEGIN
    SELECT
        shoe_name,
        shoe_price
    FROM 
        shoes
    ORDER BY
        shoe_price;
END;
```

### Lets alter the procedure and add one parameter

 ```sql

 ALTER PROCEDURE findShoes(@min_price As DECIMAL)
AS
BEGIN
    SELECT
        shoe_name,
        shoe_price
    FROM 
        shoes
    WHERE
        shoe_price >= @min_price
    ORDER BY
        shoe_price;
END;
```

## Executing a stored procedure with one parameter:
=> Lets execute the procedure and pass the `min_price` as `500`.  
=> You pass an argument to it as follows:

```sql
EXEC findShoes 500;
```

## Executing a stored procedure with multiple parameters:
=> The parameters are separated by commas.

```sql
ALTER PROC findShoes(
    @min_price AS DECIMAL,
    @max_price AS DECIMAL
)
AS
BEGIN
    SELECT
        shoe_name,
        shoe_price
    FROM 
        shoes
    WHERE
        shoe_price >= @min_price AND
        shoe_price <= @max_price
    ORDER BY
        shoe_price;
END;
```

### Executing a Stored Procedure with multiple parameters

```sql
EXEC findShoes 500, 3500;
```

### Executing Stored Parameters using `Named parameters`

```sql
EXECUTE findShoes 
    @min_price = 500, 
    @max_price = 3500;
```

## Creating text parameters:

```sql 
ALTER PROCEDURE findShoes(
    @min_price AS DECIMAL,
    @max_price AS DECIMAL,
    @name AS VARCHAR(max)
)
AS
BEGIN
    SELECT
        shoe_name,
        shoe_price
    FROM 
        shoes
    WHERE
        shoe_price >= @min_price AND
        shoe_price <= @max_price AND
        shoe_name LIKE '%' + @name + '%'
    ORDER BY
        shoe_price;
END;
```
### Executing a Stored Procedure with multiple parameters

```sql
EXECUTE findShoes 
    @min_price = 500, 
    @max_price = 3500,
    @name = 'Adidas';
```

# Creating optional parameters:

## Using default values

```sql
ALTER PROCEDURE findShoes(
    @min_price AS DECIMAL = 0,
    @max_price AS DECIMAL = 10000,
    @name AS VARCHAR(max)
)
AS
BEGIN
    SELECT
        shoe_name,
        shoe_price
    FROM 
        shoes
    WHERE
        shoe_price >= @min_price AND
        shoe_price <= @max_price AND
        shoe_name LIKE '%' + @name + '%'
    ORDER BY
        shoe_price;
END;
```
## Execute 

```sql
EXECUTE findShoes
@name = 'Adidas';
```

## Using NULL as the default value

```sql
ALTER PROCEDURE findShoes(
    @min_price AS DECIMAL = 0,
    @max_price AS DECIMAL = NULL,
    @name AS VARCHAR(max)
)
AS
BEGIN
    SELECT
        shoe_name,
        shoe_price
    FROM 
        shoes
    WHERE
        shoe_price >= @min_price AND
        (@max_price IS NULL OR shoe_price <= @max_price) AND
        shoe_name LIKE '%' + @name + '%'
    ORDER BY
       shoe_ price;
END;
```

### Execute

```sql
EXECUTE findShoes
@min_price = 500,
@name = 'Vans';
```







 `THE MORE YOU KNOW, THE MORE YOU REALIZE HOW MUCH YOU DON'T KNOW. ` 
`The less you know, the more you think you know everything. ` 
 `Knowledge is humbling.  `
` Ignorance is arrogant.`
