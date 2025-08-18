# azure-healthcare-rcm-pipeline
📌 Overview<br>

This project implements a Medallion Architecture (Bronze → Silver → Gold) on Azure Data Engineering Stack to process Healthcare Revenue Cycle Management (RCM) data. RCM covers the complete financial lifecycle of a patient’s journey — from appointment scheduling to final payment — helping hospitals track Accounts Receivable (AR) and Accounts Payable (AP) KPIs such as Days in AR and AR > 90 Days %. <br>

🏗 Architecture <br>

<img width="762" height="452" alt="image" src="https://github.com/user-attachments/assets/113be95a-ed5e-4846-b174-dc9bca188554" />

Medallion Architecture <br>
* Bronze – Raw data ingestion from multiple sources in Parquet format.
* Silver – Data cleaning, enrichment, SCD2 implementation, Common Data Model (CDM).
* Gold – Aggregated fact and dimension tables for reporting & analytics.

Data Sources <br>
* EMR Data – Azure SQL DB (Patients, Providers, Departments, Transactions, Encounters)
* Claims Data – Flat files from insurance providers (Landing Zone)
* NPI & ICD Codes – Public APIs
* CPT Codes – Flat files

Tools & Services <br>
* Azure Data Factory – Orchestrates ingestion pipelines
* Azure Databricks – Data transformation & SCD2 implementation
* Azure SQL Database – EMR data source
* Azure Data Lake Storage Gen2 – Landing, Bronze, Silver, Gold zones
* Azure Key Vault – Secure credentials storage
* Unity Catalog – Centralized table & schema management

⚙️ Pipeline Workflow
1. Ingestion (Bronze Layer)
   * Load EMR data from Azure SQL DB to ADLS Gen2 in Parquet format.
     
     <img width="1723" height="637" alt="image" src="https://github.com/user-attachments/assets/d131d0e1-2cec-4629-bbd2-1fe24c734e73" />

   * Move claims, CPT, NPI, and ICD data from landing/API to Bronze zone.

   * Maintain ingestion audit logs with metadata.

2. Transformation (Silver Layer) <br>
   * Apply cleaning, data quality checks, and quarantine invalid records.
   * Implemented **Slowly Changing Dimension Type 2 (SCD2)** for tracking historical changes in Patients, Providers, Departments, Transactions, Claims, and Codes using **Delta Tables**. Example shown below for cpt_codes
     <img width="543" height="352" alt="image" src="https://github.com/user-attachments/assets/8c2746d5-3353-46df-850e-477f21bbd751" />

   * Standardize schema via Common Data Model.

3. Aggregation (Gold Layer)
   * Build Fact and Dimension tables for KPIs and dashboards.
     <img width="948" height="533" alt="image" src="https://github.com/user-attachments/assets/148bab89-4fbf-4a78-832b-df7ce5374844" />

   * Support business teams, data analysts, and ML workloads.


Key Features of project

1. Built scalable ingestion and transformation pipelines with Azure Data Factory and Databricks

2. Implemented the Bronze → Silver → Gold Medallion Architecture

3. Applied SCD2, Common Data Model (CDM), and data quality checks with quarantine handling

4. Designed fact and dimension tables to support BI and KPI reporting

5. Used Unity Catalog to:

    * Store and manage Bronze, Silver, and Gold tables

    * Maintain audit logs for incremental and full loads
