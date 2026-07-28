# azure-data-engineering-adventureworks1
End-to-end Azure Data Engineering project using Azure Data Factory, ADLS Gen2, Azure Databricks, Azure Synapse Analytics and Power BI.

# Azure Data Engineering Project – AdventureWorks

## Project Overview

This project demonstrates an end-to-end Azure Data Engineering solution using the AdventureWorks database.

The objective is to extract data from Azure SQL Database, ingest it into Azure Data Lake Storage, transform it using Azure Databricks, expose the curated data through Azure Synapse Analytics, and visualize business insights in Power BI.

# 1 Business Problem
## Company Background
AdventureWorks is a bicycle manufacturing and retail company.
The company sells bicycles, bicycle accessories and cycling equipment through multiple sales channels.
As the company expanded, different departments started using different operational systems.

For example:

The CRM system stores customer information.
The ERP system manages products and inventory.
The Sales system records customer orders.
Other operational systems manage different business activities.
Because the information is distributed across multiple systems, it is difficult for managers to obtain a complete view of the business.

## Business Challenges
The company faced several challenges:

Customer information was distributed across multiple systems.
Product information was stored separately from sales transactions.
Business reports required manual data extraction.
Decision-makers could not access real-time business insights.
Reporting processes were slow and error-prone.

## Business Goal
The objective of this project is to centralize operational data into a modern Azure Data Platform.
The platform enables reliable reporting, business analytics and executive dashboards.
# 2. Source Systems

## Overview

AdventureWorks operates multiple business systems, each designed to support a specific business function.
Instead of storing all business information in a single database, each department uses a specialized operational system.
The project integrates data from three main source systems.

### CRM System

The CRM (Customer Relationship Management) system stores customer-related information, including customer identity and contact details.

### ERP System

The ERP (Enterprise Resource Planning) system manages products, inventory, pricing and other operational data.

### Sales System

The Sales system records customer orders, purchased products, quantities and sales transactions.
Because these systems operate independently, business information is distributed across multiple databases. The role of the Data Engineering platform is to centralize these datasets into a unified analytical environment.

## 3 Architecture
<img width="1367" height="727" alt="Architecture" src="https://github.com/user-attachments/assets/d5ebf506-ca35-45ba-8645-53e53445d94e" />

## Business Requirement

The company needed a centralized data platform capable of collecting data from multiple operational systems, transforming it into trusted datasets and delivering business-ready information for reporting.

## Proposed Solution

The solution is built on Microsoft Azure and follows a modern Data Engineering architecture.

Each Azure service has a specific responsibility:

- Azure Data Factory orchestrates data ingestion.
- Azure Data Lake Storage stores raw and curated datasets.
- Azure Databricks performs data transformation using the Medallion Architecture.
- Azure Synapse Analytics exposes the curated data through SQL views.
- Power BI provides business dashboards for decision-makers.
- 
# 4. Data Pipeline
<img width="1902" height="911" alt="Screenshot 2026-07-01 082514" src="https://github.com/user-attachments/assets/221347ad-5622-44b5-b807-ebd1ddae6d03" />

To automate data ingestion, I implemented a **metadata-driven pipeline** using **Azure Data Factory (ADF)**.

Instead of creating one pipeline for each source table, the pipeline dynamically reads a configuration table (`ingestion_config`) and ingests all active tables into the Bronze layer of Azure Data Lake Storage Gen2.
This approach makes the solution scalable, reusable, and easy to maintain.
AdventureWorks stores business data in multiple operational systems, including CRM and ERP.

The company required an automated process to:

- Extract data from different source systems.
- Load the data into a centralized Data Lake.
- Support new source tables without redesigning the pipeline.
- Reduce manual work and simplify maintenance.
## Pipeline Workflow

The ingestion pipeline consists of three main activities.

### a. Lookup Activity

The Lookup activity reads the **ingestion_config** table.

Its purpose is to retrieve the list of source tables that must be ingested.

**Output:**
- Source schema
- Source table
- Destination folder
- Active flag

The Lookup activity does **not** copy business data. It only retrieves the pipeline configuration.

### b. ForEach Activity

The ForEach activity iterates through each row returned by the Lookup activity.
For every active table, it passes the metadata to the Copy Activity.
This design allows the same pipeline to ingest any number of source tables without modification.


### c. Copy Activity

The Copy Activity extracts data from the source system and writes it into the Bronze layer of Azure Data Lake Storage Gen2.
Each table is copied into its corresponding destination folder while preserving the raw data.

