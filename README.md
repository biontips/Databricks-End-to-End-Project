# Azure Databricks Lakehouse End-to-End Analytics Project

> **Lakehouse Implementation Focused on Analytics Consumption & Power BI Semantic Modeling**

---

## 📌 Overview

This repository showcases an **end-to-end Analytics project built on Azure Databricks**, created to demonstrate a real-world implementation of the **Medallion Architecture (Bronze, Silver, Gold)** using modern data platform practices.

While the implementation includes core data engineering patterns such as ingestion, transformation, and orchestration, the primary goal is to show **how lakehouse data can be structured into analytics-ready datasets aligned for Power BI and enterprise BI consumption**.

The project covers:

- Full Azure cloud setup  
- Streaming ingestion using Auto Loader  
- Batch and transformation pipelines  
- Job orchestration using Databricks Workflows  
- Slowly Changing Dimension (SCD) handling  
- Creation of curated analytical fact tables  

---

## 🎯 Analytics Objective

This project focuses on solving a common modern analytics challenge:

> How do we transform raw lakehouse data into governed, dimensional datasets optimized for reporting and semantic modeling?

Key goals:

- Shape Gold-layer datasets aligned to **star schema modeling**
- Minimize transformation complexity inside BI tools
- Implement SCD handling directly in the Lakehouse
- Demonstrate Databricks → Power BI integration patterns
- Bridge Data Engineering pipelines with Analytics Engineering outcomes

---

## 🏗️ Architecture Overview

The solution follows a layered architecture based on the **Medallion Architecture** pattern:

Source → Bronze → Silver → Gold → Fact Tables → Power BI

The architecture is intentionally designed so that the **Gold layer behaves like a dimensional warehouse**, enabling efficient analytical querying rather than leaving modeling responsibility to reporting tools.

| Layer | Purpose |
|------|---------|
| Bronze | Raw ingestion from ADLS Gen2 using Auto Loader |
| Silver | Data cleansing, validation, and standardization |
| Gold | Business-ready dimensions, facts, and SCD modeling |
| Power BI | Semantic modeling and reporting layer |

---

## 🔁 Databricks Job Orchestration

The pipeline is orchestrated using **Databricks Jobs**, where each layer depends on successful completion of the previous one.

This mirrors real-world production pipelines where raw data is progressively refined into trusted analytical assets.

