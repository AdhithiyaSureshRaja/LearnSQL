# List of sql quiries from bro code video
Source : https://www.youtube.com/watch?v=5OdVJbNCSso
Note: <> means have to write your own database, table , coloumn names and datatypes. Keywords are uppercase but in SQL they are case insensitive . [] means optional . // means comment.
{} means alternative ;



## 1.Database


### To create a new database:

    CREATE DATABASE <database_name>;

### To use a database:

    USE <database_name>;

### To delete a database:

    DROP DATABASE <database_name>;

### To change a database to Read only:

    ALTER DATABASE <database_name> READ ONLY = 1;

Note: Cannot Drop Database in read only mode.

### To show all databases in server:

    SHOW DATABASES;



## 2.Tables


### To create a new table:

    CREATE TABLE <table_name> (
        <column_name_1> <data_type>,
        <column_name_2> <data_type>,
        <column_name_3> <data_type>
        );
Note: data_type can be varchar(size) , int , float , date , time , datetime , decimal(total number of digits , digits after digits ) , boolean etc.

### To show all columns in table:

    SELECT * FROM <table_name>;

### To show specific columns in table:

    SELECT <column_name> FROM <table_name>;

### To show all tables in database:

    SHOW TABLES;

### To delete a table:

    DROP TABLE <table_name>;

### To rename a table:

    RENAME TABLE <table_name> TO <new_table_name>;

### To add a new coloumn in table:

    ALTER TABLE <table_name> ADD <column_name> <data_type>;

### To rename a coloumn in table:

    ALTER TABLE <table_name> RENAME COLUMN <old_column_name> TO <new_coloumn_name>; 
    
### To change the datatype of a column:

    ALTER TABLE <table_name> MODIFY COLUMN <column_name> <new_data_type>;

 Note:

Let's break down what happens when you alter the salary column from decimal(6,2) to decimal(3,1) or varchar(10) after inserting data:

*Scenario 1: Altering to decimal(3,1)*

- The existing data 60000.50 will be truncated to fit the new precision and scale.
- Since decimal(3,1) has a total of 3 digits, with 1 digit after the decimal point, the value will be rounded to 999.9 (the maximum value that can be stored in decimal(3,1)).
- The actual value 60000.50 will be lost, and you'll end up with a value like 999.9, which is incorrect.

*Scenario 2: Altering to varchar(10)*

- The existing data 60000.50 will be converted to a string and stored as is.
- You can still retrieve the original value as a string, but you'll lose the ability to perform numeric operations on it.
- If you try to perform arithmetic operations or comparisons on the salary column, you might encounter errors or unexpected behavior due to the data type mismatch.


### To change the position of a column: 

    ALTER TABLE <table_name> MODIFY [COLUMN] <column_name> <data_type> AFTER <another_column_name>;

    ALTER TABLE <table_name> MODIFY [COLUMN] <column_name> <data_type> FIRST;

### To delete a column:

    ALTER TABLE <table_name> DROP <column_name>;



## 3.Insert Rows 


### To insert a new row into a table:

    INSERT INTO <table_name> VALUES(  <first_column_data> , <second_column_data> , <third_column_data> );

### To insert multiple rows into a table:

    INSERT INTO <table_name> VALUES( <first_column_data> , <second_column_data> , <third_column_data>) , ( <first_column_data> , <second_column_data> , <third_column_data>) , ( <first_column_data> , <second_column_data> , <third_column_data>) ;

Note: If number of values in VALUES() inside parenthesis doesnt match number of columns in table it throws error.

### To insert new data into specific columns:

    INSERT INTO <table_name> ( <column_name_1> , <column_name_2> ) VALUES ( <first_column_data> , <second_column_data> );

Note: The other columns without the data will be NULL



## 4.Select Data from Table


### To select all data in a table:

    SELECT * FROM <table_name>;

### To select all data in a specific column:

    SELECT <column_name_1> , <column_name_2> FROM <table_name> ;

Note: Order of the column name given after SELECT keyword matters . That is <column_name_1> , <column_name_2> != <column_name_2> , <column_name_1>

### To select specific rows in the table using where clause:

    SELECT * FROM <table_name> WHERE <column_name> <conditional_expression> <data>;

    SELECT * FROM <table_name> WHERE <column_name> IS NULL {NOT NULL};

Note: <conditional_expression> includes things like = , > , < , >= , <= , !=



## 5.Update and Delete data


### To update data in a table for exsiting row:

    UPDATE <table_name> SET <column_name_1> = <data_1> , <column_name_2> = <data_2> , <column_name_3> = <data_3>  WHERE <column_name_4> <conditional_expression> <data_4> [AND  <column_name_5> = <data_5>] ;

    UPDATE <table_name> SET <column_name_1> = NULL WHERE <column_name_2> = <data_2>;

Note: The columns given in SET can also be given in WHERE. Example SET employee_id = 3 WHERE employee_id = 4; 
If no where is given , all values of the SET column will be updated to that particular data.

### To delete all values of a table:

    DELETE FROM <table_name>;

### To delete a specific value in a table:

    DELETE FROM <table_name> WHERE <column_name_1> <conditional_expression> <data_1>;



## 6.Autocommit , Commit and Rollback


### To turn off autocommit:

    SET AUTOCOMMIT = OFF;

Note: By default autocommit is ON. Commits are like a savepoint.

### To manually commit:

    COMMIT;

### To restore previous savepoint:

    ROLLBACK;



## 7.Current Date and Current Time


### To use current date , time and date time when inserting values:

    INSERT INTO employees VALUES( CURRENT_DATE() , CURRENT_TIME() , NOW() );

Note: In this case we Assume that 1st column is DATE , 2nd is TIIME and 3rd is DATETIME. Date is given as "yyyy-mm-dd". Time is given as "hh:mm:ss" . DateTime is given as "yyyy-mm-dd hh:mm:ss" . To reference tommorow or yesterday we would add CURRENT_DATE() + 1 or CURRENT_DATE() - 1 respectively. Similarly for CURRENT_TIME() as well but for the seconds.



## 8.Unique Constraint


### To add the UNIQUE constraint on a column when creating table:

    CREATE TABLE <table_name>(
        <column_name_1> <data_type> UNIQUE,
        <column_name_2> <data_type>
    );

Note: UNIQUE is used when we do not want any duplicate data in a column. 

### To add UNIQUE after table creation:

    ALTER TABLE <table_name> ADD CONSTRAINT UNIQUE(<column_name>);