Example:

- crm.customer → Bronze/customer
- crm.product → Bronze/product
- crm.sales → Bronze/sales
- erp.customer → Bronze/erp_customer
- erp.location → Bronze/location
- erp.category → Bronze/category
# 5. Bronze Layer
## Azure Data Lake Integration

Azure Databricks was connected to Azure Data Lake Storage Gen2 to access the Bronze data.
Using the ABFSS protocol, Databricks was able to securely read the raw files, process them with Apache Spark, and write the transformed data into the Silver layer.
<img width="1166" height="482" alt="image" src="https://github.com/user-attachments/assets/f419a2b8-6c58-4a72-bcb0-47b2e74d97f3" />


## Overview
The Bronze layer is the landing zone of the Medallion Architecture.
Its purpose is to store raw data exactly as it is received from the source systems without applying any business transformation.
This layer acts as the single source of truth and preserves the original data for auditing, troubleshooting, and future reprocessing.
AdventureWorks receives operational data from multiple systems every day.

Before cleaning or transforming the data, the company needs to preserve an exact copy of the original records.
Keeping raw data allows engineers to:

- Recover from transformation errors.
- Reprocess historical data.
- Audit data quality.
- Compare transformed data with the original source.
- 
Characteristics of the Bronze Layer

- Stores raw data.
- Preserves original records.
- No joins.
- No filtering.
- No aggregation.
- No business transformation.
# 6. Silver Layer

## Overview

The Silver layer is responsible for transforming the raw data stored in the Bronze layer into clean, consistent, and reliable datasets.

At this stage, data quality rules are applied to remove inconsistencies, standardize formats, and integrate information from multiple source systems. The objective is to create trusted datasets that can be used for business modeling and analytics.

Unlike the Bronze layer, which preserves the original data, the Silver layer focuses on improving data quality while maintaining data integrity.
<img width="1312" height="695" alt="image" src="https://github.com/user-attachments/assets/2fa2cc14-0621-4312-92fe-1110abcd55f7" />

## Data Transformation

The Silver layer applies data quality and transformation rules to improve the usability of the raw datasets.

The main transformations include:

- Standardizing column names.
- Converting data types.
- Removing duplicate records.
- Handling missing or invalid values.
- Standardizing data formats.
- Integrating related data from CRM and ERP systems.
- Preparing datasets for dimensional modeling.
- 
# 7. Gold Layer

## Overview
The Gold layer represents the business-ready layer of the Medallion Architecture.
At this stage, the cleaned and standardized datasets from the Silver layer are transformed into analytical models that support business reporting and decision-making.
The Gold layer contains dimension tables and fact tables organized using a star schema, making the data easy to query and highly optimized for BI tools such as Power BI.
## Business Requirement
Business users are not interested in raw operational data.
Instead, they need reliable datasets that answer business questions such as:

- Who are our customers?
- Which products generate the most revenue?
- What are the monthly sales?
- Which countries have the highest sales?
To support these analyses, the data must be organized into dimensions and fact tables following a dimensional modeling approach.
## Data Modeling
The Gold layer follows a Star Schema design.
This model separates descriptive business information into Dimension tables while transactional data is stored in Fact tables.
This approach improves query performance and simplifies business reporting.
## Project Implementation
The following analytical tables were created in the Gold layer:
### Dimension Tables
- gold.dim_customer
  <img width="1257" height="815" alt="image" src="https://github.com/user-attachments/assets/529fc573-6984-4d37-9106-7badf84aacd1" />
  The Customer Dimension combines customer information collected from multiple source systems.
Customer demographic information, location details, and additional business attributes are integrated into a single business entity.
This provides a complete customer profile that can be reused across multiple reports.
- gold.dim_product
  <img width="1211" height="412" alt="image" src="https://github.com/user-attachments/assets/642864cd-dc1a-42cb-9ff0-8161847611cb" />

  The Product Dimension centralizes product information and enriches it with category details obtained from the ERP system.
This enables business users to analyze sales by product category instead of relying only on product identifiers.
### Fact Table
- gold.fact_sales
The Sales Fact table stores transactional sales data.
Each record represents a business transaction and references the Customer and Product dimensions through surrogate keys.
This structure allows Power BI to efficiently calculate KPIs such as:

- Total Sales
- Revenue by Product
- Revenue by Customer
- Monthly Sales Trends
