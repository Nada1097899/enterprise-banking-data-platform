<img width="1536" height="1024" alt="ChatGPT Image Sep 6, 2026, 05_47_28 PM" src="https://github.com/user-attachments/assets/eb265404-e2c1-4da6-9461-d9255c5dbb13" /># Enterprise Banking Data Engineering Platform

An end-to-end **Enterprise Data Engineering Platform** designed to simulate a real-world banking data environment.

The project covers the complete data lifecycle — from raw data ingestion and data quality validation to dimensional modeling, business analytics, cloud deployment, and Power BI reporting.

---

## 📌 Project Overview

Modern banking systems receive data from multiple sources with different formats, quality issues, and business rules.

This project was built to address these challenges through a layered and policy-driven data platform that provides:

* Reliable data ingestion
* Automated Data Quality validation
* Data cleansing and standardization
* Rejected-record isolation
* Incremental ETL processing
* Enterprise Data Warehouse modeling
* SCD Type 2 implementation
* Cloud deployment using Azure SQL
* Business-ready analytical views
* Power BI reporting and analytics

---

## 🏗️ Architecture

The platform follows a layered architecture:

**Raw Sources → Landing Zone → Bronze → Data Quality Validation → Quarantine / Silver → Transformation → Gold → Azure SQL → Power BI**

![Architecture](docs/Architecture.png)

### Main Layers

| Layer        | Purpose                                                 |
| ------------ | ------------------------------------------------------- |
| Landing Zone | Receives and stages incoming source data                |
| Bronze       | Stores source-level data                                |
| Data Quality | Validates data against predefined rules and policies    |
| Quarantine   | Isolates rejected or invalid records                    |
| Silver       | Stores cleaned and standardized data                    |
| Gold         | Provides dimensional models and business-ready datasets |
| Azure SQL    | Cloud-hosted analytical destination                     |
| Power BI     | Business intelligence and visualization                 |

---

## 🛠️ Technologies

* **Python**
* **SQL Server**
* **T-SQL**
* **Pandas**
* **PyODBC**
* **Azure SQL Database**
* **Power BI**
* **REST API**
* **Git & GitHub**

---



## 🔄 ETL Pipeline

The ETL pipeline was designed as a controlled and policy-driven process:

```text
Source Data
    ↓
Landing Zone
    ↓
Bronze Layer
    ↓
Acceptance Policy
    ↓
Data Quality Validation
    ↓
 ┌───────────────┐
 │               │
Valid          Invalid
 │               │
 ↓               ↓
Silver       Quarantine
 │
 ↓
Transformation
 │
 ↓
Gold Layer
 │
 ↓
Azure SQL
 │
 ↓
Power BI
```

The pipeline includes validation, cleaning, standardization, business rules, reconciliation, logging, and batch tracking.

---

# 🔍 Data Quality Framework

A major component of the project is the automated **Data Quality Engine**.

The framework evaluates data across six dimensions:

| Dimension             | Weight |
| --------------------- | -----: |
| Completeness          |    15% |
| Validity              |    20% |
| Referential Integrity |    25% |
| Consistency           |    15% |
| Uniqueness            |    10% |
| Outlier Detection     |    15% |

### Data Quality Components

The framework includes metadata-driven rules and automated validation procedures for:

* Completeness
* Validity
* Consistency
* Referential Integrity
* Uniqueness
* Outlier Detection

Data quality results are stored and tracked by batch, allowing the pipeline to monitor data quality over time.

---

## 🗄️ Metadata-Driven Validation

Instead of hard-coding every validation rule inside the ETL process, the framework uses metadata tables to define and manage quality rules.

Examples include:

```text
dq_check_columns
dq_unique_columns
dq_validity_rules
dq_referential_rules
dq_outlier_rules
dq_batches
dq_dimension_weights
data_quality_results
```

This makes the framework easier to maintain and extend when new tables or business rules are introduced.

---
<img width="1163" height="698" alt="photo_2026-09-07_01-38-47" src="https://github.com/user-attachments/assets/c3e06564-7cb6-4f53-a249-183a6a255c59" />

<img width="1280" height="714" alt="photo_2026-09-07_01-38-55" src="https://github.com/user-attachments/assets/8daab645-0a9d-4c5e-a0bc-bd0e2d0bf651" />

# 🥈 Silver Layer

The Silver Layer contains cleaned and standardized records that successfully pass the required acceptance and business rules.

Rejected records are isolated in the quarantine process instead of contaminating downstream analytical data.

The pipeline also performs reconciliation and referential integrity validation against the actual Silver layer to ensure that relationships remain valid after ETL processing.

---

# 🥇 Gold Layer

The Gold Layer follows a **Star Schema** designed for analytical workloads.

### Dimensions

```text
dim_customer
dim_account
dim_branch
dim_campaign
dim_card
dim_date
```

### Facts

```text
fact_transaction
fact_loan
fact_customer_campaign
```

![Database Schema](docs/Database_Schema.png)

---

## 📊 Gold Layer Results

The final Gold Layer contains:

| Table                  | Records |
| ---------------------- | ------: |
| dim_customer           |  97,847 |
| dim_account            | 118,600 |
| dim_branch             |     243 |
| dim_campaign           |      97 |
| dim_card               | 105,557 |
| dim_date               |  14,610 |
| fact_transaction       | 381,180 |
| fact_loan              |  18,752 |
| fact_customer_campaign | 270,392 |