Note: When you apply UNIQUE to a already UNIQUE column , you create another new key but the column name cannot be same so _2 is add and so on. For example if during creation unique was already added to employee_id a new key will be created in the index called employee_id which is to put the unique key constraint on the column but if you alter table and once again add unique to the same column a new key index generated automatically called employee_id_2. This is redundent.

### To remove the UNQIUE constraint(or any other key applied to a column):

    ALTER TABLE <table_name> DROP INDEX <column_name>;



## 9.Not Null Constraint


### To add the NOT NULL constraint on a column when creating table:

    CREATE TABLE <table_name>(
        <column_name_1> <data_type> NOT NULL,
        <column_name_2> <data_type>
    );

Note: NOT NULL is used when we do not want empty data / null data in a column.

### To add NOT NULL after table creation:

    ALTER TABLE <table_name> MODIFY <column_name> <data_type> NOT NULL;

## To drop NOT NULL constraint:

    ALTER TABLE <table_name> MODIFY <column_name> <data_type> NULL;



## 10.Check Constraint 


### To add CHECK constraint while creating table:

    CREATE TABLE <table_name>(
        <column_name_1> <data_type>,
        <column_name_2> <data_type>
        CONSTRAINT <check_name> CHECK( <column_name_1> <conditional_operation> <data> )
    );

Note: Check is used when we want values of a certain range or a threshold , if not error will be thrown

### To add the CHECK constraint after creating table:

    ALTER TABLE <table_name> ADD CONSTRAINT CHECK( <column_name_1> <conditional_operation> <data> );

### To delete a CHECK constraint:

    ALTER TABLE <table_name> DROP CHECK <check_name>;



## 11.Default Constraint


### To add DEFAULT constraint while creating table:

    CREATE TABLE <table_name>(
        <column_name_1> <data_type> DEFAULT <data>,
        <column_name_2> <data_type>
    )

### To add the DEFAULT constraint after creating table:

    ALTER TABLE <table_name> MODIFY <column_name> <data_type> DEFAULT <data>;   

    ALTER TABLE <table_name> ALTER <coloumn_name> SET DEFAULT <data>; //Check back this shit not working but its from the video

### To remove the DEFAULT constraint:

    ALTER TABLE <table_name> MODIFY <column_name> <data_type> DEFAULT NULL;

    ALTER TABLE <table_name> ALTER <coloumn_name> DROP DEFAULT;



## 12.Primary Key


### To make a column into primary key during table creation:

    CREATE TABLE <table_name> (
        <column_name-1> <data_type> PRIMARY KEY,
        <column_name_2>

    );

### To make a column into primary key after table creation:

    ALTER TABLE <table_name> ADD CONSTRAINT PRIMARY KEY( <column_name> );

### TO delete the primary key constraint from a coloumn:

    ALTER TABLE <table_name> DROP PRIMARY KEY;

Note: The primary key is a combination of the not null and unique constraints . That is , it can be used to get access of each row easily and exists for that purpose and to make joins easier. Also there can be only one primary key per table.



## 13.Auto Increment


### To apply auto increment to a column during table creation:  

    CREATE TABLE <table_name> (
        <column_name> INT PRIMARY KEY AUTO_INCREMENT,
        <column_name>
    );

Note: The AUTO_INCREMENT attribute can be applied to a column that is set as a key and is a numeric data type like INT , BIGINT , SMALLINT , TINYINT and MEDIUMINT. As default it starts from one. 

### To have a specific starting value for auto increment:
    
    ALTER TABLE <table_name> AUTO_INCREMENT = <starting_value>;

Note: Here the the starting value decides the first row's value.

### To apply auto increment to a column after table creation:

    ALTER TABLE <table_name> MODIFY COLUMN <column_name> <data_type> AUTO_INCREMENT , AUTO_INCREMENT = <starting_value>;

Note: This should be a primary key or have unique and numeric data type.

### To delete the auto increment on a column:

    ALTER TABLE <table_name> MODIFY COLUMN <column_name> <data_type>;

Note: If you drop the auto_increment on a column then once again re-apply it without giving proper starting point the auto_increment starts from one back again. For example , if three entries have been entered into the table then the auto_increment starting value was 100 therefore the values present will be 100 , 101 , 102. But if the auto_increment is dropped then once again re-applied without giving starting value and another three new entries have been made then the new values will look like 100 , 101 , 102 , 1 , 2 , 3 .



## 14.Foreign Key 

### To apply foreign key constraint:

    CREATE TABLE <table_name_1>(
        <column_name_1> <data_type> PRIMARY KEY AUTO_INCREMENT, //Primary Key of first table
        <column_name_2> <data_type>
    );

    CREATE TABLE <table_name_2>(
        <column_name_3> <data_type> PRIMARY KEY AUTO_INCREMENT,
        <column_name_4> <data_type>,
        <column_name_5> <data_type>,
        FOREIGN KEY( <column_name_5> ) REFRENCES <table_name_1>( <column_name_1> )
    );

Note: A foreign key is a primary key in one table that is also another column in another table . Using this foreign key we can establish link between two tables. This can be used to reference different columns in different tables. This is also useful because the link between two tables prevents any actions that would destroy the link between them.  Cannot add or update a child row: a foreign key constraint fails , this error will pop up if we put an entry in column_name_5 which is not present in column_name_1. That is if column_name_1 only has 101 , 102 , 103  then putting 111 in column_name_5 will result in an error.BUT you can put NULL as a value. Also you cannot update or delete a parent column whoose values is being used by the child column where parent column is column_name_1 and child column is column_name_5.

### To apply foreign key constraint after table creation:

    ALTER TABLE <table_name_1> ADD CONSTRAINT <foreign_key_name> FOREIGN KEY( <column_name_1> ) REFRENCES 
    <table_name_2>( <column_name_2> );

Note: column_name_1 is a column in table_name_1 and column_name_2 is the PRIMARY KEY in table_name_2. Only column_name_2 needs to be a PRIMARY KEY . column_name_1 need not be unique or not null or any other constraint 

### To drop a foreign key:

    ALTER TABLE <table_name> DROP FOREIGN KEY <foreign_key_name>;



## 15.Joins


### To perform inner join between two tables:

    SELECT * FROM <table_name_1> [INNER] JOIN <table_name_2> ON <table_name_1>.<column_name_1> = <table_name_2>.<column_name_2>;

Note: Imagine a ven diagram , in this case INNER join means only the intersection is shown , that is only the rows/entires that are present in both columns are shown here. Joins are used to connect two tables using a similar column as a link or using a foreign key as a link.

