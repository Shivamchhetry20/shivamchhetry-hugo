---
title: "SQL Data Warehouse from Scratch - Part I"
subtitle: "Social icons may appear on several pages throughout your site. Learn how to set them up, and control where they show up."
excerpt: "this project demonstrates a comprehensive data warehousing and analytics solution, from building a data warehouse to generating actionable insights. designed as a portfolio project, it highlights industry best practices in data engineering and analytics."
date: 2024-11-12
author: "Shivam Chhetry"
draft: false
# layout options: single, single-sidebar
layout: single
categories: [SQL, Data Analysis]
---

# **`Data Warehouse and Analytics Project`**

Welcome to the **Data Warehouse and Analytics Project**

this project demonstrates a comprehensive data warehousing and analytics solution, from building a data warehouse to generating actionable insights. designed as a portfolio project, it highlights industry best practices in data engineering and analytics.

---

## 📖 Project Overview

This project involves:

1. **Data Architecture**: Designing a Modern Data Warehouse Using Medallion Architecture **Bronze**, **Silver**, and **Gold** layers.
2. **ETL Pipelines**: Extracting, transforming, and loading data from source systems into the warehouse.
3. **Data Modeling**: Developing fact and dimension tables optimized for analytical queries.
4. **Analytics & Reporting**: Creating SQL-based reports and dashboards for actionable insights.

🎯 This repository is an excellent resource for professionals and students looking to showcase expertise in:

- SQL Development
- Data Architect
- Data Engineering
- ETL Pipeline Developer
- Data Modeling
- Data Analytics

---

## 🛠️ Important Links & Tools:

Everything is for Free!

