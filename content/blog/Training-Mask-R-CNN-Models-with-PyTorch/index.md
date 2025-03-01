---
title: "The Ultimate SQL Cheat Sheet for Interview Success"
subtitle: "Social icons may appear on several pages throughout your site. Learn how to set them up, and control where they show up."
excerpt: "Learn how to train Mask R-CNN models on custom datasets with PyTorch."
date: 2025-02-23
author: "Shiavam Chhetry"
draft: false
# layout options: single, single-sidebar
layout: single
categories: [pytorch, mask-rcnn, object-detection, instance-segmentation, tutorial]
---

## Introduction

SQL proficiency is essential for anyone seeking a job in data analysis or data science. It’s the standard language for managing and querying relational databases, so most organizations use it to make data-driven decisions. So it almost always comes up in job interviews for data-related positions.

This SQL cheat sheet for interviews is designed to help you prepare for these interviews, providing a comprehensive yet concise overview of the most important SQL concepts, commands, and techniques.

## Overview

Each section gives a brief explanation of how SQL clauses and functions work, and then provides a brief summary cheatsheet of the concepts mentioned.

- [**SQL Basics and Syntax**](https://www.interviewquery.com/p/sql-cheat-sheet#sql-basics-and-syntax)
- [**SQL Queries**](https://www.interviewquery.com/p/sql-cheat-sheet#sql-query-cheat-sheet)
- [**SQL Functions**](https://www.interviewquery.com/p/sql-cheat-sheet#sql-functions-cheat-sheet)
- [**SQL Window Functions**](https://www.interviewquery.com/p/sql-cheat-sheet#sql-window-functions-cheat-sheet)
- [**SQL Data Types**](https://www.interviewquery.com/p/sql-cheat-sheet#sql-data-types-cheat-sheet)
- [**Tips For Using A SQL Cheatsheet for Interview Preparation**](https://www.interviewquery.com/p/sql-cheat-sheet#tips-for-using-a-sql-cheat-sheet-in-interviews)
- [**Conclusion**](https://www.interviewquery.com/p/sql-cheat-sheet#conclusion)

## SQL Basics and Syntax

Before diving into specific queries, it’s crucial to understand the basics of SQL and the syntax used for writing SQL commands. SQL is a declarative language, which means you specify what you want to accomplish without outlining the exact steps to get there.

## 

### SELECT and FROM

The `SELECT` statement is used to retrieve data from one or more tables. List the column names you want to retrieve, separated by commas. The `FROM` clause follows the `SELECT` clause and specifies the table(s) from which the data is fetched. Here’s an example:

```sql
SELECT column1, column2
FROM table_name;

```

### WHERE

The `WHERE` clause filters the results of a query based on a specified condition. It appears after the `FROM` clause. For instance:

```sql
SELECT column1, column2
FROM table_name
WHERE column1 = 'value

```

### HAVING

The `HAVING` clause filters the results of a `GROUP BY` query based on a specified condition. It appears after the `GROUP BY` clause. For example:

```sql
SELECT column1, COUNT(*)
FROM table_name
GROUP BY column1
HAVING COUNT(*) > 10;

```

### ORDER BY

The **`ORDER BY`** clause appears after the **`HAVING`** clause (or if there’s no `HAVING` , whatever comes earlier).

It sorts the result table based on specified column(s). You must specify the column names separated by commas, and then specify if you want them to be ordered in ascending (ASC) or descending (DESC) order.

```sql
SELECT column1, column2
FROM table_name
WHERE column1 > 10
ORDER BY column1, column2 ASC;

```

Here’s a brief syntax cheat sheet of what we just mentioned:

1. `SELECT`: Used to retrieve data from one or more tables.
    
    Syntax: `SELECT column1, column2, ... FROM table_name;`
    
2. `FROM`: Specifies the table(s) from which data should be retrieved.
    
    Syntax: `SELECT column1, column2, ... FROM table_name;`
    
3. `WHERE`: Filters the data based on a specified condition.
    
    Syntax: `SELECT column1, column2, ... FROM table_name WHERE condition;`
    
4. `GROUP BY`: Groups rows that have the same values in specified columns.
    
    Syntax: SELECT column1, column2, … FROM table_name GROUP BY column1, column2, …;
    
5. `HAVING`: Filters the results of a GROUP BY query based on a specified condition.
    
    Syntax: `SELECT column1, column2, ... FROM table_name GROUP BY column1, column2, ... HAVING condition;`
    
6. `ORDER BY`: Sorts the result set in ascending or descending order based on one or more columns.
    
    Syntax: `SELECT column1, column2, ... FROM table_name ORDER BY column1 [ASC|DESC], column2 [ASC|DESC], ...;`
    

These basic commands form the foundation of SQL queries, and mastering them is essential for succeeding in data analysis and data science interviews. In the next sections, we’ll provide a cheat sheet of common SQL queries and functions to enhance your interview preparation further.

## SQL Query Cheat Sheet

Mastering SQL queries is essential for data analysis and data science interviews. In this section, we provide a cheat sheet of the most common SQL queries that may come up in a job interview.

### Retrieving Specific Data

Sometimes, you may need to retrieve only specific pieces of data. SQL allows you to either fetch all columns from a table or just a selection of them:

- When you need to analyze all available data in a table, such as examining the entire inventory of a store, use the wildcard **`(*)`** after the SELECT statement:

```sql
SELECT * FROM table_name;

```

- If you’re interested in only certain data, like extracting the names and email addresses of customers for a marketing campaign, enumerate the columns you want, separated by commas:

```sql
SELECT column1, column2 FROM table_name;

```

## 

### Filtering Data Using Conditions

SQL enables you to retrieve subsets of rows that meet specific criteria using the **`WHERE`** clause. This can be particularly helpful for carrying out analyses on certain groups:

- For example, to see all orders from a particular customer or all employees working in a specific area, include the **`WHERE`** clause followed by the column name, an equals sign, and the desired value:

```sql
SELECT * FROM table_name WHERE column1 = 'value';

```

- You can also filter for other conditions than equality. For example, to retrieve rows where the value of a column is greater than a specified value, use the following query, use the WHERE clause followed by the column name, the greater-than operator, and the desired value:

```sql
SELECT * FROM table_name WHERE column1 > 'value';

```

This can be useful for identifying trends or outliers, like finding products with unusually high prices or orders that exceed a certain quantity.

### Sorting Data

Sorting data can help you organize information in a more readable format or rank it based on specific criteria:

- To sort data in ascending order by a column, use the **`ORDER BY`** clause followed by the column name and the **`ASC`** keyword:

```sql
SELECT * FROM table_name ORDER BY column1 ASC;

```

- Conversely, to sort data in descending order, you can use the **`DESC`** keyword instead:

```sql
SELECT * FROM table_name ORDER BY column1 DESC;

```

## 

### Joining Tables

When working with relational databases, you’ll often need to join tables to retrieve related data:

- The following query demonstrates how to retrieve specific columns from two tables, joining them based on a specified condition:

```sql
SELECT table1.column1, table2.column2
FROM table1
INNER JOIN table2
ON table1.column1 = table2.column1;

```

This can be useful for combining related data from different tables, like determining the total revenue generated by each customer by joining the **Customers** and **Orders** tables.

### **Grouping and Aggregating Data**

Grouping and aggregating data helps you summarize information for better insights:

- To group data by a column and count the number of rows in each group, use the **`GROUP BY`** clause followed by the column name:

```sql
SELECT column1, COUNT(*)
FROM table_name
GROUP BY column1;

```

This is useful for finding the total number of products sold per category or the average salary of employees in each department.

## 

### **Limiting the Number of Rows in the Output**

Limiting the number of rows in the output can help you focus on specific portions of data:

- To retrieve a limited number of rows from the result set, use the **`LIMIT`** keyword followed by the desired number of rows:

```sql
SELECT * FROM table_name
LIMIT 10;

```

This is helpful for displaying only the top 10 best-selling products or the 5 most recent orders.

This SQL query cheat sheet covers fundamental queries that form the foundation for data analysis tasks. Practicing these queries and understanding their use cases will help you succeed in data analysis and data science interviews.

Here’s a brief syntax cheat sheet of the common SQL queries:

1. Retrieving specific data:
    - `SELECT * FROM table_name;` Retrieves all columns from the specified table.
    - `SELECT column1, column2 FROM table_name;` Retrieves specific columns from the specified table.
2. Filtering data using conditions:
    - `SELECT * FROM table_name WHERE column1 = 'value';` Retrieves rows where the value of column1 matches the specified value.
    - `SELECT * FROM table_name WHERE column1 > 'value';` Retrieves rows where the value of column1 is greater than the specified value.
3. Sorting data:
    - `SELECT * FROM table_name ORDER BY column1 ASC;` Retrieves data sorted by column1 in ascending order.
    - `SELECT * FROM table_name ORDER BY column1 DESC;` Retrieves data sorted by column1 in descending order.
4. Joining tables:
    - `SELECT table1.column1, table2.column2 FROM table1 INNER JOIN table2 ON table1.column1 = table2.column1;` Retrieves specific columns from two tables, joining them based on the specified condition.
5. Grouping and aggregating data:
    - `SELECT column1, COUNT(*) FROM table_name GROUP BY column1;` Groups data by column1 and counts the number of rows in each group.
6. Limiting the number of rows in the output
    - `SELECT * FROM table_name LIMIT 10;`

## SQL Functions Cheat Sheet

SQL functions are indispensable in data analysis and manipulation, as they simplify complex operations and improve the efficiency of data processing. They’re versatile and can be used with various data types and scenarios. In this section, we’ll explore essential SQL functions, including aggregate functions, string functions, and date functions.

### Aggregate Functions

Aggregate functions perform calculations on a set of values and return a single value. Some common aggregate functions include:

- **`COUNT()`**: Counts the number of rows that match a specified condition.

```sql
SELECT COUNT(column_name) FROM table_name GROUP BY column_name;

```

- **`SUM()`**: Adds up the values of a specified column.

```sql
SELECT SUM(column_name) FROM table_name GROUP BY column_name;

```

- **`AVG()`**: Calculates the average value of a specified column.

```sql
SELECT AVG(column_name) FROM table_name GROUP BY column_name;

```

- **`MIN()`**: Finds the smallest value in a specified column.

```sql
SELECT MIN(column_name) FROM table_name GROUP BY column_name;

```

- **`MAX()`**: Finds the largest value in a specified column.

```sql
SELECT MAX(column_name) FROM table_name GROUP BY column_name;

```

Aggregate functions are often used in conjunction with the **`GROUP BY`** clause to perform calculations on each group of rows with the same value in specified columns. Let’s look at an example:

```sql
SELECT department, COUNT(employee_id) AS employee_count
FROM employees
GROUP BY department;

```

In this example, we group the rows by the **`department`** column and then count the number of employees in each department using the **`COUNT()`** function. The result is a summary table with each department’s employee count.

## 

### String Functions

String functions manipulate and transform text data. Some essential string functions include:

- **`CONCAT()`**: Joins two or more strings together. For example, **`CONCAT('Hello', ' World')`** returns **`'Hello World'`**.

```sql
SELECT CONCAT(column1, column2) FROM table_name;

```

- **`SUBSTRING()`**: Extracts a part of a string. For example, **`SUBSTRING('Hello World', 1, 5)`** returns **`'Hello'`**.

```sql
SELECT SUBSTRING(column_name, start_position, length) FROM table_name

```

- **`LENGTH()`**: Returns the length of a string. For example, **`LENGTH('Hello')`** returns **`5`**.

```sql
SELECT LENGTH(column_name) FROM table_name;

```

- **`REPLACE()`**: Replaces a specified part of a string with another string. For example, **`REPLACE('Hello World', 'World', 'There')`** returns **`'Hello There'`**.

```sql
SELECT REPLACE(column_name, 'old_string', 'new_string') FROM table_name;

```

- `TRIM(column):` Removes leading and trailing spaces from the specified column. For example, `TRIM(' Hello World ')` returns `'Hello World'`

### Date Functions

Date functions help in manipulating and formatting date and time values. They tend to come up frequently in job interviews because they are necessary for any data analysis that involves dates, which is crucial for reporting. Some common date functions include:

- **`NOW()`**: Returns the current date and time.

```sql
SELECT NOW();

```

- **`DATE()`**: Extracts the date part of a date-time expression.

```sql
SELECT DATE(column_name) FROM table_name;

```

- **`DATEDIFF()`**: Calculates the difference between two dates.

```sql
SELECT DATEDIFF(date1, date2) FROM table_name;

```

- **`DATE_ADD()`**: Adds a specified time interval to a date.

```sql
SELECT DATE_ADD(date, INTERVAL value unit) FROM table_name;

```

You need to use the `INTERVAL` keyword followed by the value and the time unit (`DAY`, `MONTH`, `YEAR`, `HOUR`, `MINUTE`, or `SECOND`). For example, in `INTERVAL 5 DAY`, `5` would be the value and `DAY` would be the unit.

Here’s a brief syntax cheat sheet of what we just mentioned:

1. Aggregate functions:
    - `COUNT(column):` Counts the number of non-null values in the specified column.
    - `SUM(column):` Calculates the sum of non-null values in the specified column.
    - `AVG(column):` Computes the average of non-null values in the specified column.
    - `MIN(column):` Finds the minimum value in the specified column.
    - `MAX(column):` Identifies the maximum value in the specified column.
2. String functions:
    - `CONCAT(column1, column2)`: Concatenates two columns of strings.
    - `SUBSTRING(column, start, length)`: Extracts a substring from the specified column, starting at the given position for the specified length.
    - **`LENGTH(column)`:** Returns the string length for the value in the column.
    - `REPLACE(column, search_string, replacement_string)`: Replaces all occurrences of the search_string with the replacement_string in the specified column.
    - `TRIM(column):` Removes leading and trailing spaces from the specified column.
3. Date functions:
    - `NOW()`: Returns the current date and time.
    - `DATE(column)`: Extracts the date part of a datetime value in the specified column.
    - `DATEDIFF(column1, column2)`: Calculates the difference in days between two dates.
    - `DATE_ADD(date, INTERVAL value unit)`

## SQL Window Functions Cheat Sheet

SQL window functions enable advanced data analysis by performing calculations across a set of rows related to the current row. These functions are powerful tools for solving complex analytical problems, such as cumulative sums, running averages, and ranking. In this section, we’ll explore commonly used window functions, including `ROW_NUMBER`, `RANK`, `DENSE_RANK`, and `NTILE`, and explain how they can be used in data analysis tasks.

### **ROW_NUMBER**

The **`ROW_NUMBER()`** function assigns a unique, sequential number to each row within the result set. Here’s an example:

```sql
SELECT product_name, sales, ROW_NUMBER() OVER (ORDER BY sales DESC) AS row_number
FROM products;

```

In this query, we assign a row number to each product based on their sales in descending order.

The `ROW_NUMBER` function is useful for pagination because it lets us relate each row to a unique and sequential number. Therefore, we can choose to show the rows with values between 1 and 10 on one page, then the rows with values between 11 and 20 on another, and so on, and know that each of these pages will have 10 rows. We couldn’t use the rows’ ids directly because there’s no guarantee that it will be sequential (it may skip values, so there may be less than 10 rows with ids between 1 and 10).

### **RANK**

The **`RANK()`** function assigns a unique rank to each row within the result set, with the same rank for rows with equal values. If two rows have the same rank, the next rank is skipped. For example:

```sql
SELECT product_name, sales, RANK() OVER (ORDER BY sales DESC) AS rank
FROM products;

```

This query ranks products by sales, with higher sales receiving a lower rank.

By ranking records, you can analyze trends, detect outliers, and make informed decisions about your data. For instance, a sales manager can use the **`RANK()`** function to identify the top 10 salespeople in the team and reward them accordingly.

### **DENSE_RANK**

The **`DENSE_RANK()`** function is similar to **`RANK()`**, but it does not skip any rank. Rows with equal values receive the same rank, and the next rank is assigned immediately after. Example:

```sql
SELECT product_name, sales, DENSE_RANK() OVER (ORDER BY sales DESC) AS dense_rank
FROM products;

```

In this query, we assign a dense rank to products based on their sales.

### **NTILE**

The **`NTILE()`** function divides the result set into a specified number of equal groups. It assigns each row to one of these groups based on the order specified. For instance:

```sql
SELECT product_name, sales, NTILE(4) OVER (ORDER BY sales DESC) AS quartile
FROM products;

```

This query divides products into four quartiles based on their sales.

**`NTILE()`** is a valuable tool in data analysis for segmenting your data into different groups or categories, such as quartiles, quintiles, or percentiles. By dividing your dataset into equal parts based on a specific measure, you can gain insights into the distribution and trends of your data. For example, a marketing team can use **`NTILE()`** to segment customers into quartiles based on their spending habits. This information can then be used to create targeted marketing campaigns for different customer segments, ultimately improving marketing efficiency and customer engagement.

Here’s a brief syntax cheat sheet of what we just mentioned:

1. `ROW_NUMBER()`: Assigns a unique, sequential number to each row within the result set, starting at 1.
    
    Syntax: `ROW_NUMBER() OVER (ORDER BY column)`
    
2. `RANK()`: Assigns a unique rank to each row within the result set, with the same rank assigned to rows with equal values. Rows with equal values will leave a gap in the sequence.
    
    Syntax: `RANK() OVER (ORDER BY column)`
    
3. `DENSE_RANK()`: Similar to `RANK()`, but without gaps in the rank sequence for equal values.
    
    Syntax: `DENSE_RANK() OVER (ORDER BY column)`
    
4. `NTILE(n)`: Divides the result set into a specified number of groups (n) and assigns a group number to each row.
    
    Syntax: `NTILE(n) OVER (ORDER BY column)`
    

Understanding and mastering SQL window functions is essential for tackling sophisticated data analysis tasks, making them a valuable addition to your SQL interview cheat sheet.

## SQL Data Types Cheat Sheet

Understanding SQL data types is crucial for interview success, as it ensures you can create and manipulate database structures effectively. Here’s a cheat sheet of common SQL data types:

1. `INT`: A whole number with a range depending on the database system.
2. `FLOAT`: A floating-point number, used to store approximate values with varying degrees of precision.
3. `DECIMAL(p, s)`: A fixed-point number with user-defined precision (p) and scale (s). Scale represents the number of digits to the right of the decimal point.
4. `VARCHAR(n)`: A variable-length character string with a maximum length of n characters.
5. `DATE`: Represents a date value (YYYY-MM-DD).
6. `TIMESTAMP`: Represents a date and time value (YYYY-MM-DD HH:MI:SS).

Knowing these common SQL data types will help you create and modify database schemas, write efficient queries, and avoid data type conversion errors during your interview.