### To perform left join between two tables:

    SELECT * FROM <table_name_1> LEFT JOIN <table_name_2> ON <table_name_1>.<column_name_1> = <table_name_2>.<column_name_2>;

Note: Imagine a ven diagram , in this case LEFT join means all rows of table_name_1 and only the intersecting rows of table_name_2 is shown.

### To perform right join between two tables:

    SELECT * FROM <table_name_1> RIGHT JOIN <table_2> ON <table_name_1>.<column_name_1> = <table_name_2>.<column_name_2>;

Note: Imagine a ven diagram , in this case RIGHT join means all rows of table_name_2 and only the intersecting rows of table_name_1 is shown.

### To display only specific columns during join:

    SELECT <table_name_1>.<column_name_1> , <table_name_1>.<column_name_2> , <table_name_2>.<columns_name_3> , <table_name_2>.<column_name_4>  FROM <table_name_1> JOIN <table_name_2> ON <table_name_1>.<column_name_1> = <table_name_1>.<column_name_10>;

Note: Here column name 1 , 2 are from table_name_1 and column from 3 to lets say 20 is form table name 2 . If I only want to display a certain columns 1,2 from table 1   and 3,4 from table 2 , I will use this query. Also if lets say table 2 with column 18 also has the same column name as column 2 of table 1 . To specify the column and have no amiguity we must use the syntax <table_name_1>.<column_name_2> after the SELECT keyword.

### To perform a "FULL" join: 

    SELECT * FROM <table_name_1> LEFT JOIN <table_name_2> ON <table_name_1>.<column_name_1> = <table_name_2>.<column_name_2> UNION 
    SELECT * FROM <table_name_1> RIGHT JOIN <table_2> ON <table_name_1>.<column_name_1> = <table_name_2>.<column_name_2>;

Note: We use the UNION keyword which is used to combine 2 or more SELECT queries together to perform the full join which uses the left and right join. We will learn more about UNION keyword later.



## 16.Functions


Note: Functions are vast and there are multiple functions for calculating sum , avg or concatinating string etc. Lets see some of the more commonly use functions

### AGGREGATE FUNCTIONS

### To use the COUNT fucntion to display the number of rows for a certain column:

    SELECT COUNT( <coulumn_name_1> ) AS "<alias_name>" FROM <table_name_1>;

Note: This will return a table with the column name of the alias_name and Display one row which is number of NOT NULL rows present in that column . We can use as to give alternate name but if we dont , it will display COUNT(<column_name_1>) as default. If you give union and 2 count select query a , only the first select statements name , that is COUNT(<column_name_1>) will appear even if you give alias for 2nd query after the UNION. Also , if two tables both have same number of count , we know that UNION keyword will delete duplicate columns , hence only 1 size will appear even if you did query count for two tables.

### To use the SUM functions to sum all the values of a column:

    SELECT SUM(<column_name_1>) AS "<alias_name>" FROM <table_name_1> ;

Note: This will return the sum of all the values in the column as a single row with the column name as the alias name . 

### To use the AVG functions to find average of all the values of a column:

    SELECT AVG(<column_name_1>) AS "<alias_name>" FROM <table_name_1> ;

Note: This will return the avg of all the values in the column as a single row with the column name as the alias name . Return 0 for string

### To use the MIN functions to find min of all the values of a column:

    SELECT MIN(<column_name_1>) AS "<alias_name>" FROM <table_name_1> ;

Note: This will return the MIN of all the values in the column as a single row with the column name as the alias name . 

### To use the MAX functions to sum all the values of a column:

    SELECT SUM(<column_name_1>) AS "<alias_name>" FROM <table_name_1> ;

Note: This will return the MAX of all the values in the column as a single row with the column name as the alias name .


### STRING FUNCTIONS


### To use the CONCAT functions to combine two strings:
    
    SELECT CONCAT( <column_name_1> , <column_name_2> ) AS <alias_name> FROM <table_name_1>;

Note: <column_name_2>'s  data is added to the end of  <column_name_1>'s data . We can also use concat to create 

### To use the UPPER functions to capitalize the string:

    SELECT UPPER( <column_name_1> ) AS <alias_name> FROM <table_name_1>;

Note: A new column with <alias_name> will be created which contains the capilatized form of <column_name_1>'s data.

### To use the LOWER functions to lowercase the string:

    SELECT LOWER( <column_name_1> ) AS <alias_name> FROM <table_name_1>;

Note: A new column with <alias_name> will be created which contains the lowercase form of <column_name_1>'s data.

### To use the SUBSTRING function to only get a portion of the string:

    SELECT SUBSTRING (<column_name_1> , start_position , end_position  )  AS <alias_name> FROM <table_name_1>.

Note: A new column with the <alias_name> is created which contains a portion of the string which was in <column_name_1> from starting position given to ending position. Can be used with numeric datatypes and with date and time.

### To use the LENGTH function to get the length of a string:

    SELECT LENGTH ( <column_name_1> ) AS <alias_name> FROM <table_name_1>.

Note: A new column with alias name with rows which display the lengths of each row in the particular <column_name_1>. DATETIME datatype is 19.


### DATE AND TIME FUNCTIONS


### To get the CURRENT DATE , TIME as well as DATE TIME:

    SELECT NOW() { CURDATE() , CURRENT_DATE() , CURTIME() , CURRENT_TIME() } AS <alias_name>;

Note: We can apply this while using insert into for like inserting the current system's date time information . Important when needing to keep track of transactions for example.

### To get the DIFFERENCE in DATE:

    SELECT DATEDDIFF( <date_1> , <date_2> ) AS <alias_name>;

    SELECT DATEDDIFF( <column_name_1> , <column_name_2> ) AS <alias_name> FROM <table_name_1>;

    SELECT DATEDDIFF( CURRDATE(), <column_name_2> ) AS <alias_name> FROM <table_name_1>;

Note: It shows a column with alias name and the difference in dates(how many days) as an integer value.


### To only get DATE from DATETIME column:

    SELECT DATE(<column_name>) AS <alias_name> FROM <table_name>;

Note: This will display a column and rows pertaining to table_name and column_name and return the DATE datatype for the column in the format 'YYYY-MM-DD'

### To only get TIME from DATETIME column:

    SELECT TIME(<column_name>) AS <alias_name> FROM <table_name>;

Note: This will display a column and rows pertaining to table and column and  return the TIME datatype for the column in the format 'HH:MM:SS'