- [**Datasets](https://www.notion.so/datasets/):** Access to the project dataset (csv files).
- [**SQL Server Express](https://www.microsoft.com/en-us/sql-server/sql-server-downloads):** Lightweight server for hosting your SQL database.
- [**SQL Server Management Studio (SSMS)](https://learn.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-ver16):** GUI for managing and interacting with databases.
- [**Git Repository](https://github.com/):** Set up a GitHub account and repository to manage, version, and collaborate on your code efficiently.
- [**DrawIO](https://www.drawio.com/):** Design data architecture, models, flows, and diagrams.
- [**Notion](https://www.notion.com/):** All-in-one tool for project management and organization.
- [**Notion Project Steps](https://www.notion.so/16ed041640ef80489667cfe2f380b269?pvs=21):** Access to All Project Phases and Tasks.

---

## 🚀 Project Requirements

### Building the Data Warehouse (Data Engineering)

### Objective

Develop a modern data warehouse using SQL Server to consolidate sales data, enabling analytical reporting and informed decision-making.

### Specifications

- **Data Sources**: Import data from two source systems (ERP and CRM) provided as CSV files.
- **Data Quality**: Cleanse and resolve data quality issues prior to analysis.
- **Integration**: Combine both sources into a single, user-friendly data model designed for analytical queries.
- **Scope**: Focus on the latest dataset only; historization of data is not required.
- **Documentation**: Provide clear documentation of the data model to support both business stakeholders and analytics teams.

---

### BI: Analytics & Reporting (Data Analysis)

### Objective

Develop SQL-based analytics to deliver detailed insights into:

- **Customer Behavior**
- **Product Performance**
- **Sales Trends**

These insights empower stakeholders with key business metrics, enabling strategic decision-making.

For more details, refer to [docs/requirements.md](https://www.notion.so/docs/requirements.md).

---

## 🏗️ Data Architecture

The data architecture for this project follows Medallion Architecture **Bronze** , **Silver**, and **Gold** layers:

1. **Bronze Layer**: Stores raw data as-is from the source systems. Data is ingested from CSV Files into SQL Server Database.
2. **Silver Layer**: This layer includes data cleansing, standardization, and normalization processes to prepare data for analysis.
3. **Gold Layer**: Houses business-ready data modeled into a star schema required for reporting and analytics.

![data_architecture.png](data_architecture.png)

---

## 📂 Repository Structure

```
data-warehouse-project/
│
├── datasets/                           # Raw datasets used for the project (ERP and CRM data)
│
├── docs/                               # Project documentation and architecture details
│   ├── etl.drawio                      # Draw.io file shows all different techniquies and methods of ETL
│   ├── data_architecture.drawio        # Draw.io file shows the project's architecture
│   ├── data_catalog.md                 # Catalog of datasets, including field descriptions and metadata
│   ├── data_flow.drawio                # Draw.io file for the data flow diagram
│   ├── data_models.drawio              # Draw.io file for data models (star schema)
│   ├── naming-conventions.md           # Consistent naming guidelines for tables, columns, and files
│
├── scripts/                            # SQL scripts for ETL and transformations
│   ├── bronze/                         # Scripts for extracting and loading raw data
│   ├── silver/                         # Scripts for cleaning and transforming data
│   ├── gold/                           # Scripts for creating analytical models
│
├── tests/                              # Test scripts and quality files
│
├── README.md                           # Project overview and instructions
├── LICENSE                             # License information for the repository
├── .gitignore                          # Files and directories to be ignored by Git
└── requirements.txt                    # Dependencies and requirements for the project

```

---

Let’s start with building Data Warehouse

## Create Database and Schemas

```sql
/*
=============================================================
Create Database and Schemas
=============================================================
Script Purpose:
    This script creates a new database named 'DataWarehouse' after checking if it already exists.
    If the database exists, it is dropped and recreated. Additionally, the script sets up three schemas
    within the database: 'bronze', 'silver', and 'gold'.

WARNING:
    Running this script will drop the entire 'DataWarehouse' database if it exists.
    All data in the database will be permanently deleted. Proceed with caution
    and ensure you have proper backups before running this script.
*/

USE master;
GO

-- Drop and recreate the 'DataWarehouse' database
IF EXISTS (SELECT 1 FROM sys.databases WHERE name = 'DataWarehouse')
BEGIN
    ALTER DATABASE DataWarehouse SET SINGLE_USER WITH ROLLBACK IMMEDIATE;
    DROP DATABASE DataWarehouse;
END;
GO

-- Create the 'DataWarehouse' database
CREATE DATABASE DataWarehouse;
GO

USE DataWarehouse;
GO

-- Create Schemas
CREATE SCHEMA bronze;
GO

CREATE SCHEMA silver;
GO

CREATE SCHEMA gold;
GO
```

# **Creating Bronze Tables: Understanding the SQL DDL Script**

---

In modern data architecture, it's common to define multiple stages in the data pipeline. These stages are often referred to as **bronze**, **silver**, and **gold**. The **bronze** layer typically contains raw, untransformed data, often loaded directly from source systems. The **silver** layer contains cleaned and processed data, and the **gold** layer represents data that has been fully aggregated and transformed for business reporting.

This script is designed to create tables in the **bronze** schema, which is the first stage in our data processing pipeline. The `bronze` schema contains raw data that we can further process and refine in the later stages. Let's break down the purpose and functionality of the script step-by-step:

---

### **1. Checking and Dropping Existing Tables**

Each table in the `bronze` schema is checked for existence before it is created. If a table already exists, it is dropped to ensure that the script can run without errors and the schema is defined from scratch.

```sql
IF OBJECT_ID('bronze.crm_cust_info', 'U') IS NOT NULL
    DROP TABLE bronze.crm_cust_info;
GO

```

The `OBJECT_ID` function checks whether a table with the specified name (`bronze.crm_cust_info` in this case) exists in the database. The `'U'` parameter specifies that we are looking for user-defined tables. If the table exists, it is dropped using the `DROP TABLE` statement.

This same pattern is repeated for each table in the `bronze` schema.

---

### **2. Creating New Tables in the Bronze Schema**

Once existing tables are dropped, new tables are created to hold the raw data in the `bronze` schema. These tables are designed with appropriate columns and data types to capture key information.

### **Table: crm_cust_info**

This table stores customer information such as customer ID, key, name, marital status, gender, and creation date.

```sql
CREATE TABLE bronze.crm_cust_info (
    cst_id INT,
    cst_key NVARCHAR(50),
    cst_firstname NVARCHAR(50),
    cst_lastname NVARCHAR(50),
    cst_marital_status NVARCHAR(50),
    cst_gndr NVARCHAR(50),
    cst_create_date DATE
);

```

- **cst_id**: A unique integer ID for each customer.
- **cst_key**: A unique key representing the customer.
- **cst_firstname** and **cst_lastname**: The customer's first and last names.
- **cst_marital_status** and **cst_gndr**: The customer's marital status and gender.
- **cst_create_date**: The date the customer record was created.

### **Table: crm_prd_info**

This table stores product-related information, including product ID, key, name, cost, and sales timeline.

```sql
CREATE TABLE bronze.crm_prd_info (
    prd_id INT,
    prd_key NVARCHAR(50),
    prd_nm NVARCHAR(50),
    prd_cost INT,
    prd_line NVARCHAR(50),
    prd_start_dt DATETIME,
    prd_end_dt DATETIME
);

```

- **prd_id**: Unique product ID.
- **prd_key**: Unique product key.
- **prd_nm**: Product name.
- **prd_cost**: The cost of the product.
- **prd_line**: The product line or category.
- **prd_start_dt** and **prd_end_dt**: The start and end dates for the product’s lifecycle.

### **Table: crm_sales_details**

This table stores sales order details, including order number, product key, customer ID, order dates, and sales quantities.

```sql
CREATE TABLE bronze.crm_sales_details (
    sls_ord_num NVARCHAR(50),
    sls_prd_key NVARCHAR(50),
    sls_cust_id INT,
    sls_order_dt INT,
    sls_ship_dt INT,
    sls_due_dt INT,
    sls_sales INT,
    sls_quantity INT,
    sls_price INT
);

```

- **sls_ord_num**: Sales order number.
- **sls_prd_key**: The product key related to the sales order.
- **sls_cust_id**: Customer ID who placed the order.
- **sls_order_dt**, **sls_ship_dt**, **sls_due_dt**: Order, shipment, and due dates (all stored as integers, potentially representing dates in `YYYYMMDD` format).
- **sls_sales**, **sls_quantity**, **sls_price**: Sales amount, quantity of products sold, and price for the product.

### **Other Tables in the Bronze Schema**

- **erp_loc_a101**: This table likely contains location data related to some entity, storing a country and its associated ID.

```sql
CREATE TABLE bronze.erp_loc_a101 (
    cid NVARCHAR(50),
    cntry NVARCHAR(50)
);

```

- **erp_cust_az12**: This table seems to store basic customer information such as ID, birthdate, and gender.

```sql
CREATE TABLE bronze.erp_cust_az12 (
    cid NVARCHAR(50),
    bdate DATE,
    gen NVARCHAR(50)
);

```

- **erp_px_cat_g1v2**: This table likely holds product category and subcategory information for items in an ERP system.

```sql
CREATE TABLE bronze.erp_px_cat_g1v2 (
    id NVARCHAR(50),
    cat NVARCHAR(50),
    subcat NVARCHAR(50),
    maintenance NVARCHAR(50)
);

```

---

### **Why Drop Existing Tables?**

Dropping the tables before creating new ones ensures that you don’t encounter issues if there have been schema changes or updates since the last time the script was run. This is especially useful in development or testing environments where you might need to refresh the schema frequently.

In production environments, however, you might consider using migration strategies or version control for database schemas to avoid data loss.

---

### Here is the Full SQL script

```sql
/*
===============================================================================
DDL Script: Create Bronze Tables
===============================================================================
Script Purpose:
    This script creates tables in the 'bronze' schema, dropping existing tables
    if they already exist.
	  Run this script to re-define the DDL structure of 'bronze' Tables
===============================================================================
*/

IF OBJECT_ID('bronze.crm_cust_info', 'U') IS NOT NULL
    DROP TABLE bronze.crm_cust_info;
GO

CREATE TABLE bronze.crm_cust_info (
    cst_id              INT,
    cst_key             NVARCHAR(50),
    cst_firstname       NVARCHAR(50),
    cst_lastname        NVARCHAR(50),
    cst_marital_status  NVARCHAR(50),
    cst_gndr            NVARCHAR(50),
    cst_create_date     DATE
);
GO

IF OBJECT_ID('bronze.crm_prd_info', 'U') IS NOT NULL
    DROP TABLE bronze.crm_prd_info;
GO

CREATE TABLE bronze.crm_prd_info (
    prd_id       INT,
    prd_key      NVARCHAR(50),
    prd_nm       NVARCHAR(50),
    prd_cost     INT,
    prd_line     NVARCHAR(50),
    prd_start_dt DATETIME,
    prd_end_dt   DATETIME
);
GO

IF OBJECT_ID('bronze.crm_sales_details', 'U') IS NOT NULL
    DROP TABLE bronze.crm_sales_details;
GO

CREATE TABLE bronze.crm_sales_details (
    sls_ord_num  NVARCHAR(50),
    sls_prd_key  NVARCHAR(50),
    sls_cust_id  INT,
    sls_order_dt INT,
    sls_ship_dt  INT,
    sls_due_dt   INT,
    sls_sales    INT,
    sls_quantity INT,
    sls_price    INT
);
GO

IF OBJECT_ID('bronze.erp_loc_a101', 'U') IS NOT NULL
    DROP TABLE bronze.erp_loc_a101;
GO

CREATE TABLE bronze.erp_loc_a101 (
    cid    NVARCHAR(50),
    cntry  NVARCHAR(50)
);
GO

IF OBJECT_ID('bronze.erp_cust_az12', 'U') IS NOT NULL
    DROP TABLE bronze.erp_cust_az12;
GO

CREATE TABLE bronze.erp_cust_az12 (
    cid    NVARCHAR(50),
    bdate  DATE,
    gen    NVARCHAR(50)
);
GO

IF OBJECT_ID('bronze.erp_px_cat_g1v2', 'U') IS NOT NULL
    DROP TABLE bronze.erp_px_cat_g1v2;
GO

CREATE TABLE bronze.erp_px_cat_g1v2 (
    id           NVARCHAR(50),
    cat          NVARCHAR(50),
    subcat       NVARCHAR(50),
    maintenance  NVARCHAR(50)
);
GO
```

---

### Check for NULLs or Duplicates in Primary Key

---

### SQL Query to Find Duplicate or Null `cst_id` with Their Counts

When working with customer data, it's often essential to spot potential data quality issues, such as duplicates or null values. In this post, I'll walk you through an SQL query that helps identify such issues in the `cst_id` field from the `bronze.crm_cust_info` table.

Let's dive into the query:

```sql
SELECT
     cst_id,
     COUNT(*) AS no_of_counts
FROM bronze.crm_cust_info
GROUP BY cst_id
HAVING COUNT(*) > 1 OR cst_id IS NULL;

```

### 1. **`SELECT` Clause**:

The first part of the query is the `SELECT` clause, which is where we define what data we want to retrieve from the database. In this case, we want to retrieve the `cst_id` and the count of how many times each `cst_id` appears in the table. The `COUNT(*)` function will count all the rows for each `cst_id`.

```sql
SELECT
     cst_id,
     COUNT(*) AS no_of_counts

```

- **`cst_id`**: This represents the unique customer identifier in the table.
- **`COUNT(*) AS no_of_counts`**: This counts how many times each `cst_id` appears. We alias this count as `no_of_counts` for better readability.

### 2. **`FROM` Clause**:

The `FROM` clause tells the database which table to pull the data from. In this case, we're working with the `bronze.crm_cust_info` table, which presumably contains customer information.

```sql
FROM bronze.crm_cust_info

```

### 3. **`GROUP BY` Clause**:

Now, we're interested in counting the occurrences of each `cst_id`. To do this, we group the data by `cst_id`. This means that for each unique `cst_id`, we'll aggregate the rows together and count how many times that `cst_id` appears.

```sql
GROUP BY cst_id

```

### 4. **`HAVING` Clause**:

The `HAVING` clause comes into play after the grouping, and it allows us to filter out results based on the aggregate values.

In this case, we're interested in `cst_id`s that either:

- Appear more than once (`COUNT(*) > 1`), which indicates a duplicate.
- Or, the `cst_id` is `NULL`, which is another common issue in databases that we might want to address.

```sql
HAVING COUNT(*) > 1 OR cst_id IS NULL;

```

### 5. **The Result:**

Once the query is run, the result looks something like this:

| **cst_id** | **no_of_counts** |
| --- | --- |
| 29449 | 2 |
| 29473 | 2 |
| 29433 | 2 |
| NULL | 3 |
| 29483 | 2 |
| 29466 | 3 |

From the output, we can easily spot:

- Duplicate `cst_id`s, such as `29449`, `29473`, and `29433`, which each appear twice.
- Null values in the `cst_id` column, which might indicate incomplete data (3 occurrences in this case).

### 6. **Why Is This Useful?**

This query is really helpful in data cleaning and validation. You can use it to identify:

- Duplicate entries that might lead to skewed analysis or incorrect reporting.
- `NULL` values that could represent missing or unprocessed data.

In a real-world scenario, once you find these duplicates or nulls, you can either:

- Investigate why these entries are duplicated and fix the underlying issue.
- Decide whether to handle null values by filling them with a placeholder value or removing those rows from your dataset.

---

### Investigating a Duplicate `cst_id` to Explore the Data Further

After identifying that `cst_id = 29466` is a duplicate, let’s now dive deeper into the data associated with this particular `cst_id`. In the previous step, we ran a query to identify duplicate entries, but now we want to understand the actual data for this customer. Here's the query we'll use to explore all records for `cst_id = 29466`:

```sql
sql
Copy
SELECT
    *
FROM bronze.crm_cust_info
WHERE cst_id = 29466;

```

### Explanation:

- **`SELECT *`**: This will select all columns from the table for each row associated with `cst_id = 29466`.
- **`WHERE cst_id = 29466`**: This filters the data to include only rows where `cst_id` matches `29466`.

### Result:

When we run the above query, the output is as follows:

| cst_id | cst_key | cst_firstname | cst_lastname | cst_martial_status | cst_gndr | cst_create_date |
| --- | --- | --- | --- | --- | --- | --- |
| 29466 | AW00029466 | NULL | NULL | NULL | NULL | 2026-01-25 |
| 29466 | AW00029466 | Lance | Jimenez | M | NULL | 2026-01-26 |
| 29466 | AW00029466 | Lance | Jimenez | M | M | 2026-01-27 |

### Analysis:

Now that we have the records for `cst_id = 29466`, let’s break down what we’re seeing:

1. **First Row**:
    - The first record for `cst_id = 29466` shows a row with missing first name, last name, marital status, gender, and create date. This could indicate that the entry was initially created with minimal information.
2. **Second Row**:
    - The second record provides more complete information: `Lance Jimenez` is the first and last name, and the marital status is marked as `M` (presumably for "Married"). However, the gender column is still `NULL`, which may be an issue in terms of data consistency.
3. **Third Row**:
    - The third row for the same `cst_id` contains the same name but this time includes a gender value (`M` for male), indicating that some data has been updated or filled in over time.

### Understanding the Possible Causes of Duplicates

From the data, it appears that `cst_id = 29466` has multiple records with different levels of information, suggesting that this might be due to the following reasons:

1. **Incomplete Data Entry**:

    The first record could have been created as an incomplete entry, with only the `cst_key` populated, leaving other fields such as name and gender as `NULL`. The subsequent records show the `cst_id` being updated with the missing data over time.

2. **Multiple Entries for Updates**:

    It’s also possible that the system was set up to allow multiple records for the same `cst_id` as new information was provided. This might indicate that the data entry process needs to be revisited to ensure that we don’t create multiple records for the same person unnecessarily.

3. **Data Correction**:

    Another possible cause is data corrections. If the customer’s information was initially incomplete or incorrect, a new entry may have been created, which is why we see a gradual filling of missing fields.


### Keeping the Latest Record Using `ROW_NUMBER()` Window Function

Now that we’ve identified duplicate entries, let’s use the `ROW_NUMBER()` window function to easily isolate and keep the latest record for each `cst_id`.

The `ROW_NUMBER()` function assigns a unique number to each row within a partition of data. In our case, we partition by `cst_id` and order by `cst_create_date` in descending order, so the most recent entry for each `cst_id` will be assigned the value `1`.

Here’s the query to achieve that:

```sql
sql
Copy
SELECT
    *
FROM (
    SELECT
        *,
        ROW_NUMBER() OVER (PARTITION BY cst_id ORDER BY cst_create_date DESC) AS flag_last
    FROM bronze.crm_cust_info
) AS ranked
WHERE flag_last = 1;

```

### Explanation:

- **`ROW_NUMBER() OVER (PARTITION BY cst_id ORDER BY cst_create_date DESC) AS flag_last`**:
    - The `ROW_NUMBER()` function assigns a number to each row within each `cst_id` partition.
    - The `PARTITION BY cst_id` ensures that the row numbering restarts for each unique `cst_id`.
    - The `ORDER BY cst_create_date DESC` ensures that the most recent record (the one with the latest `cst_create_date`) gets assigned the row number `1`.
- **Outer query**:

    The outer query filters out all records except those where `flag_last = 1`. This ensures that only the latest record for each `cst_id` is kept.


### Result:

If you run this query, the result will show only the most recent entry for each `cst_id`:

| cst_id | cst_key | cst_firstname | cst_lastname | cst_martial_status | cst_gndr | cst_create_date | flag_last |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 29466 | AW00029466 | Lance | Jimenez | M | M | 2026-01-27 | 1 |

Here is the output after extracting only the last entry of each cst_id:

![Screenshot 2025-03-03 183014.png](Screenshot_2025-03-03_183014.png)

### Checking for Unwanted Spaces in Customer Data

Another common data issue we encounter is unwanted spaces in string values, such as leading or trailing spaces in names or other fields. These spaces can cause problems with data consistency, searching, and comparisons.

In this step, let's focus on how to find rows where the `cst_firstname` contains leading or trailing spaces. The SQL `TRIM()` function is used to remove these unwanted spaces. If the `cst_firstname` doesn't match its trimmed version, we know that there are unwanted spaces.

Here's the SQL query you can use to identify such rows:

```sql
-- Check unwanted space for cst_firstname col
SELECT cst_firstname
FROM bronze.crm_cust_info
WHERE cst_firstname != TRIM(cst_firstname);

-- Check unwanted space for cst_lastname col
SELECT cst_lastname
FROM bronze.crm_cust_info
WHERE cst_lastname != TRIM(cst_lastname);

-- Check unwanted space for cst_gndr col
SELECT cst_gndr
FROM bronze.crm_cust_info
WHERE cst_gndr != TRIM(cst_gndr);

```

### Explanation:

- **`TRIM(cst_firstname)`**: This function removes any leading and trailing spaces from the `cst_firstname` field. If the original value is not equal to the same value after trimming it means there are spaces!
- **`WHERE cst_firstname != TRIM(cst_firstname)`**: This condition checks if the original value of `cst_firstname` is different from its trimmed version, which would indicate that there are unwanted spaces.

After removing the duplicates in the primary key and the unwanted spaces here is the so far sql code with the output.

```sql
SELECT
	 cst_id
	,cst_key
	,TRIM(cst_firstname) AS cst_firstname   -- Removed unwanted space
	,TRIM(cst_lastname) AS cst_lastname     -- Removed unwanted space
	,cst_marital_status
	,cst_gndr
	,cst_create_date
FROM (
	SELECT
		 *
		,ROW_NUMBER() OVER (PARTITION BY cst_id ORDER BY cst_create_date DESC) AS flag_last
	FROM bronze.crm_cust_info
) t where flag_last = 1
```

### OUTPUT⬇️

![Screenshot 2025-03-03 184050.png](Screenshot_2025-03-03_184050.png)

## **Let’s do Quality Check**

![quality_check.drawio.png](quality_check.drawio.png)

### Data Standardization & Consistency in the `cst_gndr` Column

---

When working with customer data, it's crucial to ensure that the information stored in your database is clear, consistent, and meaningful. One common issue we often encounter is abbreviated terms that can lead to confusion or inconsistency, especially when dealing with gender identifiers.

In our data warehouse, we aim to store more readable and meaningful values instead of abbreviations like "F" and "M". For example, instead of "F" representing Female and "M" representing Male, we will replace these values with their full terms for clarity and consistency.

Let’s take a look at how we can standardize the values in the `cst_gndr` column using a `CASE WHEN` statement.

### Checking Existing Values in the `cst_gndr` Column

First, we check the distinct values present in the `cst_gndr` column to identify how the gender data is currently stored. We can run the following query to retrieve all unique values:

```sql
SELECT DISTINCT cst_gndr
FROM bronze.crm_cust_info;

```

### Output:

**cst_gndr**

---

NULL

---

F

---

M

---

From the output, we can see the following:

- `NULL`: There are some rows where the gender information is missing.
- `F`: This is used to represent "Female".
- `M`: This is used to represent "Male".

As noted earlier, we aim to replace these abbreviations with the full terms "Female" and "Male" to improve data clarity.

### Standardizing Gender Data with `CASE WHEN`

Now, we can update the `cst_gndr` values by using the `CASE WHEN` statement to transform "F" into "Female" and "M" into "Male". The `CASE WHEN` statement allows us to apply conditional logic directly within a query. Here's how we can use it to replace the abbreviated values:

```sql
SELECT
    cst_id,
    cst_key,
    TRIM(cst_firstname) AS cst_firstname,
    TRIM(cst_lastname) AS cst_lastname,
    cst_marital_status,
    CASE
        WHEN cst_gndr = 'F' THEN 'Female'
        WHEN cst_gndr = 'M' THEN 'Male'
        ELSE cst_gndr
    END AS cst_gndr,
    cst_create_date
FROM (
    SELECT
        *,
        ROW_NUMBER() OVER (PARTITION BY cst_id ORDER BY cst_create_date DESC) AS flag_last
    FROM bronze.crm_cust_info
) t
WHERE flag_last = 1;

```

### Explanation:

- **`CASE WHEN`**: The `CASE WHEN` statement checks the value in the `cst_gndr` column:
    - If `cst_gndr = 'F'`, it returns "Female".
    - If `cst_gndr = 'M'`, it returns "Male".
    - The `ELSE` part ensures that if the value is neither "F" nor "M" (like `NULL`), it remains unchanged.
- **`ROW_NUMBER() OVER (PARTITION BY cst_id ORDER BY cst_create_date DESC)`**: As discussed in a previous step, the `ROW_NUMBER()` function helps us to find the most recent record for each `cst_id`. This ensures that we only keep the latest entry for each customer.
- **`TRIM(cst_firstname)` and `TRIM(cst_lastname)`**: We’re using the `TRIM()` function to ensure that any leading or trailing spaces in the `cst_firstname` and `cst_lastname` columns are removed, further improving data consistency.
- **`WHERE flag_last = 1`**: This condition filters the results to only include the most recent record for each customer (based on `cst_create_date`).

### Result after fixing

Once you execute the query, the `cst_gndr` column will show "Female" instead of "F" and "Male" instead of "M". The output will look something like this:

| cst_id | cst_key | cst_firstname | cst_lastname | cst_marital_status | cst_gndr | cst_create_date |
| --- | --- | --- | --- | --- | --- | --- |
| 29466 | AW00029466 | Lance | Jimenez | M | Male | 2026-01-27 |

As you can see, "M" is now replaced with "Male" for better clarity, and "F" would be similarly replaced with "Female" in the rows where applicable.

---

### Further Enhancing Data Standardization with `UPPER()` and `TRIM()`

To ensure consistency in the `cst_gndr` column, we can further enhance the `CASE WHEN` logic by using the `UPPER()` and `TRIM()` functions. The `UPPER()` function makes the comparison case-insensitive, and `TRIM()` removes any leading or trailing spaces, ensuring that all values are standardized.

Here’s the updated SQL query:

```sql
SELECT
    cst_id,
    cst_key,
    TRIM(cst_firstname) AS cst_firstname,
    TRIM(cst_lastname) AS cst_lastname,
    cst_marital_status,
    CASE
        WHEN UPPER(TRIM(cst_gndr)) = 'F' THEN 'Female'
        WHEN UPPER(TRIM(cst_gndr)) = 'M' THEN 'Male'
        ELSE 'n/a'
    END AS cst_gndr,
    cst_create_date
FROM (
    SELECT
        *,
        ROW_NUMBER() OVER (PARTITION BY cst_id ORDER BY cst_create_date DESC) AS flag_last
    FROM bronze.crm_cust_info
) t
WHERE flag_last = 1;

```

This ensures that "f", " F", or "M" are all treated as "Female" and "Male", respectively, and any extra spaces are removed. If there are any unexpected values, we default them to "n/a".

---

Just like we standardized the `cst_gndr` column, we can also clean up the `cst_marital_status` column. We have three distinct values in the data:

- `NULL` (indicating missing data),
- `S` (representing "Single"),
- `M` (representing "Married").

We will use a `CASE WHEN` statement along with the `UPPER()` and `TRIM()` functions to standardize these values into more meaningful terms.

### Updated SQL Query:

```sql
SELECT
	 cst_id
	,cst_key
	,TRIM(cst_firstname) AS cst_firstname
	,TRIM(cst_lastname) AS cst_lastname
	,CASE WHEN UPPER(TRIM(cst_marital_status)) = 'S' THEN 'Single'
	      WHEN UPPER(TRIM(cst_marital_status)) = 'M' THEN 'Married'
	      ELSE 'n/a'
	 END AS cst_marital_status
	,CASE WHEN UPPER(TRIM(cst_gndr)) = 'F' THEN 'Female'
	      WHEN UPPER(TRIM(cst_gndr)) = 'M' THEN 'Male'
	      ELSE 'n/a'
	 END AS cst_gndr
	,cst_create_date
FROM (
	SELECT
		 *
		,ROW_NUMBER() OVER (PARTITION BY cst_id ORDER BY cst_create_date DESC) AS flag_last
	FROM bronze.crm_cust_info
) t where flag_last = 1
```

### Explanation:

- **`UPPER(TRIM(cst_marital_status))`**: This ensures that the `cst_marital_status` field is case-insensitive and that any extra spaces are removed.
- **`CASE WHEN`**:
    - Converts "S" to "Single".
    - Converts "M" to "Married".
    - Any other value (including `NULL` or unexpected entries) is replaced with "n/a".

---

---

### Inserting Cleaned Data into the Silver Layer

After standardizing and cleaning the data in the bronze layer, the next step is to move the cleaned data into the silver layer. The silver layer typically represents data that is cleaned, transformed, and ready for analysis or further processing.

In this case, we have applied various transformations to the `cst_gndr` and `cst_marital_status` columns, ensuring that gender and marital status values are consistent and clear. Now, it’s time to insert the updated data into the silver layer.

### SQL Query to Insert Cleaned Data into the Silver Layer

```sql
INSERT INTO silver.crm_cust_info (
    cst_id,
    cst_key,
    cst_firstname,
    cst_lastname,
    cst_marital_status,
    cst_gndr,
    cst_create_date
)
SELECT
	 cst_id
	,cst_key
	,TRIM(cst_firstname) AS cst_firstname
	,TRIM(cst_lastname) AS cst_lastname
	,CASE WHEN UPPER(TRIM(cst_marital_status)) = 'S' THEN 'Single'
	      WHEN UPPER(TRIM(cst_marital_status)) = 'M' THEN 'Married'
	      ELSE 'n/a'
	 END AS cst_marital_status  -- Normalize martial status values to readable format
	,CASE WHEN UPPER(TRIM(cst_gndr)) = 'F' THEN 'Female'
	      WHEN UPPER(TRIM(cst_gndr)) = 'M' THEN 'Male'
	      ELSE 'n/a'
	 END AS cst_gndr  -- Normalize gender values to readable format
	,cst_create_date
FROM (
	SELECT
		 *
		,ROW_NUMBER() OVER (PARTITION BY cst_id ORDER BY cst_create_date DESC) AS flag_last
	FROM bronze.crm_cust_info
) t where flag_last = 1;  -- Select the most recent record per customer

```

### Explanation:

1. **Target Table**: We are inserting the cleaned data into the `silver.crm_cust_info` table. This table should already exist in your data warehouse, and it typically holds data that has been cleaned and transformed, ready for further analytics.
2. **Columns**: The columns being inserted are:
    - `cst_id`: The customer ID.
    - `cst_key`: The customer key.
    - `cst_firstname`: The cleaned first name with leading/trailing spaces removed.
    - `cst_lastname`: The cleaned last name with leading/trailing spaces removed.
    - `cst_marital_status`: The standardized marital status ("Single" or "Married").
    - `cst_gndr`: The standardized gender ("Female" or "Male").
    - `cst_create_date`: The customer creation date.
3. **Data Selection**: The data is selected from the `bronze.crm_cust_info` table where we have already applied the necessary transformations, including:
    - **`TRIM()`** for removing leading/trailing spaces in `cst_firstname` and `cst_lastname`.
    - **`UPPER()` and `TRIM()`** in the `CASE WHEN` statements for the `cst_marital_status` and `cst_gndr` columns, ensuring that all values are consistent and standardized.
    - **`ROW_NUMBER()`** is used in the previous queries to ensure only the most recent records for each customer are selected (`flag_last = 1`).
4. **Purpose of the Insert**: This `INSERT` operation populates the silver layer with the cleaned data, ensuring that the data is now ready for downstream processes such as reporting, business intelligence, or analytics.

### Why Inserting into the Silver Layer is Important

- **Data Transformation**: The silver layer typically stores transformed and cleaned data. It’s a bridge between raw data (bronze layer) and final analytical datasets (gold layer). In our case, by inserting the cleaned data into the silver layer, we ensure it’s ready for consumption by analysts and data scientists.
- **Improved Data Quality**: By inserting standardized data, we ensure consistency across all systems, making it easier to trust and use for decision-making. For example, standardized gender and marital status values make it easier to group and analyze customer data.
- **Ready for Analysis**: The silver layer is typically optimized for analytical workloads. The cleaned and standardized data in the silver layer can now be used for various analysis, reporting, and business decision-making.

### Conclusion: Moving Cleaned Data to the Silver Layer

By inserting the cleaned and standardized data from the bronze layer into the silver layer, we ensure that the data is ready for more complex analyses and reporting. The silver layer serves as an essential middle ground for transforming raw data into meaningful insights, improving data quality, and streamlining the entire data pipeline.

---

### Final Data Quality Checks in the Silver Layer

After inserting the cleaned and standardized data into the silver layer, it's crucial to perform final data quality checks. This ensures that the data is free from duplicates, unwanted spaces, and maintains overall consistency. These checks ensure that the data in the silver layer is of the highest quality before it is used for reporting, analysis, or decision-making.

### Checking for Duplicate Primary Keys

One of the first checks to perform is to ensure there are no duplicates in the primary key, which could result from multiple records being inserted. In our case, the primary key is the `cst_id`. Duplicate primary keys can lead to data integrity issues, so it’s important to confirm that each customer has only one unique record.

Here’s how we can check for duplicates in the `cst_id` column:

```sql
SELECT
    cst_id,
    COUNT(*) AS no_of_counts
FROM silver.crm_cust_info
GROUP BY cst_id
HAVING COUNT(*) > 1;

```

If this query returns any results, it indicates that there are duplicate `cst_id` values in the table. In this case, we need to resolve the duplicates by identifying the most recent or valid record for each customer.

### 13.2: Checking for Unwanted Spaces

Next, we need to ensure that there are no unwanted spaces in key columns like `cst_firstname` and `cst_lastname`. Leading or trailing spaces can cause issues in analysis and reporting, so we should check for any instances where the names contain extra spaces.

Here’s how to check for unwanted spaces in the `cst_firstname` column:

```sql
SELECT cst_firstname
FROM silver.crm_cust_info
WHERE cst_firstname != TRIM(cst_firstname);

```

If any results are returned, we can further clean the data by removing those unwanted spaces.

### Ensuring Data Standardization & Consistency

Finally, we can recheck the data for consistency and standardization. This includes verifying that the gender and marital status values are consistent across the entire dataset. For instance, we can ensure that the `cst_gndr` column contains only "Male" or "Female" and that `cst_marital_status` is correctly labeled as "Single" or "Married" (or "n/a" for invalid entries).

Here’s a simple query to check the distinct values in the `cst_gndr` and `cst_marital_status` columns:

```sql
SELECT DISTINCT cst_gndr
FROM silver.crm_cust_info;

```

This will show the distinct values in the `cst_gndr` column to ensure that only "Male" and "Female" are present, along with any "n/a" values. Similarly, we can do the same for the `cst_marital_status` column.

```sql
SELECT DISTINCT cst_marital_status
FROM silver.crm_cust_info;

```

### Conclusion: Final Data Quality Assurance

Performing these final data quality checks in the silver layer ensures that the data is clean, consistent, and ready for advanced analysis. By checking for duplicates, unwanted spaces, and verifying data standardization, we can confidently move forward with using this data for reporting, analytics, or any other downstream processes.

These checks are essential for maintaining the integrity of your data warehouse and ensuring that the data being used for decision-making is of the highest quality.

---

## **What we have done so far.**

![clean_data.drawio.png](clean_data.drawio.png)

---

# Build Silver Layer Clean & Load crm_prd_info

Here is the top 10 values from the crm_prd_info table

![Screenshot 2025-03-03 193840.png](Screenshot_2025-03-03_193840.png)

Let’s check first for Nulls or Duplicates in the **prd_id** key (Primary Key) using following script

```sql
SELECT
	 prd_id
	,COUNT(*)
FROM bronze.crm_prd_info
GROUP BY prd_id
HAVING COUNT(*) > 1 OR prd_id IS NULL;
```

After verifying that there are no duplicates in the **`prd_id`** column, we now move on to the **`prd_key`** column. We’ve identified that **`prd_key`** contains multiple pieces of information that need to be extracted separately for better clarity and usability. To achieve this, we can use the `SUBSTRING()` function to extract specific parts of the string in the **`prd_key`** colum

### Using `SUBSTRING()` Function

The `SUBSTRING()` function allows us to extract a specific portion of a string. It takes three arguments:

1. **Argument 1**: The column name (in this case, **`prd_key`**) that we want to extract data from.
2. **Argument 2**: The starting position (from where to begin extracting).
3. **Argument 3**: The length (how many characters you want to extract).

### Example:

Let’s say you want to extract the first 5 characters from the **`prd_key`** column:

```sql
SELECT SUBSTRING(prd_key, 1, 5) AS extracted_part
FROM silver.crm_prod_info;

```

- **`prd_key`**: The column that contains the string.
- **`1`**: The starting position, which is the 1st character of the string.
- **`5`**: The number of characters to extract, so it will get the first 5 characters.

If **`prd_key`** contains a value like `CO-RF-FR-R92B-58`, the result of this query would be `CO-RF`.

So, let’s add the new column into our database

```sql
SELECT
	 prd_id
	,prd_key
	,SUBSTRING(prd_key, 1, 5) AS cat_id
	,prd_nm
	,prd_cost
	,prd_line
	,prd_start_dt
	,prd_end_dt
FROM bronze.crm_prd_info
```

As part of our data processing, we need to merge data from our **`silver.crm_prod_info`** table with another table called **`erp_px_cat_g1v2`**. This table contains an ID column called **`CO_RF`** that we intend to join with the **`prd_key`** column in our table. However, there is a key difference between the two columns that could prevent a successful join.

- **In our table**: The **`prd_key`** column contains values like `CO-RF` where the delimiter between parts of the string is a hyphen (``).
- **In the `erp_px_cat_g1v2` table**: The **`CO_RF`** column contains values like `CO_RF`, where the delimiter is an underscore (`_`).

This difference in formatting (hyphen vs underscore) would cause issues when trying to join the two tables, as the join condition would not match. Therefore, to ensure we can correctly merge the tables, we need to standardize the format of the **`prd_key`** column by replacing the hyphen (`-`) with an underscore (`_`).

### Using the `REPLACE()` Function to Change Hyphen to Underscore

To perform this transformation, we use the `REPLACE()` function. This function allows us to search for a specific substring (in this case, the hyphen `-`) and replace it with another substring (the underscore `_`), ensuring the two columns are now compatible for joining.

### SQL Query to Replace Hyphen with Underscore

```sql
SELECT
	 prd_id
	,prd_key
	,REPLACE(SUBSTRING(prd_key, 1, 5), '-', '_') AS cat_id
	,prd_nm
	,prd_cost
	,prd_line
	,prd_start_dt
	,prd_end_dt
FROM bronze.crm_prd_info
```

So far our table look like below where we an see our new column **cat_id** has been added correctly.

![Screenshot 2025-03-03 195445.png](Screenshot_2025-03-03_195445.png)

In addition to formatting the **`prd_key`** column to ensure it correctly joins with the **`erp_px_cat_g1v2`** table, we also need to join the **`prd_key`** with the **`sls_prd_key`** column in the **`crm_sales_details`** table. The **`prd_key`** in our table contains extra characters that we do not need for this join. Specifically, we need to extract a part of the **`prd_key`** to match the **`sls_prd_key`**.

To achieve this, we can use the `SUBSTRING()` function, which allows us to extract a portion o the string from the **`prd_key`**.

### Why We Need to Extract the `prd_key`

The **`prd_key`** in our table may include extra characters, such as prefixes or additional information, that are irrelevant for the join with **`sls_prd_key`** in the **`crm_sales_details`** table. The **`sls_prd_key`** column likely contains a subset of the **`prd_key`** information, starting from the 7th character onward.

### Using the `SUBSTRING()` Function to Extract the Key

The `SUBSTRING()` function allows us to specify the starting position and the length of the substring to be extracted. In this case, we want to start extracting from the 7th character of the **`prd_key`** and include all characters from that point onward.

Here’s how we can extract the relevant portion of the **`prd_key`**:

```sql
SELECT
	 prd_id
	,prd_key
	,REPLACE(SUBSTRING(prd_key, 1, 5), '-', '_') AS cat_id
	,SUBSTRING(prd_key,7, LEN(prd_key)) AS prd_key
	,prd_nm
	,prd_cost
	,prd_line
	,prd_start_dt
	,prd_end_dt
FROM bronze.crm_prd_info

```

### Explanation:

- **`prd_key`**: This is the column containing the full product key.
- **`7`**: This specifies the starting position (the 7th character of the **`prd_key`**). We're ignoring the first 6 characters, as they are not needed for the join.
- **`LEN(prd_key)`**: This specifies the length of the string to extract. By using `LEN(prd_key)`, we ensure that we extract all remaining characters from the 7th position to the end of the string.

This query extracts the portion of the **`prd_key`** starting from the 7th character until the end, which matches the format of the **`sls_prd_key`** column in the **`crm_sales_details`** table.

---

Now, let’s move on to the next column which is **prd_nm** (product name)

First check the unwanted spaces I have checked there is not unwanted space

Let’, check for NULLs or Negative Numbers in the **prd_cost** columns

```sql
SELECT prd_cost
FROM bronze.crm_prd_info
WHERE prd_cost < 0 OR prd_cost IS NULL;

```

### |we can see we don’t have negative values but we do have NULLs which we need to handle that

Using ISNULL() we can replace null —> 0

```sql
SELECT
	 prd_id
	,prd_key
	,REPLACE(SUBSTRING(prd_key, 1, 5), '-', '_') AS cat_id
	,SUBSTRING(prd_key,7, LEN(prd_key)) AS prd_key
	,prd_nm
	,ISNULL(prd_cost, 0) AS prd_cost -- Here we have used isnull to replace the NULLs value with 0
	,prd_line
	,prd_start_dt
	,prd_end_dt
FROM bronze.crm_prd_info

```

---

Now Let’s see the prd_line colunm

```sql
-- Data Standardization & Consistency
SELECT DISTINCT prd_line
FROM bronze.crm_prd_info
```

![Screenshot 2025-03-03 201051.png](Screenshot_2025-03-03_201051.png)

Note: We have to replace the above distinct value with the full name for  better data standardization.

So to do that we are going to use CASE WHEN

```sql
-- Quick CASE WHEN ideal for simple value mapping

CASE UPPER(TRIM(prd_line))
		  WHEN 'M' THEN 'Mountain'
		  WHEN 'R' THEN 'Road'
		  WHEN 'S' THEN 'Other Sales'
		  WHEN 'T' THEN 'Touring'
		  ELSE 'n/a'
	 END AS prd_line
```

Here is the full code so far we gave

```sql
SELECT
	 prd_id
	,prd_key
	,REPLACE(SUBSTRING(prd_key, 1, 5), '-', '_') AS cat_id
	,SUBSTRING(prd_key,7, LEN(prd_key)) AS prd_key
	,prd_nm
	,ISNULL(prd_cost, 0) AS prd_cost
	,CASE UPPER(TRIM(prd_line))
		  WHEN 'M' THEN 'Mountain'
		  WHEN 'R' THEN 'Road'
		  WHEN 'S' THEN 'Other Sales'
		  WHEN 'T' THEN 'Touring'
		  ELSE 'n/a'
	 END AS prd_line
	,prd_start_dt
	,prd_end_dt
FROM bronze.crm_prd_info

```

---

Now let’s got to the last columns prd_start_dt & prd_end_dt

Let’s check for Invalid Date Orders our hypothesis is **End date must not be earlier than the start date**

```sql
-- Check for invalid date Orders
SELECT *
FROM bronze.crm_prd_info
WHERE prd_end_dt < prd_start_dt;
```

Here is the output:

![Screenshot 2025-03-03 213504.png](Screenshot_2025-03-03_213504.png)

Observation:

> In our table the prd_start_dt is always greater then prd_end_dt which is incorrect/not making sense at all.
>

How we can tackle this issue?🤔

> For complex transformations in SQL, I typically narrow it down to a specific example and brainstorm multiple solution approaches.
>

Solutions:

1. Switch End Date and Start Date —>

    ![Screenshot 2025-03-03 215219.png](Screenshot_2025-03-03_215219.png)

    **Note**: Issue with this approach is the dates are overlapping and it is not making any sense, the prices are not acceptable

    =⇒ Above you can see in the approach testing table the **start_dt** is **2007** and the **end_dt** is  **2011** the price is **12** but in next row the **start_dt** is **2008** and **end_dt** is **2012** the price was **14** which is not correct because if you take **2010** year it was **12** at the same time **14**  so it is bed to have overlapping between years. ***Each Record must has a Start Date !!***

2. Derive the End Date from the Start Date —> **END DATE = Start Date of the NEXT Record - 1**

    ![Screenshot 2025-03-03 220321.png](Screenshot_2025-03-03_220321.png)


So, Here we will go with the 2nd solution in order to resolve the issue.

It’s always the best practice to see into the ID or row with the issue is occurring in order to building the logic like in above situation let’s take look into two prd_key

```sql
SELECT
	 prd_id
	,prd_key
	,prd_nm
	,prd_cost
	,prd_line
	,prd_start_dt
	,prd_end_dt
FROM bronze.crm_prd_info
WHERE prd_key IN ('AC-HE-HL-U509-R', 'AC-HE-HL-U509');
```

here is the output:

![Screenshot 2025-03-03 221434.png](Screenshot_2025-03-03_221434.png)

So, our logic is we want to next record prd_start_dt to be the prd_end_dt - 1 we will use it Window function which in this case LEAD ()

```sql
SELECT
	 prd_id
	,prd_key
	,prd_nm
	,prd_cost
	,prd_line
	,CAST(prd_start_dt AS DATE) AS prd_start_dt
	,CAST(prd_end_dt AS DATE) AS prd_end_dt
	,CAST(LEAD(prd_start_dt) OVER (PARTITION BY prd_key ORDER BY prd_start_dt)-1 AS DATE) AS prd_end_dt_test
FROM bronze.crm_prd_info
WHERE prd_key IN ('AC-HE-HL-U509-R', 'AC-HE-HL-U509');
```

![Screenshot 2025-03-03 222922.png](Screenshot_2025-03-03_222922.png)

Let’s add this code into our main code:

```sql
SELECT
	 prd_id
	,prd_key
	,REPLACE(SUBSTRING(prd_key, 1, 5), '-', '_') AS cat_id
	,SUBSTRING(prd_key,7, LEN(prd_key)) AS prd_key
	,prd_nm
	,ISNULL(prd_cost, 0) AS prd_cost
	,CASE UPPER(TRIM(prd_line))
		  WHEN 'M' THEN 'Mountain'
		  WHEN 'R' THEN 'Road'
		  WHEN 'S' THEN 'Other Sales'
		  WHEN 'T' THEN 'Touring'
		  ELSE 'n/a'
	 END AS prd_line
	,CAST(prd_start_dt AS DATE) AS prd_start_dt
	,CAST(LEAD(prd_start_dt) OVER (PARTITION BY prd_key ORDER BY prd_start_dt)-1 AS DATE) AS prd_end_dt
FROM bronze.crm_prd_info
```

Now as we changed the prd_start_dt & prd_end_dt into DATE format and also added the new cat_id column so we have to re create our silver.crm_prd_info TABLE.

```sql
IF OBJECT_ID ('silver.crm_prd_info', 'U') IS NOT NULL
	DROP TABLE silver.crm_prd_info;

CREATE TABLE silver.crm_prd_info (
	prd_id			INT,
	cat_id			NVARCHAR(50),  -- This one we have added
	prd_key			NVARCHAR(50),
	prd_nm			NVARCHAR(50),
	prd_cost		INT,
	prd_line		NVARCHAR(50),
	prd_start_dt	DATE,  -- CAST DATETIME to DATE
	prd_end_dt		DATE,  -- CAST DATETIME to DATE
	dwh_create_date DATETIME2 DEFAULT GETDATE()

);
```

Let’s Insert the clean crm_prd_info table into our silver layer.

```sql
INSERT INTO silver.crm_prd_info (
	prd_id,
	cat_id,
	prd_key,
	prd_nm,
	prd_cost,
	prd_line,
	prd_start_dt,
	prd_end_dt
	)

SELECT
	 prd_id
	,REPLACE(SUBSTRING(prd_key, 1, 5), '-', '_') AS cat_id  -- Extract category ID
	,SUBSTRING(prd_key,7, LEN(prd_key)) AS prd_key          -- Extract product key
	,prd_nm
	,ISNULL(prd_cost, 0) AS prd_cost
	,CASE UPPER(TRIM(prd_line))
		  WHEN 'M' THEN 'Mountain'
		  WHEN 'R' THEN 'Road'
		  WHEN 'S' THEN 'Other Sales'
		  WHEN 'T' THEN 'Touring'
		  ELSE 'n/a'
	 END AS prd_line  -- Map product line codes to descriptive values
	,CAST(prd_start_dt AS DATE) AS prd_start_dt
	,CAST(LEAD(prd_start_dt) OVER (PARTITION BY prd_key ORDER BY prd_start_dt)-1 AS DATE) AS prd_end_dt  -- Calculate end date as one day before the next start date.
FROM bronze.crm_prd_info;

```

---

## Build Silver Layer Clean & Load crm_sales_details

Let’s first see the table

```sql
SELECT sls_ord_num
      ,sls_prd_key
      ,sls_cust_id
      ,sls_order_dt
      ,sls_ship_dt
      ,sls_due_dt
      ,sls_sales
      ,sls_quantity
      ,sls_price
FROM bronze.crm_sales_details

```

![Screenshot 2025-03-04 175952.png](Screenshot_2025-03-04_175952.png)

Let’s take first column sls_ord_num check for unwanted spaces

Now move one to sls_prd_key & sls_cust_id columns So here these column we will need to connect with **crm_prd_info** through **prd_key** and **crm_cust_info** table with cst_id

So, we need to check by writing

```sql
SELECT sls_ord_num
      ,sls_prd_key
      ,sls_cust_id
      ,sls_order_dt
      ,sls_ship_dt
      ,sls_due_dt
      ,sls_sales
      ,sls_quantity
      ,sls_price
FROM bronze.crm_sales_details
WHERE sls_prd_key NOT IN (SELECT prd_key FROM silver.crm_prd_info)

-- Check for crm_cust_info table
SELECT sls_ord_num
      ,sls_prd_key
      ,sls_cust_id
      ,sls_order_dt
      ,sls_ship_dt
      ,sls_due_dt
      ,sls_sales
      ,sls_quantity
      ,sls_price
FROM bronze.crm_sales_details
WHERE sls_cust_id NOT IN (SELECT cust_id FROM silver.crm_cust_info)
```

So, first three columns are perfect the next three date columns are not in date format.

Here we have to convert the date from Integer —> Date (*Note: Negative numbers or zeros can’t be cast to a date*)

We can check by writing below script

```sql
-- Check for Invalid Dates
SELECT
	sls_order_dt
FROM bronze.crm_sales_details
WHERE sls_order_dt <= 0
```

![Screenshot 2025-03-04 181028.png](Screenshot_2025-03-04_181028.png)

We can see we do have total 17 rows where where 0 are there which is not good but we can handle this issue by using **NULLIF( )** function

NULLIF() —> Return NULL if two given values are equal; otherwise, it returns the first expression.

Here is the code to convert 0 value to NULL

```sql
SELECT
	NULLIF(sls_order_dt,0) sls_order_dt
FROM bronze.crm_sales_details
WHERE sls_order_dt <=0
```

Now let’s see the value

![Screenshot 2025-03-04 181355.png](Screenshot_2025-03-04_181355.png)

We can see the value are in integer format which we have to convert it to date format

![Screenshot 2025-03-04 182227.png](Screenshot_2025-03-04_182227.png)

In this scenario, the length of the date must be 8 more then length 8 is not a valid value let’s check

```sql
SELECT
	NULLIF(sls_order_dt,0) sls_order_dt
FROM bronze.crm_sales_details
WHERE sls_order_dt <=0 OR LEN(sls_order_dt) != 8
```

![Screenshot 2025-03-04 182914.png](Screenshot_2025-03-04_182914.png)

Check for outliers by validating the boundaries of the date range

Here are all checks that I have checked

```sql
SELECT
	NULLIF(sls_order_dt,0) sls_order_dt
FROM bronze.crm_sales_details
WHERE  sls_order_dt <= 0
	OR LEN(sls_order_dt) != 8
	OR sls_order_dt > 20500101
	OR sls_order_dt < 19000101
```

So, below are the steps we need to follow

1. Convert all 0 or the value which not equal to 8 to NULL
2. Typecasting Integer ——> Date format but in SQL we can’t cast Integer —> Date first we have to convert integer —> varchar —> Date

Here is the script

```sql
SELECT sls_ord_num
      ,sls_prd_key
      ,sls_cust_id
	    ,CASE WHEN sls_order_dt = 0 OR LEN(sls_order_dt) != 8 THEN NULL
			      ELSE CAST(CAST(sls_order_dt AS VARCHAR) AS DATE)
	     END AS sls_order_dt
      ,sls_ship_dt
      ,sls_due_dt
      ,sls_sales
      ,sls_quantity
      ,sls_price
FROM bronze.crm_sales_details
```

![Screenshot 2025-03-04 184728.png](Screenshot_2025-03-04_184728.png)

As you can see our sls_order_dt data which was integer type has been converted to date format

Similarly we can convert sls_ship_dt & sls_due_dt  value to Date format

```sql
SELECT sls_ord_num
      ,sls_prd_key
      ,sls_cust_id
	    ,CASE WHEN sls_order_dt = 0 OR LEN(sls_order_dt) != 8 THEN NULL
			      ELSE CAST(CAST(sls_order_dt AS VARCHAR) AS DATE)
	     END AS sls_order_dt
      ,CASE WHEN sls_ship_dt= 0 OR LEN(sls_ship_dt) != 8 THEN NULL
			      ELSE CAST(CAST(sls_ship_dtAS VARCHAR) AS DATE)
	     END AS sls_ship_dt
      ,CASE WHEN sls_due_dt= 0 OR LEN(sls_due_dt) != 8 THEN NULL
			      ELSE CAST(CAST(sls_due_dtAS VARCHAR) AS DATE)
	     END AS sls_due_dt
      ,sls_sales
      ,sls_quantity
      ,sls_price
FROM bronze.crm_sales_details
```

Now let’s think deeper with the date related columns like **Order Date must always be earlier than the shipping Data or Due Date**

Let’s check whether order_dt > ship_dt OR order_dt > due_dt

```sql
SELECT
	*
FROM bronze.crm_sales_details
WHERE sls_order_dt > sls_ship_dt OR sls_order_dt > sls_due_dt;
```

![Screenshot 2025-03-04 215505.png](Screenshot_2025-03-04_215505.png)

## Check Data Consistency: Between Sales, Quantity, & Price

- Sales = Quantity * Price
- Values must not be NULL, Zero or Negative

```sql
SELECT
	 sls_sales
	,sls_quantity
	,sls_price
FROM bronze.crm_sales_details
WHERE sls_sales != sls_quantity * sls_price
	 OR sls_sales IS NULL OR sls_quantity IS NULL OR sls_price IS NULL
	 OR sls_sales <=0 OR sls_quantity <=0 OR sls_price <=0
ORDER BY sls_sales, sls_quantity, sls_price
```

![Screenshot 2025-03-04 220457.png](Screenshot_2025-03-04_220457.png)

Above we can see in our database the sls_sales have NULL values and also the negative value and the Sales value is not correct because sales should come by multiply Quantity * Price

Solutions:

1. Data Issues will be fixed direct in source system
2. Data issues has to be fixed in data warehouse

Rules:

- If Sales is negative, zero, or null, derive it using Quantity and Price.
- If Price is zero or null, calculate it using Sales & Quantity.
- If Price is negative, convert it to a positive value.

```sql
SELECT
	 sls_sales AS old_sls_sales
	,sls_quantity
	,sls_price AS old_sls_price

	,CASE WHEN sls_sales IS NULL OR sls_sales <=0 OR sls_sales != sls_quantity * ABS(sls_price)
		  THEN sls_quantity * ABS(sls_price)
		  ELSE sls_sales
	 END AS new_sls_sales

	,CASE WHEN sls_price IS NULL OR sls_price <=0
		  THEN sls_sales / NULLIF(sls_quantity,0)
		  ELSE sls_price
	 END AS new_sls_price

FROM bronze.crm_sales_details
WHERE sls_sales != sls_quantity * sls_price
	OR sls_sales IS NULL OR sls_quantity IS NULL OR sls_price IS NULL
	OR sls_sales <=0 OR sls_quantity <=0 OR sls_price <=0
ORDER BY sls_sales, sls_quantity, sls_price;
```

![Screenshot 2025-03-04 222106.png](Screenshot_2025-03-04_222106.png)

We have done with cleaning the sales_data now we have to modify our column data types and insert into silver layer

```sql
INSERT INTO silver.crm_sales_details (
	sls_ord_num,
	sls_prd_key,
	sls_cust_id,
	sls_order_dt,
	sls_ship_dt,
	sls_due_dt,
	sls_sales,
	sls_quantity,
	sls_price
)

SELECT sls_ord_num
      ,sls_prd_key
      ,sls_cust_id
	  ,CASE WHEN sls_order_dt = 0 OR LEN(sls_order_dt) != 8 THEN NULL
			ELSE CAST(CAST(sls_order_dt AS VARCHAR) AS DATE)
	   END AS sls_order_dt
	  ,CASE WHEN sls_ship_dt = 0 OR LEN(sls_ship_dt) != 8 THEN NULL
			ELSE CAST(CAST(sls_ship_dt AS VARCHAR) AS DATE)
	   END AS sls_ship_dt
	  ,CASE WHEN sls_due_dt = 0 OR LEN(sls_due_dt) != 8 THEN NULL
			ELSE CAST(CAST(sls_due_dt AS VARCHAR) AS DATE)
	   END AS sls_due_dt
	  ,CASE WHEN sls_sales IS NULL OR sls_sales <=0 OR sls_sales != sls_quantity * ABS(sls_price)
		  THEN sls_quantity * ABS(sls_price)
		  ELSE sls_sales
	   END AS sls_sales
	  ,sls_quantity
	  ,CASE WHEN sls_price IS NULL OR sls_price <=0
		  THEN sls_sales / NULLIF(sls_quantity,0)
		  ELSE sls_price
	   END AS sls_price
FROM bronze.crm_sales_details

```

Now let’s see our final table which we have insert into silver.crm_sales_details

```sql
SELECT * FROM silver.crm_sales_details;
```

### Before Data Cleansing

![Screenshot 2025-03-04 224006.png](Screenshot_2025-03-04_224006.png)

### After Data Cleansing

![Screenshot 2025-03-04 223833.png](Screenshot_2025-03-04_223833.png)

---

---

# Build Silver Layer Clean & Load erp_cust_az12

```sql
SELECT
	 cid
	,bdate
	,gen
FROM bronze.erp_cust_az12
```

![Screenshot 2025-03-04 230457.png](Screenshot_2025-03-04_230457.png)

![Screenshot 2025-03-04 231340.png](Screenshot_2025-03-04_231340.png)

Here to remove ‘NAS’ from the starting to keep the ‘AW00011000’ we can use LIKE function.

```sql
SELECT
	 CASE WHEN cid LIKE 'NAS%' THEN SUBSTRING(cid,4, LEN(cid))
	      ELSE cid
	 END cid
	,bdate
	,gen
FROM bronze.erp_cust_az12
```

Now we can check whether there are any unmatched ID is there in crm_cust_info table

```sql
SELECT
	 CASE WHEN cid LIKE 'NAS%' THEN SUBSTRING(cid,4, LEN(cid))
	      ELSE cid
	 END cid
	,bdate
	,gen
FROM bronze.erp_cust_az12
WHERE CASE WHEN cid LIKE 'NAS%' THEN SUBSTRING(cid,4, LEN(cid)) ELSE cid
	 END NOT IN (
	 SELECT
		 DISTINCT cst_key
	 FROM silver.crm_cust_info
);
```

After checking we can see we don’t have any unmatched cst_key which is a good sign

Now we can also check that the birthday should don’t have dob in future years like 9999 which hasn’t come yet.

```sql
-- Identify Out-of-Range Dates

SELECT DISTINCT bdate
FROM bronze.erp_cust_az12
WHERE bdate < '1924-01-01' OR bdate > GETDATE()
```

Let’s also check Standardization & Consistency of gen column

```sql
-- Data Standardization & Consistency
SELECT DISTINCT gen
FROM bronze.erp_cust_az12

------------------------
OUTPUT:
	gen
1 NULL
2 F
3
4 Male
5 Female
6 M
```

Above out we can we our **gen** values are not in multiple option like we have **NULL, F, Male, Female, M, space**

So, we need to use CASE WHEN with TRIM() so whenever there will be (’F’ or ‘Female’) make it ‘Famale’ and same for ‘Male’

```sql
-- Data Standardization & Consistency
SELECT
	,CASE WHEN UPPER(TRIM(gen)) IN ('F', 'FEMALE') THEN 'Female'
		  WHEN UPPER(TRIM(gen)) IN ('M', 'MALE') THEN 'Male'
		  ELSE 'n/a'
	 END AS gen
FROM bronze.erp_cust_az12
```

Old gen_table     —————————————————————————————————>

### gen

NULL

F

Male

Female

M

New gen_table after standardization

### gen

n/a

Female

n/a

Male

Female

Male

Now we have finished cleaning the erp_cust_az12 table now let’s insert into silver layer

```sql
INSERT INTO silver.erp_cust_az12 (
	cid,
	bdate,
	gen
)

SELECT
	 CASE WHEN cid LIKE 'NAS%' THEN SUBSTRING(cid,4, LEN(cid))    -- Remove 'NAS' prefix if present
	      ELSE cid
	 END cid
	,CASE WHEN bdate > GETDATE() THEN NULL
		  ELSE bdate
	 END AS bdate  -- Set future birthdates to NULL
	,CASE WHEN UPPER(TRIM(gen)) IN ('F', 'FEMALE') THEN 'Female'
		  WHEN UPPER(TRIM(gen)) IN ('M', 'MALE') THEN 'Male'
		  ELSE 'n/a'  -- Handle the missing values
	 END AS gen  -- Normalize gender values and handle unknown cases
FROM bronze.erp_cust_az12;

```

Also do the Quality Check of the Silver Table

---

# Build Silver Layer Clean & Load erp_loc_a101

So first lets see the table

```sql
SELECT
	cid,
	cntry
FROM bronze.erp_loc_a101;
```

![Screenshot 2025-03-05 181232.png](Screenshot_2025-03-05_181232.png)

Let’s see the other table with which we can join the table

```sql
SELECT
	cst_key
FROM silver.crm_cust_info;
```

![Screenshot 2025-03-05 181500.png](Screenshot_2025-03-05_181500.png)

We can see in the crm_cust_info table the cst_key column didn’t separated by ‘’-” but in the erp_loc_a101 table the cid have separated by “-” for an example
in cid → AW-00011015 but in cst_key —> AW00011015 so we need to remove “-” from cid column value

We are going to use **REPLACE() to replace “-” with “”**

```sql
SELECT
	 REPLACE(cid, '-', '') AS cid
	,cntry
FROM bronze.erp_loc_a101
```

After it gets replaced we can check whether we could merge the table with silver.crm_cust_info table using cst_key column. I have checked it did work.

```sql
SELECT
	 REPLACE(cid, '-','') AS cid
	,cntry
FROM bronze.erp_loc_a101
WHERE REPLACE(cid, '-','') NOT IN (
SELECT cst_key FROM silver.crm_cust_info
)
```

Now, after cid column is fixed let’s move on to cntry col let’s check first Unique value in the cntry

```sql
SELECT DISTINCT cntry
FROM bronze.erp_loc_a101
ORDER BY cntry

```

![Screenshot 2025-03-05 182434.png](Screenshot_2025-03-05_182434.png)

Above we can we do have lot of unique values which is not good for Data standardization & Consistency

Let’s handle this using CASE WHEN

```sql
--- Using below CASE WHEN we can handle our issue
CASE WHEN TRIM(cntry) = 'DE' THEN 'Germany'
		  WHEN TRIM(cntry) IN ('US', 'USA') THEN 'United States'
		  WHEN TRIM(cntry)= '' OR cntry IS NULL THEN 'n/a'
		  ELSE cntry
	 END AS cntry

--- FULL code
SELECT
	 REPLACE(cid, '-','') AS cid
	,cntry
	,CASE WHEN TRIM(cntry) = 'DE' THEN 'Germany'
		  WHEN TRIM(cntry) IN ('US', 'USA') THEN 'United States'
		  WHEN TRIM(cntry)= '' OR cntry IS NULL THEN 'n/a'
		  ELSE cntry
	 END AS cntry
FROM bronze.erp_loc_a101

```

### Now we have done with this table let’s insert to silver layer

```sql
INSERT INTO silver.erp_loc_a101 (
	cid,
	cntry
)
SELECT
	 REPLACE(cid, '-','') AS cid
	,CASE WHEN TRIM(cntry) = 'DE' THEN 'Germany'
		  WHEN TRIM(cntry) IN ('US', 'USA') THEN 'United States'
		  WHEN TRIM(cntry)= '' OR cntry IS NULL THEN 'n/a'
		  ELSE cntry
	 END AS cntry
FROM bronze.erp_loc_a101
```

let’s recheck for Quality purpose.

---

# Build Silver Layer Clean & Load erp_ex_cat_g1v2

So first lets see the table

```sql
SELECT
	id,
	cat,
	subcat,
	maintenance
FROM bronze.erp_px_cat_g1v2
```

![Screenshot 2025-03-05 184759.png](Screenshot_2025-03-05_184759.png)

you can check the data quality using below code.

```sql

-- Check for unwanted spaces
SELECT * FROM bronze.erp_px_cat_g1v2
WHERE cat != TRIM(cat) OR subcat != TRIM(subcat) OR maintenance != TRIM(maintenance)

-- Data Standardization & Consistency
SELECT DISTINCT
subcat
FROM bronze.erp_px_cat_g1v2

SELECT DISTINCT
maintenance
FROM bronze.erp_px_cat_g1v2
```

I have everything is good so only thing is left to insert into silver layer

```sql
INSERT INTO silver.erp_px_cat_g1v2 (
	id,
	cat,
	subcat,
	maintenance
)
SELECT
	id,
	cat,
	subcat,
	maintenance
FROM bronze.erp_px_cat_g1v2

```

### Using below you can check the Quality check for your silver layer.

```sql
/*
===============================================================================
Quality Checks
===============================================================================
Script Purpose:
    This script performs various quality checks for data consistency, accuracy,
    and standardization across the 'silver' layer. It includes checks for:
    - Null or duplicate primary keys.
    - Unwanted spaces in string fields.
    - Data standardization and consistency.
    - Invalid date ranges and orders.
    - Data consistency between related fields.

Usage Notes:
    - Run these checks after data loading Silver Layer.
    - Investigate and resolve any discrepancies found during the checks.
===============================================================================
*/

-- ====================================================================
-- Checking 'silver.crm_cust_info'
-- ====================================================================
-- Check for NULLs or Duplicates in Primary Key
-- Expectation: No Results
SELECT
    cst_id,
    COUNT(*)
FROM silver.crm_cust_info
GROUP BY cst_id
HAVING COUNT(*) > 1 OR cst_id IS NULL;

-- Check for Unwanted Spaces
-- Expectation: No Results
SELECT
    cst_key
FROM silver.crm_cust_info
WHERE cst_key != TRIM(cst_key);

-- Data Standardization & Consistency
SELECT DISTINCT
    cst_marital_status
FROM silver.crm_cust_info;

-- ====================================================================
-- Checking 'silver.crm_prd_info'
-- ====================================================================
-- Check for NULLs or Duplicates in Primary Key
-- Expectation: No Results
SELECT
    prd_id,
    COUNT(*)
FROM silver.crm_prd_info
GROUP BY prd_id
HAVING COUNT(*) > 1 OR prd_id IS NULL;

-- Check for Unwanted Spaces
-- Expectation: No Results
SELECT
    prd_nm
FROM silver.crm_prd_info
WHERE prd_nm != TRIM(prd_nm);

-- Check for NULLs or Negative Values in Cost
-- Expectation: No Results
SELECT
    prd_cost
FROM silver.crm_prd_info
WHERE prd_cost < 0 OR prd_cost IS NULL;

-- Data Standardization & Consistency
SELECT DISTINCT
    prd_line
FROM silver.crm_prd_info;

-- Check for Invalid Date Orders (Start Date > End Date)
-- Expectation: No Results
SELECT
    *
FROM silver.crm_prd_info
WHERE prd_end_dt < prd_start_dt;

-- ====================================================================
-- Checking 'silver.crm_sales_details'
-- ====================================================================
-- Check for Invalid Dates
-- Expectation: No Invalid Dates
SELECT
    NULLIF(sls_due_dt, 0) AS sls_due_dt
FROM bronze.crm_sales_details
WHERE sls_due_dt <= 0
    OR LEN(sls_due_dt) != 8
    OR sls_due_dt > 20500101
    OR sls_due_dt < 19000101;

-- Check for Invalid Date Orders (Order Date > Shipping/Due Dates)
-- Expectation: No Results
SELECT
    *
FROM silver.crm_sales_details
WHERE sls_order_dt > sls_ship_dt
   OR sls_order_dt > sls_due_dt;

-- Check Data Consistency: Sales = Quantity * Price
-- Expectation: No Results
SELECT DISTINCT
    sls_sales,
    sls_quantity,
    sls_price
FROM silver.crm_sales_details
WHERE sls_sales != sls_quantity * sls_price
   OR sls_sales IS NULL
   OR sls_quantity IS NULL
   OR sls_price IS NULL
   OR sls_sales <= 0
   OR sls_quantity <= 0
   OR sls_price <= 0
ORDER BY sls_sales, sls_quantity, sls_price;

-- ====================================================================
-- Checking 'silver.erp_cust_az12'
-- ====================================================================
-- Identify Out-of-Range Dates
-- Expectation: Birthdates between 1924-01-01 and Today
SELECT DISTINCT
    bdate
FROM silver.erp_cust_az12
WHERE bdate < '1924-01-01'
   OR bdate > GETDATE();

-- Data Standardization & Consistency
SELECT DISTINCT
    gen
FROM silver.erp_cust_az12;

-- ====================================================================
-- Checking 'silver.erp_loc_a101'
-- ====================================================================
-- Data Standardization & Consistency
SELECT DISTINCT
    cntry
FROM silver.erp_loc_a101
ORDER BY cntry;

-- ====================================================================
-- Checking 'silver.erp_px_cat_g1v2'
-- ====================================================================
-- Check for Unwanted Spaces
-- Expectation: No Results
SELECT
    *
FROM silver.erp_px_cat_g1v2
WHERE cat != TRIM(cat)
   OR subcat != TRIM(subcat)
   OR maintenance != TRIM(maintenance);

-- Data Standardization & Consistency
SELECT DISTINCT
    maintenance
FROM silver.erp_px_cat_g1v2;
```

---

---

# `Build Gold Layer`

## `Create Dimension Customers`

![dimension_fact.drawio.png](dimension_fact.drawio.png)

## **Explore the Business Objects**

![data_integration.png](data_integration.png)

[](https://www.notion.so)

### Let’s build first customer table

So, we need **crm_cust_info**, **erp_cust_az12** & **erp_loc_a101** tables in order to make it one customer table.

First we see with which columns we can join with other table, here we can join **crm_cust_info** table with **erp_cust_az12** on **cst_id = cid** column and same **crm_cust_info** with **erp_loc_a101** on **cst_id = cid.**

![joings.drawio.png](joings.drawio.png)

Let’s write a script to apply LEFT JOIN on tables:

```sql
SELECT
	ci.cst_id,
	ci.cst_key,
	ci.cst_firstname,
	ci.cst_lastname,
	ci.cst_marital_status,
	ci.cst_gndr,
	ci.cst_create_date,
	ca.bdate,
	ca.gen,
	la.cntry
FROM silver.crm_cust_info AS ci
LEFT JOIN silver.erp_cust_az12 AS ca
	ON ci.cst_key = ca.cid
LEFT JOIN silver.erp_loc_a101 AS la
	ON ci.cst_key = la.cid

```

![Screenshot 2025-03-05 220003.png](Screenshot_2025-03-05_220003.png)

> **TIP:  After Joining table, check if any duplicates were introduced by the join logic**
>

Here how we can check the duplicates

```sql

SELECT
	cst_id,
	COUNT(*)
FROM (
	SELECT
		ci.cst_id,
		ci.cst_key,
		ci.cst_firstname,
		ci.cst_lastname,
		ci.cst_marital_status,
		ci.cst_gndr,
		ci.cst_create_date,
		ca.bdate,
		ca.gen,
		la.cntry
	FROM silver.crm_cust_info AS ci
	LEFT JOIN silver.erp_cust_az12 AS ca
		ON ci.cst_key = ca.cid
	LEFT JOIN silver.erp_loc_a101 AS la
		ON ci.cst_key = la.cid
) t
GROUP BY cst_id
HAVING COUNT(*) > 1

-- We don't get any duplicates dataset which is a good thing.

```

Now, Let’s check the Gender as I can we have two columns related to customer gender let’s look into it.

```sql
SELECT DISTINCT
	ci.cst_gndr,
	ca.gen
FROM silver.crm_cust_info AS ci
	LEFT JOIN silver.erp_cust_az12 AS ca
	ON ci.cst_key = ca.cid
	LEFT JOIN silver.erp_loc_a101 AS la
	ON ci.cst_key = la.cid
ORDER 1,2
```

![Screenshot 2025-03-05 220437.png](Screenshot_2025-03-05_220437.png)

As we can see sometimes we have different values and sometime NULL or n/a value and above NULL we got it from the Join table NULLs often come from joined tables! NULL will appear if SQL finds no match.

```sql
SELECT DISTINCT
	ci.cst_gndr,
	CASE WHEN ci.cst_gndr != 'n/a' THEN ci.cst_gndr
		 ELSE COALESCE(ca.gen, 'n/a')
	END AS new_gndr
FROM silver.crm_cust_info AS ci
	LEFT JOIN silver.erp_cust_az12 AS ca
	ON ci.cst_key = ca.cid
	LEFT JOIN silver.erp_loc_a101 AS la
	ON ci.cst_key = la.cid
```

Now let’s look into our new_gen col

![image.png](image.png)

### Now Rename columns to friendly, meaningful names

**cst_id    —> customer_id**

**cst_key —> customer_number**

**cst_firstname —> fist_name**

**cst_lastname —> last_name**

**cntry —> country**

**cst_marital_status —> marital_status**

**bdate —> birth_date**

**cst_create_date —> create_date**

### General Principles:

- **Naming Conventions: Use snake_case, with lowercase letters and underscores(_) to separate words.**
- **Language: Use English for all names.**
- **Avoid Reserved Words: Do not use SQL reserved words as object names.**

### Sort the columns into logical groups to improve readability.

```sql
SELECT
	ci.cst_key AS customer_number,
	ci.cst_firstname AS first_name,
	ci.cst_lastname AS last_name,
	la.cntry AS country,
	ci.cst_marital_status AS marital_status,
	CASE WHEN ci.cst_gndr != 'n/a' THEN ci.cst_gndr
		 ELSE COALESCE(ca.gen, 'n/a')
	END AS gender,
	ca.bdate AS birthdate,
	ci.cst_create_date AS create_date
FROM silver.crm_cust_info AS ci
	LEFT JOIN silver.erp_cust_az12 AS ca
	ON ci.cst_key = ca.cid
	LEFT JOIN silver.erp_loc_a101 AS la
	ON ci.cst_key = la.cid
```

![Screenshot 2025-03-05 225118.png](Screenshot_2025-03-05_225118.png)

Let’s add **`Surrogate Key`**

System-generated unique identifier , assigned to each record in a table.

- DDL-based generation.
- Query-based using Window function (Row_Number)

### Let’s create a view in gold layer

```sql
CREATE VIEW gold.dim_customers AS
SELECT
    ROW_NUMBER() OVER (ORDER BY cst_id) AS customer_key,
	ci.cst_id AS customer_id,
	ci.cst_key AS customer_number,
	ci.cst_firstname AS first_name,
	ci.cst_lastname AS last_name,
	la.cntry AS country,
	ci.cst_marital_status AS marital_status,
	CASE WHEN ci.cst_gndr != 'n/a' THEN ci.cst_gndr
		 ELSE COALESCE(ca.gen, 'n/a')
	END AS gender,
	ca.bdate AS birthdate,
	ci.cst_create_date AS create_date
FROM silver.crm_cust_info AS ci
	LEFT JOIN silver.erp_cust_az12 AS ca
	ON ci.cst_key = ca.cid
	LEFT JOIN silver.erp_loc_a101 AS la
	ON ci.cst_key = la.cid

```

### We have created successfully created the view table for dim_customer

---

# `Build Gold Layer Create Dimension Products`

So, first we need to see which column are require in order to join the tables.

We need **cat_id** and **id** in order to join the table



```sql
SELECT
	 pn.prd_id
	,pn.cat_id
	,pn.prd_key
	,pn.prd_nm
	,pn.prd_cost
	,pn.prd_line
	,pn.prd_start_dt
	,pc.cat
	,pc.subcat
	,pc.maintenance
FROM silver.crm_prd_info AS pn
LEFT JOIN silver.erp_px_cat_g1v2 AS pc
ON	 pn.cat_id = pc.id
WHERE prd_end_dt IS NULL  --Filter out all historical date

```

![Screenshot 2025-03-06 183819.png](Screenshot_2025-03-06_183819.png)

### Sort the columns into logical groups to improve readability

```sql
SELECT
	 pn.prd_id
	,pn.prd_key
	,pn.prd_nm
	,pn.cat_id
	,pc.cat
	,pc.subcat
	,pc.maintenance
	,pn.prd_cost
	,pn.prd_line
	,pn.prd_start_dt
FROM silver.crm_prd_info AS pn
LEFT JOIN silver.erp_px_cat_g1v2 AS pc
ON	 pn.cat_id = pc.id
WHERE prd_end_dt IS NULL  --Filter out all historical date

```

### Rename columns to friendly, meaningful names

```sql
SELECT
	 pn.prd_id AS product_id
	,pn.prd_key AS product_number
	,pn.prd_nm AS product_name
	,pn.cat_id AS category_id
	,pc.cat AS category
	,pc.subcat AS subcategory
	,pc.maintenance
	,pn.prd_cost AS cost
	,pn.prd_line AS product_line
	,pn.prd_start_dt AS start_date
FROM silver.crm_prd_info AS pn
LEFT JOIN silver.erp_px_cat_g1v2 AS pc
ON	 pn.cat_id = pc.id
WHERE prd_end_dt IS NULL  --Filter out all historical date
```

## `🎏 Dimension v/s Fact ??`

In this case we all have columns which help it describing the products. So, it is dimension not a Fact

Now we have create new primary key for it.

`ROW_NUMBER() OVER (ORDER BY pn.prd_start_dt, pn_prd_key) AS product_key`

```sql
CREATE VIEW gold.dim_products AS
SELECT
   ROW_NUMBER() OVER (ORDER BY pn.prd_start_dt, pn.prd_key) AS product_key,
	 pn.prd_id AS product_id
	,pn.prd_key AS product_number
	,pn.prd_nm AS product_name
	,pn.cat_id AS category_id
	,pc.cat AS category
	,pc.subcat AS subcategory
	,pc.maintenance
	,pn.prd_cost AS cost
	,pn.prd_line AS product_line
	,pn.prd_start_dt AS start_date
FROM silver.crm_prd_info AS pn
LEFT JOIN silver.erp_px_cat_g1v2 AS pc
ON	 pn.cat_id = pc.id
WHERE prd_end_dt IS NULL  --Filter out all historical date

```

# `Build Gold **Layer** Create Facts Sales`

For sales we have only one table

```sql
SELECT
	 sd.sls_ord_num
	,sd.sls_prd_key
	,sd.sls_cust_id
	,sd.sls_order_dt
	,sd.sls_ship_dt
	,sd.sls_due_dt
	,sd.sls_sales
	,sd.sls_quantity
	,sd.sls_price
	FROM silver.crm_sales_details AS sd
```

Do we have here Dimension vs Fact??

It is a Fact

We have lot of ids to connect with other table so here we will use those surrogate keys instead of IDs to easily connect facts with dimensions

So this step is called **VLOOKUP**

Now, we have to JOIN the crm_sales_details table with both dimension tables and replace the key/id with our surrogate keys.

```sql
SELECT
	 sd.sls_ord_num
	,pr.product_key  -- Dimension Keys
	,cu.customer_key  -- Dimension Keys
	,sd.sls_order_dt
	,sd.sls_ship_dt
	,sd.sls_due_dt
	,sd.sls_sales
	,sd.sls_quantity
	,sd.sls_price
FROM silver.crm_sales_details AS sd
LEFT JOIN gold.dim_products AS pr
ON sd.sls_prd_key = pr.product_number
LEFT JOIN gold.dim_customers AS cu
ON sd.sls_cust_id = cu.customer_id
```

Now rename columns to friendly, meaningful names and also sort the columns into logical groups to improve readability

```sql
SELECT
	 sd.sls_ord_num AS order_number
	,pr.product_key
	,cu.customer_key
	,sd.sls_order_dt AS order_date
	,sd.sls_ship_dt AS shipping_date
	,sd.sls_due_dt AS due_date
	,sd.sls_sales AS sales_amount
	,sd.sls_quantity AS quantity
	,sd.sls_price AS price
FROM silver.crm_sales_details AS sd
LEFT JOIN gold.dim_products AS pr
ON sd.sls_prd_key = pr.product_number
LEFT JOIN gold.dim_customers AS cu
ON sd.sls_cust_id = cu.customer_id
```

Now we can build it to gold.fact_sales

```sql
CREATE VIEW gold.fact_sales AS
SELECT
	 sd.sls_ord_num AS order_number
	,pr.product_key
	,cu.customer_key
	,sd.sls_order_dt AS order_date
	,sd.sls_ship_dt AS shipping_date
	,sd.sls_due_dt AS due_date
	,sd.sls_sales AS sales_amount
	,sd.sls_quantity AS quantity
	,sd.sls_price AS price
FROM silver.crm_sales_details AS sd
LEFT JOIN gold.dim_products AS pr
ON sd.sls_prd_key = pr.product_number
LEFT JOIN gold.dim_customers AS cu
ON sd.sls_cust_id = cu.customer_id
```

## |Fact Check: Check if all dimension tables can successfully join to the fact table

```sql
select * from gold.fact_sales f
LEFT JOIN gold.dim_customers c
ON c.customer_key = f.customer_key
LEFT JOIN gold.dim_products p
ON p.product_key = f.product_key
WHERE c.customer_key IS NULL
```

## Data Model of Start Schema

Relationship

![data_model.png](data_model.png)

# Build Gold Layer Data Catalog

# Data Catalog for Gold Layer

## Overview

The Gold Layer is the business-level data representation, structured to support analytical and reporting use cases. It consists of **dimension tables** and **fact tables** for specific business metrics.

---

### 1. **gold.dim_customers**

- **Purpose:** Stores customer details enriched with demographic and geographic data.
- **Columns:**

| Column Name | Data Type | Description |
| --- | --- | --- |
| customer_key | INT | Surrogate key uniquely identifying each customer record in the dimension table. |
| customer_id | INT | Unique numerical identifier assigned to each customer. |
| customer_number | NVARCHAR(50) | Alphanumeric identifier representing the customer, used for tracking and referencing. |
| first_name | NVARCHAR(50) | The customer's first name, as recorded in the system. |
| last_name | NVARCHAR(50) | The customer's last name or family name. |
| country | NVARCHAR(50) | The country of residence for the customer (e.g., 'Australia'). |
| marital_status | NVARCHAR(50) | The marital status of the customer (e.g., 'Married', 'Single'). |
| gender | NVARCHAR(50) | The gender of the customer (e.g., 'Male', 'Female', 'n/a'). |
| birthdate | DATE | The date of birth of the customer, formatted as YYYY-MM-DD (e.g., 1971-10-06). |
| create_date | DATE | The date and time when the customer record was created in the system |

---

### 2. **gold.dim_products**

- **Purpose:** Provides information about the products and their attributes.
- **Columns:**

| Column Name | Data Type | Description |
| --- | --- | --- |
| product_key | INT | Surrogate key uniquely identifying each product record in the product dimension table. |
| product_id | INT | A unique identifier assigned to the product for internal tracking and referencing. |
| product_number | NVARCHAR(50) | A structured alphanumeric code representing the product, often used for categorization or inventory. |
| product_name | NVARCHAR(50) | Descriptive name of the product, including key details such as type, color, and size. |
| category_id | NVARCHAR(50) | A unique identifier for the product's category, linking to its high-level classification. |
| category | NVARCHAR(50) | The broader classification of the product (e.g., Bikes, Components) to group related items. |
| subcategory | NVARCHAR(50) | A more detailed classification of the product within the category, such as product type. |
| maintenance_required | NVARCHAR(50) | Indicates whether the product requires maintenance (e.g., 'Yes', 'No'). |
| cost | INT | The cost or base price of the product, measured in monetary units. |
| product_line | NVARCHAR(50) | The specific product line or series to which the product belongs (e.g., Road, Mountain). |
| start_date | DATE | The date when the product became available for sale or use, stored in |

---

### 3. **gold.fact_sales**

- **Purpose:** Stores transactional sales data for analytical purposes.
- **Columns:**

| Column Name | Data Type | Description |
| --- | --- | --- |
| order_number | NVARCHAR(50) | A unique alphanumeric identifier for each sales order (e.g., 'SO54496'). |
| product_key | INT | Surrogate key linking the order to the product dimension table. |
| customer_key | INT | Surrogate key linking the order to the customer dimension table. |
| order_date | DATE | The date when the order was placed. |
| shipping_date | DATE | The date when the order was shipped to the customer. |
| due_date | DATE | The date when the order payment was due. |
| sales_amount | INT | The total monetary value of the sale for the line item, in whole currency units (e.g., 25). |
| quantity | INT | The number of units of the product ordered for the line item (e.g., 1). |
| price | INT | The price per unit of the product for the line item, in whole currency units (e.g., 25). |

### Final Data Flow Diagram

![data_flow.png](data_flow.png)

---
