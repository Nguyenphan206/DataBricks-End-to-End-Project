# DataBricks-End-to-End-Project

## Project Overview

This project demonstrates an End-to-End Data Engineering Pipeline built on Azure Databricks using the Medallion Architecture (Bronze → Silver → Gold).

The pipeline ingests raw data from Azure Data Lake Gen2, processes it through multiple transformation layers in Databricks, and models the curated data into a Star Schema (Dimensional Model) in the Gold layer.
The final curated data is exposed as External Tables for integration into Azure Synapse Analytics, making it ready for downstream analytics and reporting (e.g., Power BI).
⚠️ Note: Power BI reports are not implemented in this project — it is only shown in the architecture diagram for completeness.

## Architecture Overview
<img width="1227" height="683" alt="image" src="https://github.com/user-attachments/assets/fd4a3cd2-4146-4cac-b73b-69cc3f2518a1" />

**Flow Explanation:**
- Azure Data Lake Gen2 – Stores raw source files.
- Bronze Layer – Ingest raw data as-is into Delta tables using Databricks Autoloader.
- Silver Layer – Clean, deduplicate, and standardize data.
- Gold Layer – Aggregate and model data into Star Schema:


- Gold_Products built using Delta Live Tables.
- (Optional) Azure Synapse Analytics – Consumes Gold layer data via External Tables.
- (Optional) Power BI – Connects to Synapse for reporting and dashboards.
## Pipeline Flow in Databricks

The pipeline is orchestrated in Databricks Workflows with automatic backups to Azure Data Lake Gen2 at every stage.
**Pipeline Diagram:**

<img width="2239" height="766" alt="image" src="https://github.com/user-attachments/assets/6de8dc57-b9a2-4150-964b-c9b705e8967d" />

**Execution Order:**
- Parameters – Load global configs (storage paths, table names).
- Bronze_Autoloader_iteration – Incrementally ingest raw data from ADLS Gen2 → Bronze Delta tables.
- Silver_ERP – Standardize ERP source data.
- Silver_Customers / Silver_Products / Silver_Sales – Apply data cleansing, deduplication, and formatting.
- Gold_Customers / Gold_Products / Gold_Sales – Build final Star Schema tables.
- Gold_Products uses Delta Live Tables for declarative transformation.
- Publish to Synapse – Register Gold tables as External Tables for Azure Synapse.

## Technologies Used

- Azure Data Lake Gen2 – Raw & curated data storage
<img width="2542" height="711" alt="image" src="https://github.com/user-attachments/assets/6a4d486b-5579-4055-b263-01834f8b186f" />
- Azure Databricks – ETL processing, orchestration, Medallion Architecture
- PySpark – Data transformation & processing
- Delta Lake – ACID-compliant data storage
<img width="1894" height="566" alt="image" src="https://github.com/user-attachments/assets/268aa1a6-9ec7-45fb-add7-df8af874f442" />
- Delta Live Tables – Declarative data pipeline in Gold layer
<img width="2536" height="1263" alt="image" src="https://github.com/user-attachments/assets/78f81d53-f1d4-4d30-b3ef-2cf3e156784c" />
- Databricks Catalog – Registers all Delta tables in Unity Catalog for discoverability.
<img width="1760" height="888" alt="image" src="https://github.com/user-attachments/assets/a9a9c27f-2304-4dc8-ae52-904b75e51af8" />
- Azure Key Vault – Secure credential management
- Azure Synapse Analytics – Data warehouse integration
- GitHub – Version control