### To get a particular section from DATE / TIME /DATETIME:

    SELECT DATE_FORMAT( <column_name> , '%Y-%m-%d %H:%i:%s %p') AS <alias_name> from <table_name>;

Note: This will display a column with the alias name as column and return STRING datatype. We can apply modifiers to set the date for example if we want to set it to 1st day of the month , the 2nd parameter for DATE_FORMAT() will be '%Y-%m-01'.  The types of modifiers . %a: Abbreviated weekday name (e.g., Sun, Mon, ..., Sat)
- %b: Abbreviated month name (e.g., Jan, Feb, ..., Dec)
- %c: Month number (e.g., 1, 2, ..., 12)
- %d: Day of the month (e.g., 01, 02, ..., 31)
- %D: Day of the month with English suffix (e.g., 1st, 2nd, 3rd, ...)
- %e: Day of the month (e.g., 1, 2, ..., 31)
- %j: Day of the year (e.g., 001, 002, ..., 366)
- %M: Full month name (e.g., January, February, ..., December)
- %m: Month number (e.g., 01, 02, ..., 12)
- %U: Week number (Sunday as the first day of the week, e.g., 00, 01, ..., 53)
- %u: Week number (Monday as the first day of the week, e.g., 00, 01, ..., 53)
- %V: Week number (Sunday as the first day of the week, e.g., 01, 02, ..., 53)
- %v: Week number (Monday as the first day of the week, e.g., 01, 02, ..., 53)
- %W: Full weekday name (e.g., Sunday, Monday, ..., Saturday)
- %w: Day of the week (e.g., 0 = Sunday, 1 = Monday, ..., 6 = Saturday)
- %X: Year for the week where Sunday is the first day of the week (e.g., 2023)
- %x: Year for the week where Monday is the first day of the week (e.g., 2023)
- %Y: Four-digit year (e.g., 2023)
- %y: Two-digit year (e.g., 23)

Time Specifiers:

- %f: Microseconds (e.g., 000000, 001000, ..., 999000)
- %H: Hour (24-hour format, e.g., 00, 01, ..., 23)
- %h: Hour (12-hour format, e.g., 01, 02, ..., 12)
- %I: Hour (12-hour format, e.g., 01, 02, ..., 12)
- %i: Minutes (e.g., 00, 01, ..., 59)
- %k: Hour (24-hour format, e.g., 0, 1, ..., 23)
- %l: Hour (12-hour format, e.g., 1, 2, ..., 12)
- %p: AM/PM (e.g., AM, PM)
- %r: Time in 12-hour format (e.g., 01:00:00 AM, 12:00:00 PM)
- %S: Seconds (e.g., 00, 01, ..., 59)
- %s: Seconds (e.g., 00, 01, ..., 59)
- %T: Time in 24-hour format (e.g., 00:00:00, 23:59:59)

You can combine these specifiers to create custom date and time formats. When comparing different columns make sure they maintain the same common format if not it may not work as intended.

### To get the YEAR/MONTH/DAY/QUATER from the date:

    SELECT YEAR(<column_name>) AS <alias_name> FROM <table_name>;

    SELECT MONTH(<column_name>) AS <alias_name> FROM <table_name>;

    SELECT DAY(<column_name>) AS <alias_name> FROM <table_name>;

    SELECT QUARTER(<column_name>) AS <alias_name> FROM <table_name>;

Note: These will return YEAR , MONTH , DAY and QUARTER as a numeric datatype. 

### To get the start of the QUARTER from date:

    SELECT MAKEDATE(YEAR( <column_name> ), 1) + INTERVAL (QUARTER( <column_name> ) - 1) QUARTER AS <alias_name> FROM <table_name>;

Note: When you use MAKEDATE(YEAR(your_date), 1), it returns the first day of the year for the given date.For example, if your_date is 2024-08-15, MAKEDATE(YEAR(your_date), 1) would return 2024-01-01.

INTERVAL is a keyword in MySQL that allows you to perform date and time arithmetic.When you use INTERVAL, you specify a value and a unit, such as:
- INTERVAL 1 DAY (add 1 day)
- INTERVAL 3 MONTH (add 3 months)
- INTERVAL 2 QUARTER (add 2 quarters)
- INTERVAL 1 YEAR (add 1 year)
You can use INTERVAL with the + or - operators to add or subtract a specified interval from a date.
For example:
- your_date + INTERVAL 1 DAY adds 1 day to your_date
- your_date - INTERVAL 1 MONTH subtracts 1 month from your_date

QUARTER(date) is a MySQL function that returns the quarter of the year for a given date.
The quarter is represented as a number from 1 to 4, where:
- Q1 (Quarter 1): January 1st -> January 1 - March 31 (returns 1) 
- Q2 (Quarter 2): April 1st -> April 1 - June 30 (returns 2)
- Q3 (Quarter 3): July 1st -> July 1 - September 30 (returns 3)
- Q4 (Quarter 4): October 1st -> October 1 - December 31 (returns 4)


### Logical and Control Flow Functions


### To use the CASE WHEN when dealing with multiple branching paths:

    SELECT 
    CASE
        WHEN <column_name> <condition_1> THEN <result_1>
        WHEN <column_name> <condition_2> THEN <result_2>
        ELSE <default_result> 
    END AS <alias_name>
    FROM <table_name>;

Note: Query given in multiline because it is better coding convention /  cleaner coding / easier to understand .  CASE WHEN is Similar to case switch in other programming languages where by if <condition_1> is true then the displayed row will have the <result_1> . If the following conditions also gives true then that result will overwrite the previous result. Pretty similar to if else I dont know what else to say just learn the syntax.

### To use COALESCE to find the first NON NULL value:

    SELECT COALESCE( <column_name_1> , <column_name_2> , <column_name_3>) AS <alias_name> FROM <table_name>; 

Note: Starting from <column_name_1> then <column_name_2> then finally <column_name_3> it will return the first NON NULL value , for example if a row has NULL for both <column_name_1> and <column_name_2> but value for <column_name_3> then the value at <column_name_3> will be returned . But if some other row has value at <column_name_1> itself then that value will be returned. The first parameter/column entered in the COALESCE has highest priority and last has lowest priority. Also if string or other high precedence datatype is first parameter then few problems will arise but if COALESCE(INT , INT VARCHAR) is given then the database will try converting the VARCHAR to INT which will result in error therefore it might be wise to first typecast it to CHAR. To do that use the Syntax CAST( <column_name> AS CHAR).   

### To use NULLIF to set a row to NULL if two variables are equal:

    SELECT NULLIF( <column_name_1> , <value> ) AS <alias_name> FROM <table_name>;