![Databricks Pipeline Architecture](https://github.com/dataontips/Databricks-End-to-End-Project/blob/462676906cd612c576f80b4fef07b217c6388e44/Images/databricks_pipeline.png)

---

## 🔧 Technologies Used

- **Cloud Platform:** Microsoft Azure  
- **Data Processing:** Azure Databricks (PySpark)  
- **Storage:** Azure Data Lake Storage Gen2 (ADLS)  
- **Ingestion:** Databricks Auto Loader  
- **Architecture Pattern:** Medallion Architecture  
- **Orchestration:** Databricks Jobs  
- **Governance:** Unity Catalog  
- **Version Control:** GitHub  

These tools simulate a **modern Microsoft analytics ecosystem** where Databricks performs transformation and Power BI consumes curated datasets.

---

## ☁️ Azure Resources Created

All infrastructure was provisioned manually as part of the implementation:

- Azure Resource Group  
- Azure Storage Account (ADLS Gen2)
  - Containers for source, bronze, silver, and gold layers  
- Azure Databricks Workspace  
- Databricks Access Connector  
- Unity Catalog Metastore  
- External Locations & Storage Credentials  

This setup reflects a governed Lakehouse deployment model with separation of storage, compute, and catalog.

---

## 📂 Project Structure

> The data files included are **sample datasets** used for demonstration purposes only.

````text
Databricks-End-to-End-Project
│
├── Data Files
├── Images
├── Power BI Report
├── Project NoteBooks
└── README.md
````

The structure mirrors how engineering zones are separated in production analytics environments.

---

## 🔄 Data Pipeline Flow

Raw Operational Data → Validated Data → Analytics-Ready Dimensions → BI Consumption


---

## 🧬 Slowly Changing Dimensions (SCD)

This project implements **both SCD Type 1 and SCD Type 2** patterns in the Gold layer, demonstrating dimensional modeling directly inside a Lakehouse.

---

### 🔹 SCD Type 1 — Overwrite History (Customers Dimension)

Used when only the latest version is required.

**Approach:**

- Reads from Silver Customers table  
- Deduplicates records on `customer_id`  
- Identifies new vs existing records using joins  
- Generates surrogate key (`DimCustomerKey`)  
- Maintains audit timestamps  
- Uses Delta Lake MERGE for updates  

**Outcome:**  
Ensures a clean, current-state dimension optimized for reporting.

---

### 🔹 SCD Type 2 — Preserve History (Products Dimension)

Used when historical tracking is required (e.g., product price/category changes).

**Approach:**

- Implemented using Delta Live Tables (DLT)  
- Applies `dlt.apply_changes()` with `stored_as_scd_type = 2`  
- Maintains:
  - `effective_start_date`
  - `effective_end_date`
  - `is_current` flag  

**Outcome:**  
Enables lifecycle analysis, trend reporting, and historical comparisons.

---

## 🔄 Layer Responsibilities

### 1️⃣ Bronze Layer — Raw Ingestion

- Uses Databricks Auto Loader (`cloudFiles`)
- Ingests Parquet files from ADLS
- Supports schema evolution
- Checkpointing ensures reliability

### 2️⃣ Silver Layer — Data Transformation

- Cleansing and normalization
- Column standardization
- Business-level entity separation:
  - Customers
  - Orders
  - Products

### 3️⃣ Gold Layer — Analytics Modeling

- Curated dimensional tables
- Created using Spark jobs and DLT
- Optimized for BI consumption

### 4️⃣ Fact Tables

- `fact_orders` created for analytical workloads
- Structured for aggregation and filtering

---

## 🧩 Orchestration Strategy

- Implemented using Databricks Jobs
- Layer dependencies reflect Medallion flow
- Supports reruns and failure recovery

---

## 🔐 Governance & Security

- Unity Catalog enabled for centralized governance
- External locations configured for ADLS access
- Separation of storage and compute
- Structured promotion across Bronze → Silver → Gold ensures traceability

---

## 📊 Power BI Reporting

Power BI connects to curated **Gold layer tables** to support business reporting and analytics.

The BI layer consumes pre-modeled datasets so semantic modeling focuses on measures and business logic rather than data shaping.

### Sales Overview
![Power BI Report](https://github.com/dataontips/Databricks-End-to-End-Project/blob/b756d4b17d082669f2a2b4efbe4795ef3b32857b/Images/Sales%20Overview.png)

### Product Performance
![Power BI Report](https://github.com/dataontips/Databricks-End-to-End-Project/blob/b756d4b17d082669f2a2b4efbe4795ef3b32857b/Images/Product%20Performance%20Analysis.png)

### Customer Insights
![Power BI Report](https://github.com/dataontips/Databricks-End-to-End-Project/blob/b756d4b17d082669f2a2b4efbe4795ef3b32857b/Images/Customer%20Insights.png)

---

## 🚀 Key Highlights

- End-to-end Azure Databricks implementation  
- Production-style Medallion Architecture  
- Streaming + batch hybrid processing  
- Dimensional modeling within the Lakehouse  
- Designed for BI and semantic model integration  
- Demonstrates Lakehouse-to-Analytics workflow alignment  

---

## 📈 Possible Enhancements

- Add orchestration via Azure Data Factory  
- CI/CD integration using GitHub Actions  
- Monitoring via Azure Monitor  
- Expand Power BI semantic model  

---

## 👤 Author

**Navjeet Singh**  
Lead Analytics Engineer | Enterprise Semantic Architecture | BI & Lakehouse Integration | Azure | Databricks 

This repository demonstrates transforming lakehouse data into analytics-ready models aligned with enterprise BI consumption patterns.

---

## 📜 License

This project is for learning and portfolio purposes.  
You are free to fork and adapt it with attribution.

