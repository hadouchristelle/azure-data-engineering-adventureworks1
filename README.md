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