Note: It will set the row as NULL when displaying the column with <alias_name>. If it is not equal then the value of the row from <column_name_1> will be displayed.


### Window Functions


### To use ROW_NUMBER to assign numbers/ranks to rows in a window:

    SELECT ROW_NUMBER() OVER( ORDER BY <column_name_1> ASC/DESC ) AS <alias_name> FROM <table_name>; //consider entire table's rows as 1 window

    SELECT ROW_NUMBER() OVER( PARTITION BY <column_name_2> ORDER BY <column_name_1> ASC/DESC ) AS <alias_name> FROM <table_name>; //seperate the table based on <column_name_2> then apply function to each seperate window.

Note: This makes it display another column where it is numbering each row from top to bottom. In any window function there are two parts. The OVER( ORDER BY <column_name> ASC/DESC ) is the window and ROW_NUMBER() is the function. The ROW_NUMBER() function will always count from 1 to end of the window regardless of tie.We can use partition to seperate the table into multiple different windows . For example , lets say there are multiple departments like ECE,CSE , EEE etc. Then , partitioning based on these departments will result in a window for each department arranged lexiographically and inside each window there will be numbering from 1 to end of the window.

### To use RANK to assign numbers/ranks to rows in a window:

    SELECT RANK() OVER( ORDER BY <column_name_1> ASC/DESC ) AS <alias_name> FROM <table_name>; //consider entire table's rows as a single window

    SELECT RANK() OVER( PARTITION BY <column_name_2> ORDER BY <column_name_1> ASC/DESC ) AS <alias_name> FROM <table_name>; //seperate the table based on <column_name_1> then apply function to each seperate window.

Note: Similar to ROW_NUMBER() but , when there is a tie , the rows with tie are given the same rank and the next ranked is skipped corresponding to the number of rows with ties. For example , if 5th and 6th row have same <column_name_1> value and are tied , then both will have the same rank as 5 in <alias_name> column but the 7th row will have rank as 7. Remember Positionally.

### To use DENSE_RANK to assign numbers/ranks to rows in a window:

    SELECT RANK() OVER( ORDER BY <column_name_1> ASC/DESC ) AS <alias_name> FROM <table_name>; //consider entire table's rows as a single window

    SELECT RANK() OVER( PARTITION BY <column_name_2> ORDER BY <column_name_1> ASC/DESC ) AS <alias_name> FROM <table_name>; //seperate the table based on <column_name_1> then apply function to each seperate window.

Note: Similar to RANK() but , the next number after tie will continue the ranking serially instead of skipping numbers due to the tied rows. For example , if 5th and 6th row have same <column_name_1> value and are tied , then both will have the same rank as 5 in <alias_name> column but the 7th row will have rank as 6. Remember Numerically.

### To get the realtive ranking using PERCENT RANK:

    SELECT *, PERCENT_RANK() OVER(PARTITION BY <column_name_2> ORDER BY <column_name_1> ASC/DESC ) AS <alias_name> FROM <table_name>;

Note: Similar to the RANK function as internaly it uses the rank function. Basically the ranking go from 0 to 1 in decimal/floating point fashion kind of like percentages. For example , lets say the values in a particular window are 10 20 30 30 40 50 in ascending order . Then the column with <alias_name> will have the values as 0 0.2 0.4 0.4 0.8 1 . This is because the value 30 repeat , and rank is given to 30 as 0.4 twice then since it follows how RANK worked , that is positionally assigning values , it will now skip 0.6 and go to 0.8 directly when it reaches 40. If number of rows in the window = n , then the ranking go from 0/n-1 =0 to n-1/n-1 = 1. Formula = current row -1/total n -1;

### To get the cumilative distance using CUME_DIST:

    SELECT *, CUME_DIST() OVER(PARTITION BY <column_name_2> ORDER BY <column_name_1> ASC/DESC ) AS <alias_name> FROM <table_name>;

Note: Similar to Percentage but instead of 0 to 1 now it is 1/number of rows in window to 1. It is simialr to how PERCENT RANK deals with duplicate but different in a way that PERCENT RANK takes the rank of the row's value and skip next rank but CUME_DISTANCE compares what are all the values lower or equal to current row's value and therefor inevetibly skips previous ranks/decimals/fractions. Formula number of rows lesser or equal to current row / total nnumber of rows in the window.

Difference between CUME_DIST and PERCENTAGE_RANK calculations:

| Employee | Salary | CUME_DIST  | PERCENT_RANK        |
|----------|--------|------------|---------------------|
| John     | 50000  | 1/6 = 0.17 | 1-1/6-1 = 0/5 = 0   |
| Jane     | 60000  | 3/6 = 0.5  | 2-1/6-1 = 1/5 = 0.2 |
| Joe      | 60000  | 3/6 = 0.5  | 2-1/6-1 = 1/5 = 0.2 |
| Sarah    | 70000  | 5/6 = 0.83 | 3-1/6-1 = 2/5 = 0.4 |
| Mike     | 70000  | 5/6 = 0.83 | 3-1/6-1 = 2/5 = 0.4 |
| Emma     | 80000  | 6/6 = 1    | 5-1/6-1 = 4/5 = 0.8 |
| David    | 80000  | 6/6 = 1    | 5-1/6-1 = 4/5 = 0.8 |

This table illustrates how CUME_DIST and PERCENT_RANK handle duplicates: CUME_DIST assigns the same cumulative distribution value to duplicates, and PERCENT_RANK assigns the same percentage rank value to duplicates.

### To divide the window into buckets using NTILE:

    SELECT SELECT *, NTILE(N) OVER(PARTITION BY <column_name_2> ORDER BY <column_name_1> ASC/DESC ) AS <alias_name> FROM <table_name>;

Note: This will divide the window into N number of buckets which are each allocated a bucket number. For example , let us say the current partition/window has 8 rows. Then if N = 2 then the column <alias_name> will have the values 1 1 2 2 3 3 4 4 . Simimalrly , if N=3 and obviously there is a remmainder of 2 , the remainder will be spread out from the first bucket. Therefore now the values of the <alais_name> column will be 1 1 1 2 2 2 3 3.

### To use AVG to get the average value of windows:

    SELECT * , AVG(<column_name_1>) OVER(PARTITION BY <column_name_2> ) AS <alias_name> FROM <table_name>; //Total is static 

    SELECT * , AVG(<column_name_1>) OVER(PARTITION BY <column_name_2> ORDER BY <column_name_1> ) AS <alias_name> FROM <table_name>; //Rolling total , increases as it goes down each row