---

# 🔁 SCD Type 2

The `dim_customer` dimension implements **Slowly Changing Dimension Type 2**.

This allows the warehouse to preserve historical versions of customer attributes instead of overwriting previous values.

The implementation tracks:

* Surrogate Key
* Valid From
* Valid To
* Current Flag

This enables historical analysis while maintaining the current customer state.

![SCD Type 2](screenshots/scd_type2.png)

---

# 🔗 Data Validation

The Gold Layer was validated through multiple checks:

### Row Count Validation

Local and target datasets were reconciled to ensure expected record counts.

### Foreign Key Validation

Fact-to-dimension relationships were validated to detect orphan records.

### Null Foreign Key Validation

Required analytical foreign keys were checked for missing values.

### Fact Key Validation

Fact keys were validated against expected row counts.

### SCD Type 2 Validation

Historical and current customer versions were checked to ensure correct versioning behavior.

---

# ☁️ Cloud Deployment

The analytical Gold Layer was migrated to **Azure SQL Database** as the cloud destination.

The migration process included:

1. Gold table migration
2. Schema validation
3. Row-count reconciliation
4. Foreign-key validation
5. Data type validation
6. Business view deployment
7. Final analytical validation

![Azure SQL](screenshots/azure_sql.png)

---
<img width="1920" height="1080" alt="azure_sql_gold" src="https://github.com/user-attachments/assets/8f36b413-d9bf-4ba2-ade0-14a21f4e94c8" />


# 📈 Power BI

The final analytical layer is consumed through Power BI.

The dashboard provides both **Data Quality Monitoring** and **Business Analytics**.

### Data Quality

* Overall data quality score
* Quality by dimension
* Failed records
* Quality trends
* Validation results

### Business Analytics

* Customer analysis
* Loan portfolio
* Branch performance
* Campaign performance
* Transaction analytics

![Power BI Dashboard](screenshots/powerbi_dashboard.png)

---

# 📊 Business Views

Five analytical business views were created:

```text
vw_Customer360
vw_LoanPortfolio
vw_BranchPerformance
vw_CampaignPerformance
vw_TransactionAnalytics
```

These views provide business-ready datasets for reporting and dashboard development.

---

<img width="1340" height="750" alt="Annotation 2026-09-07 013649" src="https://github.com/user-attachments/assets/1c57155f-16fd-4633-acd6-3e1706c195c2" />
<img width="1341" height="738" alt="Annotation 2026-09-07 013711" src="https://github.com/user-attachments/assets/8dff00e6-4709-46d7-981a-b797679ca783" />
<img width="1445" height="762" alt="Annotation 2026-09-07 013744" src="https://github.com/user-attachments/assets/313a11d4-854d-4c6b-b345-e0a43077637e" />
<img width="1381" height="753" alt="Annotation 2026-09-07 013805" src="https://github.com/user-attachments/assets/38f1ec01-f136-410b-bf76-2dd64ac6eb63" />
<img width="1280" height="720" alt="photo_2026-09-07_01-36-23" src="https://github.com/user-attachments/assets/e3d97eb6-ecc0-4af8-b1b3-4d8730add213" />

# 📁 Project Structure

```text
Enterprise-Banking-Data-Engineering/
│
├── README.md
│
├── python/
│   ├── ETL scripts
│   ├── Data Quality scripts
│   └── utility scripts
│
├── sql/
│   ├── Bronze
│   ├── Silver
│   ├── Gold
│   ├── Data Quality
│   └── Views
│
├── docs/
│   ├── Architecture.png
│   ├── Database_Schema.png
│   └── Data_Dictionary.xlsx
│
├── screenshots/
│   ├── architecture.png
│   ├── dq_dashboard.png
│   ├── gold_layer.png
│   ├── azure_sql.png
│   ├── powerbi_dashboard.png
│   └── scd_type2.png
│
└── powerbi/
    └── Enterprise_Banking_Dashboard.pbix
```

> **Note:** The Power BI `.pbix` file may exceed GitHub's standard file-size limit. If necessary, it can be provided through Git LFS or as a separate downloadable artifact.

---

# 🎯 Key Engineering Concepts Demonstrated

This project demonstrates practical experience with:

* End-to-end ETL
* Layered Data Architecture
* Data Quality Engineering
* Metadata-driven validation
* Data Cleansing & Standardization
* Data Reconciliation
* Batch Processing
* Error Handling
* Quarantine Processing
* Referential Integrity
* Star Schema
* Surrogate Keys
* Slowly Changing Dimensions Type 2
* Business Data Marts
* Azure SQL
* Power BI
* Python Automation
* SQL Server
* Cloud Data Warehousing

---

# 🚀 Project Outcome

The final platform transforms heterogeneous banking data into a validated, standardized, historical, and analytics-ready data warehouse.

The architecture was designed with **data reliability, scalability, maintainability, and business usability** in mind.

---

## 👩‍💻 Author

**Nada**

Data Science Student | Data Analytics & Data Engineering

---

## 📌 Project Status

**Completed — Version 1.0**
