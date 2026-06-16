---
title: "SQL Data Warehouse & Analytics"
subtitle: "End-to-End Data Engineering Project"
excerpt: "A fully architected data warehouse built with SQL Server — covering ingestion, transformation through Bronze/Silver/Gold layers, and analytics-ready dimensional modeling. Implements the Medallion Architecture with star schema design."
date: 2024-06-01
author: "Shivam Chhetry"
featured: true
draft: false
tags:
  - SQL
  - Data Engineering
  - Data Warehouse
  - Analytics
categories:
  - Data Engineering
  - SQL
layout: single
links:
- icon: github
  icon_pack: fab
  name: Code
  url: https://github.com/Shivamchhetry20/SQL-Data-Warehouse-project
---

## SQL Data Warehouse & Analytics Project

An end-to-end data warehouse and analytics solution built using **SQL Server**, following the **Medallion Architecture** (Bronze → Silver → Gold layers).

### Key Features

- **Bronze Layer**: Raw data ingestion from source systems (CRM and ERP datasets) into the warehouse with no transformations applied — preserving data lineage.
- **Silver Layer**: Data cleansing, type casting, deduplication, and standardization to create a reliable, consistent data foundation.
- **Gold Layer**: Business-ready dimensional model with fact and dimension tables (`dim_customers`, `dim_products`, `fact_sales`) built for analytical queries and reporting.

### Architecture

The project follows a **Star Schema** design within the Gold layer, enabling efficient analytical queries across:

- Customer demographics and segmentation
- Product catalog and categorization
- Sales transactions and performance metrics

### Tech Stack

- **T-SQL / SQL Server** for all transformation and modeling
- **Stored Procedures** for ETL pipeline orchestration
- **Views** for clean, abstracted access to Gold layer entities

This project demonstrates the full lifecycle of data from raw landing through analytics-ready consumption, mirroring real-world enterprise data warehouse patterns.