Note: This will display all the columns and an extra column called <alias_name> where each window's average will be display for each row in that particular window. The first query considers the entire window's total (everything in each partition) . That is lets say we have the values 2 4 6 for a window in that order . Then the <alias_name> column will display the values 4 4 4. But for the second query , it is rolling total concept meaning the first value is consider the total initally and then as it goes down the row , the value is added to the total so for the same example of 2 4 6 , now the <alias_name> column will have the values 2 3 4 , because 2/1 = 2 , 2+4/2 = 3 and 2+4+6/3 = 4.

### To use SUM to get the total value of windows:

    SELECT * , SUM(<column_name_1>) OVER(PARTITION BY <column_name_2> ) AS <alias_name> FROM <table_name>; //Total is static 

    SELECT * , SUM(<column_name_1>) OVER(PARTITION BY <column_name_2> ORDER BY <column_name_1> ) AS <alias_name> FROM <table_name>; //Rolling total , increases as it goes down each row

Note: This will display all the columns and an extra column called <alias_name> where each window's total sum will be display for each row in that particular window. The first query considers the entire window's total (everything in each partition) . That is lets say we have the values 2 4 6 for a window in that order . Then the <alias_name> column will display the values 12 12 12. But for the second query , it is rolling total concept meaning the first value is consider the total initally and then as it goes down the row , the value is added to the total so for the same example of 2 4 6 , now the <alias_name> column will have the values 2 6 12 , because 2 = 2 , 2+4 = 6 and 2+4+6 = 12.

### To get the FIRST           VALUE from a window:

    SELECT * , FIRST_VALUE(<column_name_1>) OVER(PARTITION BY <column_name_2>) AS <alias_name> FROM <table_name>;

    SELECT * , FIRST_VALUE(<column_name_1>) OVER(PARTITION BY <column_name_2> ORDER BY <column_name_1> ASC/DESC ) AS <alias_name> FROM <table_name>;

Note: This will display all columns plus the column with <alias_name> which will contain the FIRST_VALUE of <column_name_1> in that window as the row values for each window . For example if in a window there is 100 150 200 and we gave ASC or nothing (ASC by default) in ORDER BY then 100 100 100 will be displayed for those rows in the column <alias_name> for that window.

### To get the NTH VALUE from a window:

    SELECT * , NTH_VALUE(<column_name_1>,N) OVER(PARTITION BY <column_name_2>) AS <alias_name> FROM <table_name>;

    SELECT * , NTH_VALUE(<column_name_1>,N) OVER(PARTITION BY <column_name_2> ORDER BY <column_name_1> ASC/DESC ) AS <alias_name> FROM <table_name>;

Note: This will display all columns plus the column with <alias_name> which will contain the NTH_VALUE of <column_name_1> in that window as the row values for each window . The first Query will have access to total window but second queryy uses rolling total concept. For example if in a window there is 100 150 200 and we gave ASC or nothing (ASC by default) in ORDER BY and put N  = 2 then 150 150 150 will be displayed for those rows in the column <alias_name> for that window. But in the case of the second query when we give N=2 we will get NULL 150 150 . This is because of the rolling total concept that , when we are at the first value of the window and haven't seen N=2 yet because it isn't part of the total yet , we don't know N=2 and hence have to put NULL , but from second row that it N=2 onwards we now have access to N=2 and hence can put the value as 150 . If N=3 , then the rows would be NULL NULL 200.

### To get the previous values of after a particular row using LAG:

    SELECT *, LAG( <column_name_1> , <offset_value> , <default_value> ) OVER (PARTITION BY <column_name_2> ORDER BY <column_name_1> ASC/DESC) AS <alias_name> FROM <table_name>;

Note: This will create a column <alias_name> which will contain the previous value of <column_name_1>. For example , if the <column_name_1> has values 1 2 3 , the <alias_name> column will have the rows NULL 1 2. The default <offset_value> is 1 and <default_value> is NULL . Why it has NULL because there is no row before first row of the window , therefore it will be NULL. We can edit this by changing the <default_value> which is the third parameter of the LAG function.

### To get the subsequent values of after a particular row using LEAD:

    SELECT *, LEAD( <column_name_1> , <offset_value> , <default_value> ) OVER (PARTITION BY <column_name_2> ORDER BY <column_name_1> ASC/DESC) AS <alias_name> FROM <table_name>;

Note: This will create a column <alias_name> which will contain the next value of <column_name_1>. For example , if the <column_name_1> has values 1 2 3 , the <alias_name> column will have the rows 2 3 NULL. The default <offset_value> is 1 and <default_value> is NULL . Why it has NULL because there is no row after last row of the window , therefore it will be NULL. We can edit this by changing the <default_value> which is the third parameter of the LEAD function.

### Other Important Functions:

### To ROUND off a value using ROUND fucntion:

    SELECT ROUND(<column_name> ,  <decimal_point> ) AS <alias_name> FROM <table_name>;

Note: The value of the 2nd parameter will dictate till how many values after the decimal point the displayed column will round of to. That is , if it is 0 , then round of to 0 decimal place meaning no decimal displayed and value at 1st decimal place taken to round up / round down the value at ones place.

### To remove leading and trailing whitespaces by using TRIM:

    SELECT TRIM( <column_name> ) AS <alias_name> FROM <table_name>;

Note: This will return a string without whitespaces at the start or end of the string but will not remove whitespaces in the middle of the string.


## 17.Logical Operators


### To use AND to combine many conditions:

    SELECT * FROM <table_name> WHERE <column_name_1> <condition_1> AND <column_name_2> <condition_2> AND <column_name_3> <condition_3>;

Note: Only when all conditions are satisfied , then only the row will be displayed/selected.

### To use OR to combine many conditions:

    SELECT * FROM <table_name> WHERE <column_name_1> <condition_1> OR <column_name_2> <condition_2> OR <column_name_3> <condition_3>;

Note: Even if one condition is satisfied , then the row will be displayed/selected. 

### To use NOT to get inverse of a condition and also use it in tandem with other logical operator:

    SELECT * FROM <table_name> WHERE NOT <column_name_1> <condition_1> OR NOT <column_name_2> <condition_2> AND NOT <column_name_3> <condition_3>;

