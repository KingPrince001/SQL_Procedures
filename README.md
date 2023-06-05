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

## VARIABLES

=> A variable is an object that holds a single value of a specific type e.g., `integer`, `date`, or `varying character string`.

### Where do we use variables?

(a) As a loop counter to count the number of times a `loop` is performed.  
(b) To hold a value to be tested by a control-of-flow statement such as` WHILE`.  
(c) To store the value returned by a `stored procedure` or a `function`.

## We will learn how to:

=> Declare variables.  
=> Set their values.  
=> Assign value fields of a record to variables.

## Declaring a Variable.

=> To declare a variable, you use the `DECLARE` statement.

```sql
DECLARE @price DECIMAL;
```

=> The `DECLARE `statement initializes a variable by assigning it a` name` and a `data type`.  
=>  The `variable name` must start with the` @ `sign.  
=> By default, when a `variable is declared`, its `value` is set to `NULL`.  
=> Between the `variable name` and `data type`, you can use the optional `AS` keyword as follows:

```sql
DECLARE @price AS DECIMAL;
```

=> To declare` multiple variables`, you separate variables by `commas`:  

```sql
DECLARE @price AS DECIMAL,
        @brand AS VARCHAR(MAX);
```

## Assigning a value to a variable.

=> To assign a value to a variable, you use the `SET` statement.

```sql
SET @price = 500;
```
=> The `SET` statement assigns a value to a variable.

## Using variables in a query.

=> The following `SELECT` statement uses the `@price `variable in the `WHERE` clause to find the shoes of a specific price:

```sql
SELECT
    shoe_name,
    shoe_size,
    shoe_price 
FROM 
    Shoes
WHERE 
    shoe_price = @price
ORDER BY
    shoe_name;
```

=> Now, I want us to put everything together that we have learnt so far about variables and  
   execute a code block to get a list of shoes whose price  is 500:

```sql 
DECLARE @price AS DECIMAL;

SET @price = 500;

SELECT
    shoe_name,
    shoe_size,
    shoe_price 
FROM 
    Shoes
WHERE 
    shoe_price = @price
ORDER BY
    shoe_name;
```

## Stored Procedure Output Parameters.

=> We will learn how to use the `output parameters` to return data back to the `calling program`.

### Creating output parameters.

=> To create an `output parameter` for a `stored procedure`, you use the following syntax:

```sql
parameter_name data_type OUTPUT
```
=>  Output parameters can be in any valid data type e.g., `integer`, `date`, and `varying character`.

### Lets do an example:
=>  The following `stored procedure` finds shoes by name and returns the number of shoes via the `@shoe_count` output parameter:

```sql
CREATE PROCEDURE findShoesByName (
    @shoe_name VARCHAR(MAX),
    @shoe_count INT OUTPUT
) AS
BEGIN
    SELECT 
        shoe_name,
        shoe_price
    FROM
        Shoes
    WHERE
        shoe_name = @shoe_name;

    SELECT @shoe_count = @@ROWCOUNT;
END;
```

=>  We've assigned the number of rows returned by the query`(@@ROWCOUNT)` to the `@shoe_count` parameter.  
=> `@@ROWCOUNT` is a system variable that returns the number of rows read by the previous statement.

## Calling stored procedures with output parameters.
=> First,` declare variables` to hold the values returned by the output parameters.    
=> Second, use these `variables` in the stored procedure call.

### Lets execute the above stored procedure.

```sql
DECLARE @count INT;

EXEC findShoesByName
    @shoe_name = 'Nike',
    @shoe_count = @count OUTPUT;

SELECT @count AS 'Number of products found';
```

## Stored Procedure with RETURN Value.

```sql
CREATE PROCEDURE IsShoeExists
    @shoe_name INT,
    @exists BIT OUTPUT
AS
BEGIN
    IF EXISTS (SELECT 1 FROM Shoes WHERE shoe_name = @shoe_name)
        SET @exists = 1;
    ELSE
        SET @exists = 0;
    RETURN @exists;
END;
```

=> This example defines a stored procedure `IsShoeExists` that checks if a shoe with the specified` @shoe_name` exists in the `Shoes` table. It assigns the result to the output parameter `@exists` and returns the value as the` return value` of the stored procedure.



