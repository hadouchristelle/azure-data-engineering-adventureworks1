# azure-data-engineering-adventureworks1
End-to-end Azure Data Engineering project using Azure Data Factory, ADLS Gen2, Azure Databricks, Azure Synapse Analytics and Power BI.

# Azure Data Engineering Project – AdventureWorks

## Project Overview

This project demonstrates an end-to-end Azure Data Engineering solution using the AdventureWorks database.

The objective is to extract data from Azure SQL Database, ingest it into Azure Data Lake Storage, transform it using Azure Databricks, expose the curated data through Azure Synapse Analytics, and visualize business insights in Power BI.

## Architecture

The project uses the following Azure services:

- Azure SQL Database
- Azure Data Factory
- Azure Data Lake Storage Gen2
- Azure Databricks
- Azure Synapse Analytics
- Power BI

## Data Architecture

The project follows the Medallion Architecture:

### Bronze Layer

The Bronze layer contains raw data extracted from the source system
### Silver Layer

The Silver layer contains cleaned and standardized data.

Transformations include:

- Removing duplicates
- Handling missing values
- Correcting data types
- Standardizing column names
- Validating business data
- Combining data from related source tables

### Gold Layer

The Gold layer contains business-ready tables used for analytics and reporting.

The main Gold tables are:

- `dim_customers`
- `dim_products`
- `fact_sales`
- `fact_orders`

## Data Flow
