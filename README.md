# 📊 Loan Portfolio Analytics using Microsoft Fabric

An end-to-end Data Engineering project built using **Microsoft Fabric**, implementing the **Medallion Architecture (Bronze → Silver → Gold)** to transform raw Lending Club loan data into an analytics-ready Star Schema for business reporting.

---

## 🚀 Project Overview

This project demonstrates a production-style data engineering workflow using Microsoft Fabric.

The pipeline ingests raw loan data into a Bronze layer, performs cleansing and validation in Silver, builds a dimensional model in Gold, and prepares the data for Power BI reporting.

---

## 🏗️ Architecture

<p align="center">
  <img src="images/architecture.png" width="900">
</p>


```text
                                                     Lending Club Loan Dataset
                                    accepted_2007_to_2018Q4.csv | rejected_2007_to_2018Q4.csv
                                                                │
                                                                ▼
                                                     🥉 Bronze Lakehouse
                                                  nb_load_bronze (PySpark)
                                                                │
                                       ┌────────────────────────┼────────────────────────┐
                                       ▼                        ▼                        ▼
                               nb_data_dictionary      nb_data_profile          Schema Validation
                                       │                        │
                                       └───────────────┬────────┘
                                                       ▼
                                            🥈 Silver Lakehouse
                                           nb_transform_silver
                                                       │
                                    ┌──────────────────┼──────────────────┐
                                    │                  │                  │
                                    ▼                  ▼                  ▼
                            Data Cleansing     Data Validation   Column Rationalization
                            • Type Casting     • Duplicate IDs   • 151 → 104 Columns
                            • Date Parsing     • Duplicate Rows
                            • Null Handling    • Domain Checks
                                                       │
                                                       ▼
                                              🥇 Gold Lakehouse
                                               nb_build_gold
                                                       │
                                ┌──────────────┬──────────────┬──────────────┐
                                ▼              ▼              ▼              ▼
                          gold_dim_date  gold_dim_grade  gold_dim_state  gold_dim_purpose
                                                       │
                                                       ▼
                                                gold_dim_status
                                                       │
                                                       ▼
                                                gold_fact_loan
                                                       │
                                ┌──────────────────────┼──────────────────────┐
                                ▼                      ▼                      ▼
                           Surrogate Keys      Star Schema Model     Referential Integrity
                                                                    (0 Orphan Records)
                                                       │
                                                       ▼
                                          Microsoft Fabric Pipeline
                                                       │
                                                       ▼
                                              audit_log Monitoring
                                                       │
                                                       ▼
                                           📊 Power BI Dashboard
```

---

# 🛠 Technology Stack

| Layer | Technology |
|---------|------------|
| Data Platform | Microsoft Fabric |
| Storage | OneLake Lakehouse |
| Processing | PySpark |
| Storage Format | Delta Tables |
| Orchestration | Microsoft Fabric Pipeline |
| Data Modeling | Star Schema |
| Reporting | Power BI |

---

# 📂 Dataset

**Source**

Lending Club Loan Dataset (2007–2018)

Dataset contains

- Accepted Loans
- Rejected Loan Applications

---

## ⚙️ ETL Pipeline

The complete ETL process is orchestrated using a Microsoft Fabric Data Pipeline.

<p align="center">
  <img src="images/pipeline.png" width="900">
</p>

Pipeline Flow

```
Load Bronze
      ↓
Data Dictionary
      ↓
Data Profiling
      ↓
Transform Silver
      ↓
Build Gold
```

---

## 🥉 Bronze Layer

- Raw Lending Club loan files loaded into Delta tables
- No transformations performed
- Source data preserved for traceability
### Statistics

| Metric | Value |
|--------|------:|
| Accepted Loan Records | **2,260,701** |

---

## 🥈 Silver Layer

Implemented:

- Data type conversions
- Date parsing
- Duplicate removal
- Invalid record filtering
- Domain validation
- Column rationalization
- Data quality checks

  ### Result

| Metric | Value |
|--------|------:|
| Columns Reduced | **151 → 104** |
| Duplicate Rows | **0** |
| Duplicate Loan IDs | **0** |
| Invalid Purpose Records | **0** |

---

## 🥇 Gold Layer

Designed a Star Schema consisting of:

### Fact Table

- gold_fact_loan

### Dimension Tables

- gold_dim_date
- gold_dim_grade
- gold_dim_purpose
- gold_dim_state
- gold_dim_status

Features:

- Surrogate Keys
- Referential Integrity Validation
- Analytics-ready dimensional model

---

## ✅ Data Quality

Implemented validation checks for:

- Duplicate Loan IDs
- Duplicate Rows
- Invalid Loan Status
- Invalid Purpose
- Invalid Grade
- Invalid Home Ownership
- Invalid Verification Status
- Referential Integrity (Fact ↔ Dimensions)

  ---

# 🔍 Referential Integrity Validation

Validated all surrogate keys after Gold layer creation.

| Validation | Result |
|------------|--------|
| Grade Keys | ✅ 0 Orphans |
| Purpose Keys | ✅ 0 Orphans |
| State Keys | ✅ 0 Orphans |
| Status Keys | ✅ 0 Orphans |
| Date Keys | ✅ 0 Orphans |

---

## 📷 Project Screenshots

### Lakehouse

![](images/lakehouse.png)

---

### Gold Fact Table

![](images/gold_fact.png)

---
## 📋 Audit Log

![Audit Log](images/audit_log.png)

---

### Referential Integrity Validation

![](images/referential_integrity.png)

---

## 📂 Repository Structure

```
Loan-Portfolio-Analytics-Fabric/

│── images/
│── notebooks/
│── powerbi/
│── README.md
```

---

## 🔜 Future Enhancements

- Incremental Loading (MERGE)
- Slowly Changing Dimensions (SCD Type 2)
- Metadata-driven Framework
- Approval Funnel Analytics
- Power BI Executive Dashboard

---

## 👨‍💻 Author

**Chirag Arora**

Microsoft Fabric | Data Engineering | Power BI