Note: If condition one is not satified then it will return true , similarly for <condition_2> and <condition_3> . Then the query will take into consideration the other logical operators. For example , lets assume the conditions return as follows NOT FALSE OR NOT TRUE AND NOT FALSE. Then after implementing the NOT operator , we will have TRUE OR FALSE AND TRUE. In that case TRUE OR FALSE AND TRUE-> TRUE AND TRUE-> TRUE will be the final veridict for the row and the row will be displayed/selected.

### To use the BETWEEN operator to get the values in a range:

    SELECT * FROM <table_name> WHERE <column_name_1> BETWEEN <value_1> AND <value_2>;

Note: This will give all the rows where the <column_name_1> as the value between <value_1> and <value_2> . Both <value_1> and <value_2> will be included also. It is very similar to the AND logical operator but with better readability.

### To use the IN operator to see if a row's value is inside a set of values:

    SELECT * FROM <table_name> WHERE <column_name_1> IN ( <value_1> , <value_2> , <value_3> );

Note: This query return the rows in which the <column_name_1> 's values are in the set ( <value_1> , <value_2> , <value_3> ) . If yes the entire row is printed , if no then the row isnt displayed.


## 18.Wildcards

### To use the % operator to represent any number of characters in a string

    SELECT * FROM <table_name> WHERE <column_name_1> LIKE "E%"; //This means the starting letter is E for that particular string in that column. 

    SELECT * FROM <table_name> WHERE <column_name_1> LIKE "%E"; //This means the ending letter is E for that particular string in that column.  

    SELECT * FROM <table_name> WHERE <column_name_1> LIKE "A%E"; //This means the ending letter is E and Starting letter is A for that particular string in that column. 

Note: Basically the % symbol acts as substitute any number of letters. You can use this in other datatypes like date too.

### To use the _ operator to represent any number of characters in a string

    SELECT * FROM <table_name> WHERE <column_name_1> LIKE "E_"; //This means there is only 2 element and starting element is E for that particular string in that column. 

    SELECT * FROM <table_name> WHERE <column_name_1> LIKE "_E"; //This means the ending letter is E for that particular string in that column. The column size it self is one.

    SELECT * FROM <table_name> WHERE <column_name_1> LIKE "A_E"; //This means the ending letter is E there are more and Starting letter is A for that particular string in that column. We can by operoinf

Note: The _ symbol acts as substitute for a single letters. We can mix and match % and _ to get the desired rows we want.


## 19.Order By Clause


### To arrange the column in ascending / descending order:

    SELECT * FROM <table_name> ORDER BY <column_name_1> ASC / DESC ;

Note: If it is a string it will use lexiographical arrangement. For numbers , dates etc. the arrangement will consider bigger number and later dates as higher values which will be at the top during (descending)DESC ordering and at the  bottom during (ascending)ASC ordering .


### To ORDER BY another column in the case of two rows having the same values:

    SELECT * FROM <table_name> ORDER BY <column_name_1> , <column_name_2> , <column_name_3>;

Note: The rows will be order based on <column_name_1> first then when the values in <column_name_1> are the same then it will check the values in <column_name_2> and order the similar values of <column_name_1> using that . Similarly , when there are repeating row values for both <column_name_1> and <column_name_2> then it will check for <column_name_3> and order it based off of that.


## 20.Limit Clause


### To LIMIT the number of records/rows displayed:

    SELECT * FROM <table_name> LIMIT <value>;

    SELECT * FROM <table_name> ORDER BY <column_name_1> LIMIT <value>;

    SELECT * FROM <table_name> LIMIT <offset> , <value>;

    SELECT * FROM <table_name> ORDER BY <column_name_1> LIMIT <offset> , <value>;

Note: The <value> entered here will be the number of records/rows displayed. When no ORDER BY is given it will start from the top of the table and move downwards as the <value> increases. It is very useful when used in conjunction with the ORDER BY clause. The <offset> states how many rows from the top of the table should be skipped . If it was 1 then the top row of the table is skipped , if it is 2 then the top 2 rows of the table is skipped. An example usecase of <offset> would be like displaying certain amount of info in one page and uniformly distributed size through the list. Like page 1 will have <offset> = 0 and <value> = 50. Page 2 will be <offset> = 50 and <value> = 50.
Page 3 will be <offset> = 100 and <value> = 50 and so on.


## 21.Unions


### To combine two or more tables using UNION:

    SELECT * FROM <table_name_1> UNION SELECT * FROM <table_name_2>;

Note: This will display the <table_name_1> with its column names then below that <table_name_2> rows will be there probably with mismatches column name because the top will have column name of <table_name_1>. UNION can only be used when there are same number of columns. So for that instead of * we should give the specific columns. Also the UNION operator removes duplicate rows in the tables , that is if both tables have the same row values , one of the rows will not be present when we display it.


### To display even the duplicate rows using UNION ALL:

    SELECT * FROM <table_name_1> UNION ALL SELECT * FROM <table_name_2>;


## 22.Self Joins


### To use SELF JOIN by combining the table with itself:

    SELECT * FROM <table_name_1> AS <alias_name_1> INNER JOIN <table_name_1> AS <alias_name_2> ON <alias_name_1>.<column_name_1> = <alias_name_2>.<column_name_2>;

Note: A Self join is the act of joining a copy of a tale to itself , used to help compare rows of the same table and display hierarchy of data. We have one table but we assign two aliases to the same table to access them differently , like we have two different columns we are comparing where each column correspond to different aliases. For example a query could be , SELECT * FROM employee AS a INNNER JOIN employee AS b ON a.employee_id  = b.referal_id .

### To display only specific columns:

    SELECT <alias_name_1>.<column_name_1> , <alias_name_1>.<column_name_2> , <alias_name_1>.<column_name_3> , 
           <alias_name_2>.<column_name_2> , <alias_name_2>.<column_name_3>
    FROM <table_name_1> AS <alias_name_1> INNER JOIN <table_name_1> AS <alias_name_2> ON <alias_name_1>.<column_name_1> = <alias_name_2>.<column_name_2>;


## 23.Views


### To create a VIEW using all the columns from a specific table:
    
    CREATE VIEW <view_name> AS 
    SELECT * FROM <table_name>; // whatever the result of this 2nd line of the query is will be stores as the view  

Note: This creates a view which is similar to a table .It is called a virtual table. It is the result set of the above SQL statement . The field are from one or more real columns from other tables. They are no real table but can be interacted with as if they are. Useful when you want to have a copy of columns from other tables as when you edit the original columns of the real tables , these are reflected in the view.

### To create a VIEW using speicific columns of the real table:

    CREATE VIEW <view_name> AS
    SELECT <column_name_1> , <column_name_2> FROM <table_name_1>;

