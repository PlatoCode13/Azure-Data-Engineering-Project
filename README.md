# Azure Data Engineering Project for Healthcare Revenue Cycle Management (RCM)

## **Overview**
This project is a comprehensive Azure Data Engineering solution designed for Healthcare Revenue Cycle Management (RCM). and focuses on building an end-to-end **Azure Data Engineering Stack** solution for the **Healthcare Revenue Cycle Management (RCM) domain**.

RCM is the process that hospitals use to manage their financial operations, from the time a patient schedules an appointment to when the provider gets paid. Our goal is to process and manage healthcare financial data efficiently using Azure services.

## **Technology Stack**
- **Cloud Provider:** Microsoft Azure
- **Data Storage:** Azure Data Lake Storage Gen2 (ADLS Gen2), Azure SQL Database
- **Data Processing:** Azure Databricks, Delta Lake
- **Orchestration:** Azure Data Factory (ADF)
- **Security & Secrets Management:** Azure Key Vault
- **File Formats:** Parquet, CSV, Delta Tables
- **Infrastructure:** Medallion Architecture (Bronze, Silver, Gold Layers)

---

## **Healthcare Revenue Cycle Management (RCM) - Key Concepts**
RCM ensures that hospitals maintain financial stability while providing quality care. The process involves:

1. **Patient Visit:** Collect patient and insurance details.
2. **Service Provision:** Medical services are provided.
3. **Billing:** The hospital generates a bill.
4. **Claims Processing:** The insurance company reviews the claim and either accepts, partially pays, or rejects it.
5. **Payment & Follow-ups:** Patients and providers handle any outstanding payments.
6. **Tracking & Reporting:** Monitoring financial health through KPIs.

### **Key Aspects of RCM:**
- **Accounts Receivable (AR):** Ensuring timely collection of payments.
- **Accounts Payable (AP):** Managing hospital expenses.

**KPI Metrics for AR:**
- **AR > 90 Days** = (Outstanding > 90 days) / (Total AR)
- **Days in AR** = (Total AR) / (Average Daily Revenue)

---

## **Data Sources & Pipeline Architecture**
### **Datasets**
1. **EMR Data (Azure SQL Database)**
   - Patients
   - Providers
   - Departments
   - Transactions
   - Encounters
2. **Claims Data (Flat Files in ADLS Gen2)**
   - Monthly insurance claims data
3. **NPI Data (National Provider Identifier - API Source)**
4. **ICD Codes (Diagnosis Code System - API Source)**
5. **CPT Codes (Procedure Code System - Flat Files in ADLS Gen2)**

### **Data Pipeline - Medallion Architecture**
| Layer | Description |
|---|---|
| **Landing** | Raw data (flat files, API data, SQL extracts) |
| **Bronze** | Parquet storage (source of truth, minimal transformations) |
| **Silver** | Cleaned, standardized data with SCD2 (Slowly Changing Dimensions Type 2) processing |
| **Gold** | Aggregated business data (fact and dimension tables for reporting) |

---

## **Azure Components Used**
### **Data Storage & Processing**
- **Azure SQL DB:** Stores raw EMR data.
- **Azure Data Lake (ADLS Gen2):** Stores all data files across landing, bronze, silver, and gold layers.
- **Azure Databricks:** Performs data transformations and SCD2 logic.

### **Data Pipeline Orchestration**
- **Azure Data Factory (ADF):** Manages data movement and transformation workflows.
- **Azure Key Vault:** Secures connection strings and credentials.

### **Pipeline Activities**
- **Bronze Layer:** Ingests raw data from SQL, APIs, and flat files.
- **Silver Layer:** Performs data cleaning, Common Data Model (CDM) conversion, and SCD2.
- **Gold Layer:** Aggregates data into fact and dimension tables for business reporting.

---

## **Implementation Details**
### **1. Data Ingestion (Bronze Layer)**
- **EMR Data** → Extracted from **Azure SQL DB** to **ADLS Gen2 (Parquet Format)**.
- **Claims Data** → Flat files ingested from the landing folder to bronze.
- **NPI & ICD Data** → Extracted via API and stored in the bronze layer.

### **2. Data Processing (Silver Layer)**
- **Data Cleaning**
- **Implementing Common Data Model (CDM)**
- **Slowly Changing Dimensions (SCD2)**
- **Quality Checks (Quarantine Bad Records)**

### **3. Data Aggregation & Reporting (Gold Layer)**
- Creating **fact and dimension tables** for business users.
- Generating **AR and payment tracking reports**.

---

## **Configuration & Metadata-Driven Pipeline**
The pipeline is metadata-driven and follows a configuration-based approach:
- **Configuration File (CSV):** Contains database, table names, load type (Full/Incremental), and target paths.
- **Audit Table:** Logs each data load operation.

**Sample Configuration File (`load_config.csv`):**
```
database,datasource,tablename,loadtype,watermark,is_active,targetpath
trendytech-hospital-a,hos-a,dbo.encounters,Incremental,ModifiedDate,0,hosa
trendytech-hospital-b,hos-b,dbo.patients,Incremental,Updated_Date,1,hosb
```

**Audit Table Schema:**
```sql
CREATE TABLE IF NOT EXISTS audit.load_logs (
    id BIGINT GENERATED ALWAYS AS IDENTITY,
    data_source STRING,
    tablename STRING,
    numberofrowscopied INT,
    watermarkcolumnname STRING,
    loaddate TIMESTAMP
);
```

---

## **Optimizations & Enhancements**
- ✅ **Parallelized ADF Pipelines** (Earlier sequential execution improved to parallel execution)
- ✅ **Implemented Azure Key Vault** (For secure credential management)
- ✅ **Improved Naming Conventions & Folder Structure**
- ✅ **Implemented is_active Flag** (Tracks active/inactive records)
- ✅ **Introduced Unity Catalog** (Migrated to a centralized data catalog from Hive Metastore)
- ✅ **Enhanced Retry Mechanism** (For API calls and data loads)

---

## **Next Steps & Improvements**
🔲 Implement **Advanced Data Quality Checks**
🔲 Optimize **Delta Tables** for performance improvements
🔲 Introduce **Real-time Data Processing** with Azure Stream Analytics
🔲 Enhance **Security & Governance** with RBAC and Privilege Management

---

## **Contributors**
- **PlatoCode13** (Primary Developer & Maintainer)
- Based on concepts taught by **Sumit Sir (TrendyTech)**

---

## **License**
This project is for educational purposes and is distributed under the MIT License.

---

## **Final Thoughts**
This project serves as a comprehensive **Azure Data Engineering solution** for Healthcare RCM. By implementing best practices in **data ingestion, processing, and reporting**, it provides a scalable and efficient data pipeline.

🔹 If you found this project valuable, feel free to share your feedback and contribute to its improvement! 🔹

🚀 **Happy Coding!**

