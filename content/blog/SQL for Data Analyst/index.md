# SQL for Data Analysts

# What is SQL

SQL (pronounced S.Q.L. or Sequel, no one really cares) stands for Structured Querying Language and is a language that is used to query, form, and manipulate databases. 

# Setting up the Environment

## Files to Download

[SQL Course Public Downloads Folder - Google Drive](https://drive.google.com/drive/folders/1t1l7mcnhxN0rvYH-WTSSSuwocyTujXAl?usp=sharing)

## Installation

For this tutorial we're going to be using SQLite which is a database engine that works in a single application. In order to access it we'll be using DBeaver. DBeaver is an open-source database manager that connects to almost all database engines. 

**NOTE: Use DBeaver Community, it's free**

[DBeaver](https://dbeaver.io/)

### Dataset

The dataset we'll be using is from the University of California Irvine's Machine Learning Repository

[UCI Machine Learning Repository](https://archive.ics.uci.edu/dataset/603/in+vehicle+coupon+recommendation)

## Structure of an SQL environment

### Servers

The computer that contains your database or databases. This is typically your highest level of "container" for databases. 

### Databases

A database is a collection of Schemas and Tables that we can interact with. 

### Schemas

Schemas are the highest level of organization in many Databases and can contain multiple tables. 

### Tables

Tables can be thought of in the same way you think about sheets in Excel. It's a collection of columns and rows that we'll be interacting with. 

### Different Database Types

I'll be talking about SQL like it's a standardized language, and although it technically is, in practice, the SQL databases you'll most likely be interacting with are going to be created and run by major corporations that will sprinkle their own flavor of SQL on top of or in lieu of standard SQL. The commands on which we'll be focusing in this course are pretty standard and shouldn't be drastically different from those you'll be using in most other databases. Some of the biggest databases are listed below:

MySQL: Open Source, Current Development by Oracle

SQL Server created by Microsoft

PostgreSQL: Open Source

Oracle Database created by Oracle

### SQL Data Types

Depending on the database type that you use, the data types that you'll be working with will be different. Here are the basic types and what they might be referred to as in your database: 

**Text types: Used to store text and can even store numbers as text**

- CHAR
- VARCHAR
- TEXT

**Numerical Types: Used to store numbers**

- INTEGER
- FLOAT
- REAL
- NUMERIC
- BOOL
- BOOLEAN

**Date Types: Used to store dates and times**

**Binary Types: Large files stores using "1's and 0's", hence binary**

- TINYBLOB
- BLOB

# Basic Querying

After opening DBeaver and downloading the database file from the link mentioned at the top of the page, create a new connection and select the option for SQLite. 

![](Untitled.png)

Open up the database file you downloaded

![](Untitled%201.png)

If you're using DBeaver, then right click our database and Open a SQL script

![](Screen_Shot_2021-05-19_at_11.38.19_PM.png)

## Obtaining Data

### SELECT

The most basic query, this is used to pull data from a database. 

```sql
SELECT * 
FROM dataset_1;
```

You can run this query by pasting the text into the SQL Editor and clicking on this play button

![](Untitled%202.png)

In the above statement you have a couple of parts:

`SELECT *` this is the command

`FROM` this tells the Database where to pull data from

`dataset_1` this is the table you're pulling data from

`;` the semicolon at the end is how you end an SQL statement. Some databases require it, others don't so I recommend always using one. 

The above command selects all columns in the dataset, you can also specify columns like so:

```sql
SELECT weather, temperature
FROM dataset_1;
```

This will only select the `weather` and `temperature` columns from the dataset. 

### LIMIT

Can be used to limit the amount of data that is pulled into a query, very useful when first exploring the data. 

**Query:**

```sql
select * 
FROM dataset_1 LIMIT 10;
```

**Result:**

![](Screen_Shot_2021-05-18_at_9.20.12_PM.png)

### DISTINCT

Gets the unique values from a column

**Query:**

```sql
SELECT DISTINCT passanger 
FROM dataset_1;
```

**Result:**

![](Screen_Shot_2021-05-18_at_9.25.43_PM.png)

### WHERE

**Query:**

```sql
SELECT * 
FROM dataset_1
WHERE destination = 'Home';
```

**Result:**

![](Untitled%203.png)

### ORDER BY

**Query:**

```sql
SELECT * 
FROM dataset_1
ORDER BY coupon;
```

**Result:**

![](Screen_Shot_2021-05-18_at_9.34.21_PM.png)

### Aliasing

**Query:**

```sql
SELECT destination as 'Destination' 
FROM dataset_1;
```

**Result:**

![](Screen_Shot_2021-05-18_at_9.36.30_PM.png)

### Comments

**Query:**

```sql
SELECT * -- This is a comment
FROM dataset_1;
```

**Result:**

![](Screen_Shot_2021-05-18_at_9.39.26_PM.png)

## Aggregations

### GROUP BY

**Query:**

```sql
SELECT occupation
FROM dataset_1
GROUP BY occupation;
```

**Result:** 

![](Screen_Shot_2021-05-18_at_9.53.04_PM.png)

### AVG

**Query:**

```sql
SELECT weather, 
AVG(temperature) AS 'avg_temp'
FROM dataset_1
GROUP BY weather;
```

**Result:**

![](Screen_Shot_2021-05-18_at_9.52.17_PM.png)

### COUNT

**Query:** 

```sql
SELECT weather, 
COUNT(temperature) AS 'count_temp'
FROM dataset_1
GROUP BY weather;
```

**Result:**

![](Screen_Shot_2021-05-18_at_11.22.45_PM.png)

### COUNT (DISTINCT)

**Query:**

```sql
SELECT weather, 
COUNT(DISTINCT temperature) AS 'count_distinct_temp'
FROM dataset_1
GROUP BY weather;
```

**Result:**

![](Screen_Shot_2021-05-18_at_11.30.24_PM.png)

### SUM

**Query:** 

```sql
SELECT weather, 
SUM(temperature) AS 'sum_temp'
FROM dataset_1
GROUP BY weather;
```

**Result:**

![](Screen_Shot_2021-05-18_at_11.26.11_PM.png)

### MIN

**Query:**

```sql
SELECT weather, 
MIN(temperature) AS 'min_temp'
FROM dataset_1
GROUP BY weather;
```

**Result:**

![](Screen_Shot_2021-05-18_at_11.28.32_PM.png)

### MAX

**Query:**

```sql
SELECT weather, 
MAX(temperature) AS 'max_temp'
FROM dataset_1
GROUP BY weather;
```

**Result:**

![](Screen_Shot_2021-05-18_at_11.27.59_PM.png)

### HAVING

**Query:**

```sql
SELECT occupation
FROM dataset_1
GROUP BY occupation
HAVING occupation = 'Student';
```

**Result:**

![](Screen_Shot_2021-05-18_at_9.55.09_PM.png)

## Combining Data

### Joins and Unions

Oftentimes we'll want to enrich our data by adding more data from other datasets to it. There are two major ways we can combine our datasets together: (The following visualizations will be coming from my FREE Tableau Course)

**Unions**

Put simply, a union (SQL Union) is the process of stacking two tables on top of one another. You will usually do this when your data is split up into multiple sections like an excel spreadsheet of a year’s sales split by month.

![](Screen_Shot_2021-03-15_at_12.23.40_PM.png)

**Joins**

Joins combine two tables horizontally. For a join, like a Union you have to have at least two tables, what we call our Left Table and our Right Table. You (mostly) have to have at least one matching column between the two tables, and you will match rows from these columns. The most common way to visualize the types of Joins are through Venn Diagrams.

![](Screen_Shot_2021-03-15_at_12.36.03_PM.png)

![](Screen_Shot_2021-03-15_at_12.36.37_PM.png)

You’ll mostly be sticking to Left and Inner Joins. It’s worth your time to learn more about Joins because they are some of the most powerful tools you can use to manipulate data. I use Joins basically every single day in my work. For this course we’re going to stick with relatively simple Joins.

We’re now going to do something called an Inner Join on the [ID] column which will only output exact matches from the [ID] column in our output.

![Inner Join](Screen_Shot_2021-03-15_at_12.37.59_PM.png)

Inner Join

A Left Join keeps all of the data from your Left table and whatever matches from the Right table.

![Left Join](Screen_Shot_2021-03-15_at_12.39.07_PM.png)

Left Join

A Right Join does the exact opposite and keeps everything from your Right table while only bringing in the matches from the Left table.

![Right Join](Screen_Shot_2021-03-15_at_12.39.43_PM.png)

Right Join

A Full Join brings in everything from both tables and matches whatever will match from the columns you specify.

![](Screen_Shot_2021-03-15_at_12.40.52_PM.png)

*Common Join Gotchas*

Joins can get a bit tricky because of the potential for gotchas when joining two tables. The most common one is row duplication where you accidentally duplicate rows because the columns you’re matching on have multiple potential matches. In the example below we’re going to try an Inner Join. You’ll notice the columns in Orange were duplicated.

![Join duplication "error"](Screen_Shot_2021-03-15_at_12.42.17_PM.png)

Join duplication "error"

This isn’t an error per se but it is something to watch out for as it can cause you to duplicate data you don’t intend to duplicate.

![Join duplication "error"](Screen_Shot_2021-03-15_at_12.42.37_PM.png)

Join duplication "error"

### UNION

**Query:**

```sql
SELECT * 
FROM dataset_1
UNION 
SELECT * 
FROM table_to_union
```

**Result:**

In order to see the effect of the result, let's run this query:

```sql
SELECT DISTINCT destination
FROM (
SELECT * 
FROM dataset_1
UNION 
SELECT * 
FROM table_to_union);
```

![](Screen_Shot_2021-05-19_at_5.48.27_PM.png)

### JOIN (LEFT, INNER, OUTER, RIGHT)

**Query:**

```sql
SELECT a.destination, a.time, b.part_of_day
FROM dataset_1 a 
INNER JOIN table_to_join b 
ON a.time  = b.time;
```

**Result:**

![](Screen_Shot_2021-05-19_at_5.50.00_PM.png)

# Advanced Querying

## Subquerying

**Query:**

```sql
SELECT destination, passanger
FROM (SELECT * FROM dataset_1 WHERE passanger = 'Alone');
```

**Result:** 

![](Screen_Shot_2021-05-19_at_5.27.32_PM.png)

## Advanced Filtering

### LIKE

**Query:**

```sql
SELECT *
FROM dataset_1
WHERE weather LIKE 'Sun%';
```

**Result:**

![](Screen_Shot_2021-05-18_at_11.40.50_PM.png)

### Wildcard Operators

![[https://www.w3schools.com/sql/sql_wildcards.asp](https://www.w3schools.com/sql/sql_wildcards.asp)](Screen_Shot_2021-05-18_at_11.43.47_PM.png)

[https://www.w3schools.com/sql/sql_wildcards.asp](https://www.w3schools.com/sql/sql_wildcards.asp)

### BETWEEN

**Query:**

```sql
SELECT DISTINCT temperature 
FROM dataset_1
WHERE temperature BETWEEN 29 AND 75;
```

**Result:**

![](Screen_Shot_2021-05-18_at_10.40.38_PM.png)

### IN

**Query:**

```sql
SELECT occupation
FROM dataset_1
WHERE occupation IN ('Sales & Related', 'Management');
```

**Result:**

![](Screen_Shot_2021-05-18_at_10.34.37_PM.png)

## Advanced Aggregation

### Window Functions

### OVER and PARTITION BY

**Query:**

```sql
SELECT 
	destination, 
	weather, 
	AVG(temperature) OVER (PARTITION BY weather) AS 'avg_temp_by_weather'
FROM dataset_1;
```

**Result:**

![](Untitled%204.png)

You can also order by a certain column too if you want:

```sql
SELECT 
	destination, 
	weather, 
	AVG(temperature) OVER (PARTITION BY weather ORDER BY destination) AS 'avg_temp_by_weather'
FROM dataset_1;
```

### ROW_NUMBER()

**Query:**

```sql
SELECT 
	destination, 
	weather, 
	ROW_NUMBER() OVER (PARTITION BY weather ORDER BY destination) AS 'avg_temp_by_weather'
FROM dataset_1;
```

**Result:**

![](Screen_Shot_2021-05-19_at_12.04.35_AM.png)

### RANK()

**Query:**

```sql
SELECT 
	destination, 
	weather, 
	RANK() OVER (PARTITION BY weather ORDER BY destination) AS 'avg_temp_by_weather'
FROM dataset_1;
```

**Result:**

![](Untitled%205.png)

### DENSE_RANK()

**Query:**

```sql
SELECT 
destination, 
weather, 
DENSE_RANK() OVER (PARTITION BY weather ORDER BY destination) AS 'avg_temp_by_weather'
FROM dataset_1;
```

**Result:**

![](Untitled%206.png)

### NTILE

**Query:**

```sql
SELECT time, NTILE (50) OVER (ORDER BY time)
FROM dataset_1;
```

**Result:**

![](Screen_Shot_2021-05-19_at_5.01.14_PM.png)

### LAG

**Query:**

```sql
SELECT destination, time, LAG(row_count , 1, '99999') OVER (ORDER BY row_count) AS 'LaggedCount' 
FROM dataset_1;
```

**Result:**

![](Screen_Shot_2021-05-19_at_4.38.46_PM.png)

### LEAD

**Query:**

```sql
SELECT destination, time, LEAD(row_count , 1, '99999') OVER (ORDER BY row_count) AS 'LaggedCount' 
FROM dataset_1;
```

**Result:**

![](Screen_Shot_2021-05-19_at_4.39.46_PM.png)

# Manipulating Tables

## Basics

### CREATE

### DROP

### ALTER

## Views

# Tips

Full Query Example

# Further Research

Stored Procedures → Stored Procs

Query Optimization

[Performance Tuning SQL Queries | Advanced SQL - Mode Analytics](https://mode.com/sql-tutorial/sql-performance-tuning/)

# References

Basic SQL Reference

[SQL Tutorial](https://www.w3schools.com/sql/)

Window Functions

[SQL Window Functions | Advanced SQL - Mode Analytics](https://mode.com/sql-tutorial/sql-window-functions/)

Database Schema Information

[What are database schemas? 5 minute guide with examples](https://www.educative.io/blog/what-are-database-schemas-examples)