### To create a VIEW from joins:

    CREATE VIEW <view_name> AS
    SELECT <alias_name_1><column_name_1> ,<alias_name_1><column_name_2> , <alias_name_2><column_name_3> , <alias_name_2><column_name_4> 
    FROM <table_name_1> AS <alias_name_1> INNER JOIN <table_name_2> AS <alias_name_2> ON <alias_name_1>.<column_name_1> = <alias_name_2>.<column_name_6>;

Note : The INNER JOIN of the 2 tables are then saved as a VIEW.

### To delete a VIEW:

    DROP VIEW <view_name>;

### To display a VIEW:

    SELECT * FROM <view_name>;

### To use ORDER BY on a VIEW:

    SELECT * FROM <view_name> ORDER BY <column_name_1>;


## 24.Indexes


### To display the INDEXES in a table:

    SHOW INDEXES FROM <table_name>;

Note: Indexes are a sort of data structure similar to B-Tree(balanced tree) . They are used to find values in a specific column more quickly . MySQL normally searches sequentially through a column meaning the longer the column , the more expensive the operation is. If you apply an index to a column , UPDATING takes more time , SELECTING takes lesser time. Use it on table which won't be updated very often.

### To create a INDEX for a column:

    CREATE INDEX <index_name> ON <table_name>(<column_name>);

Note: Now any query that searches /filters based on that <column_name> will be much faster. It is mainly noticable only in large datasets .

### To create a MULTI COLUMN INDEX for mulltiple columns of a table:

    CREATE INDEX <index_name> ON <table_name>( <column_name_1> , <column_name_2> , <column_name_3> );

Note: Now this one index will be applied for multiple column but <column_name_1> will have sequence 1 , <column_name_2> will have sequence 2  , <column_name_3> will have sequence 3 in that index. Meaning search for the one with sequence 1 , if sequence 2 exist search for that too similar case for sequence 3. But if you were to just use the 2nd or 3rd squence ( only <column_name_2> or only <column_name_3>  ) in query to filter / display /select or search , we WILL NOT BE UTILISING THE INDEX. The previous sequence MUST be used before the next sequence is used so has to effieciently use the INDEX for faster querying 

### To delete an INDEX:

    ALTER TABLE <table_name> DROP INDEX <index_name>;


## 25.Subquery 


### To display a subquery:

    SELECT * , (SELECT AVG(<column_name>) FROM <table_name>) AS <alias_name> FROM <table_name>; // displays all columns as well as the avg of <column_name_1> 

Note: A subquery is a query inside a quesry. We can use anything from avg to even order by to return a list of rows , like litreally anything . I just feel unimaginative so I put avg here. Also that alias name can only be given in tandem with FROM , cant use it for SET or WHERE clause.

### To use a Subquery with WHERE clause:

    SELECT* FRO

    SELECT * FROM <table_name> WHERE <column_name_1> > (SELECT AVG(<column_name_1>) FROM <table_name>); // returns all the columns where <column_name_1> has greater value then the average.

Note: Here you cannot assign alias to the Subquery nor can you use the <alias_name> instead of the subquery.


## 26.Group By


### To consider values of certain column as a GROUP and use an AGGREGATE FUNCTION on them:

    SELECT  <column_name_1> , <aggregate_function>(<column_name_2>)  FROM <table_name> GROUP BY <column_name_1>;

Note: In the table , the rows with same <column_name_1> are group and the aggregate function is applied on the <column_name_2> of these groups that are formed.
For GROUP BY to work we can only select functionaly dependent column on the GROUP BY . That is 1. A column that has the groups , 2. A column that uses Aggregate functions on another column. If not this error will come :

` Error Code: 1055. Expression #1 of SELECT list is not in GROUP BY clause and contains nonaggregated column 'mydb.employees.employee_id' which is not functionally dependent on columns in GROUP BY clause; this is incompatible with sql_mode=only_full_group_by ` . 

RECAP: AGGREGATE FUNCTION ARE -> MAX() MIN() SUM() AVG() COUNT() etc. Cannot use WHERE clause after GROUP BY , thats why we use HAVING clause. Have to Follow execution order .

###    SQL Query Execution Flow

| Order | Clause            | What it does                         | Example snippet                            |
| ----: | ----------------- | ------------------------------------ | ------------------------------------------ |
|     1 | `FROM` / `JOIN`   | Choose tables and join them          | `FROM employees e JOIN departments d ON …` |
|     2 | `WHERE`           | Filter **rows** (no aggregates here) | `WHERE e.salary > 5000`                    |
|     3 | `GROUP BY`        | Group remaining rows                 | `GROUP BY d.name`                          |
|     4 | `HAVING`          | Filter **groups** (uses aggregates)  | `HAVING COUNT(*) >= 2`                     |
|     5 | `SELECT`          | Pick columns/expressions to output   | `SELECT d.name, COUNT(*) AS emp_count`     |
|     6 | `DISTINCT` (opt.) | Remove duplicate result rows         | `SELECT DISTINCT d.name`                   |
|     7 | `ORDER BY`        | Sort final result set                | `ORDER BY d.name`                          |
|     8 | `LIMIT/OFFSET`    | Return only a subset of rows         | `LIMIT 10 OFFSET 20`                       |

### FULL QUERY EXAMPLE
    SELECT department, COUNT(*) AS emp_count
    FROM employees
    WHERE salary > 5000
    GROUP BY department
    HAVING COUNT(*) >= 2
    ORDER BY department;

This query returns a list of departments that have at least two employees earning more than 5000, along with the count of such employees in each department, sorted by department name.

### To use the HAVING CLAUSE in tandem with GROUP BY so that we can apply condition to groups:

    SELECT <column_name_1> , <aggregate_function>( <column_name-2> ) FROM <table_name> GROUP BY <column_name_1> 
    HAVING <column_name_3> <condition> <value>;

    SELECT <column_name_1> , <aggregate_function>( <column_name-2> ) FROM <table_name> GROUP BY <column_name_1> 
    HAVING <column_name_3> <condition_1> <value_1> AND <column_name_4> <condition_2> <value_2>;

    SELECT <column_name_1> , <aggregate_function>( <column_name-2> ) FROM <table_name> GROUP BY <column_name_1> 
    HAVING <column_name_3> <condition_1> <value_1> OR <column_name_4> <condition_2> <value_2>;

Note: This will only return the rows which satisfies the HAVING CLAUSE CONDITION. Basically when you want to use a WHERE CLAUSE in a GROUP BY , use HAVING CLAUSE.







