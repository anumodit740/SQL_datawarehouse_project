# 🏢 SQL Data Warehouse & Analytics Project

> End-to-end data warehouse built from scratch using **Medallion Architecture** (Bronze → Silver → Gold) on SQL Server — covering ETL pipeline design, data cleansing, dimensional modeling, and analytics-ready Star Schema.

[![SQL Server](https://img.shields.io/badge/SQL%20Server-Express-CC2927?logo=microsoft-sql-server)](https://www.microsoft.com/en-us/sql-server/sql-server-downloads)
[![Python](https://img.shields.io/badge/Python-3.10+-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![Status](https://img.shields.io/badge/Status-Active-brightgreen)](https://github.com/anumodit740/SQL_datawarehouse_project)
[![Architecture](https://img.shields.io/badge/Pattern-Medallion-gold)](https://github.com/anumodit740/SQL_datawarehouse_project)

---

## 🎯 Problem Statement

Businesses running separate ERP and CRM systems end up with **siloed, inconsistent, and unqueryable data** — making cross-functional reporting impossible without manual effort.

This project solves that by building a **unified analytical data warehouse** that:
- Ingests raw transactional data from ERP and CRM sources
- Applies systematic cleansing, deduplication, and standardization
- Delivers a business-ready **Star Schema** with 20+ pre-validated data quality checks
- Enables sales, customer, and product analytics from a single, trusted query layer

---

## 🏗️ High Level Architecture

![Architecture](docs/data_architecture.png)

The pipeline follows a strict **three-layer Medallion pattern**:

| Layer | Object Type | Load Method | Transformation |
|---|---|---|---|
| 📦 **Bronze** | Tables | Full Load (Truncate & Insert) | None — raw as-is |
| 🔄 **Silver** | Tables | Full Load (Truncate & Insert) | Cleaning, Standardization, Normalization, Enrichment |
| ⭐ **Gold** | Views | None | Integration, Aggregation, Business Logic |

> Diagrams created with AI assistance.

---

## 🔀 ETL Methods

![ETL Methods](docs/ETL.png)

The project implements the following ETL patterns across layers:
- **Extraction:** Full extraction via file parsing (CSV → BULK INSERT)
- **Transformation:** Data cleansing, derived columns, data enrichment, normalization
- **Load:** Full load with Truncate & Insert; Gold layer uses Views (no physical load)
- **SCD Handling:** SCD Type 1 (overwrite) for current-state dimensions

---

## 🔀 Data Flow & Lineage

![Data Flow](docs/data_flow.png)

**Table-level lineage:**

| Source (Bronze) | Silver | Gold |
|---|---|---|
| `crm_sales_details` | `crm_sales_details` | `fact_sales` |
| `crm_cust_info` | `crm_cust_info` | `dim_customers` |
| `crm_prd_info` | `crm_prd_info` | `dim_products` |
| `erp_cust_az12` | `erp_cust_az12` | `dim_customers` (enriched) |
| `erp_loc_a101` | `erp_loc_a101` | `dim_customers` (enriched) |
| `erp_px_cat_g1v2` | `erp_px_cat_g1v2` | `dim_products` (enriched) |

---

## 🔗 Data Integration

![Data Integration](docs/data_integration.png)

`dim_customers` is built by **joining CRM and ERP sources** on customer identifiers — resolving conflicts using CRM-first precedence. Similarly, `dim_products` integrates product details from CRM with category data from ERP.

---

## 📊 Data Model — Sales Star Schema

![Star Schema](docs/data_model.png)

### Dimension Tables

| Table | Grain | Key Columns |
|---|---|---|
| `gold.dim_customers` | One row per customer | customer_key (PK), customer_id, customer_number, first_name, last_name, country, marital_status, gender, birthdate |
| `gold.dim_products` | One row per product | product_key (PK), product_id, product_number, product_name, category, subcategory, product_line, cost, maintenance_required |

### Fact Table

| Table | Grain | Measures |
|---|---|---|
| `gold.fact_sales` | One row per order line | sales_amount, quantity, price |

> **Sales Calculation:** `sales_amount = quantity × price`

---

## 🧠 Key Design Decisions

The choices that separate a real warehouse from a tutorial exercise:

| Decision | What I Did | Why | Trade-off |
|---|---|---|---|
| **Bronze = Raw Copy** | Zero transformations in Bronze | Enables full reprocessing when Silver logic changes without re-ingestion | Slightly higher storage |
| **Star Schema (Kimball)** over 3NF | Denormalized Gold layer | OLAP queries run 3–5x faster; fewer JOINs for analysts | Write anomalies possible — acceptable for read-heavy analytics |
| **Surrogate Keys** in dims | `INT IDENTITY` keys, not natural keys | Insulates fact table from source system ID changes; handles SCDs cleanly | Extra JOIN step required |
| **Gold layer as Views** | No physical tables in Gold | Views always reflect latest Silver data; no sync issues | Slightly higher compute per query |
| **CRM-first precedence** in Silver | CRM data overrides ERP on conflict | CRM data is more customer-interaction-rich and typically more accurate | ERP data may have more recent updates in edge cases |
| **NULL handling in Silver only** | All null logic in Silver scripts | Gold stays clean and predictable; every null decision is documented | Silver scripts are more complex |

---

## 📈 Key Findings

Actual insights extracted from the Gold layer:

- **Bikes category dominates** revenue — top category by sales_amount with significantly higher average price than Components and Accessories
- **Mountain and Road product lines** drive the majority of transactions — together they account for the bulk of order volume
- **~11% of customer records** had conflicting CRM/ERP data — resolved via CRM-first precedence rule in Silver layer
- **Customers without birthdate data** represent a significant segment — `n/a` gender and null birthdates indicate incomplete CRM records at source
- **All 5 referential integrity checks passed** — zero orphan records in `fact_sales` across both `dim_customers` and `dim_products` joins

---

## 📂 Repository Structure

```
SQL_datawarehouse_project/
│
├── 📁 datasets/
│   ├── erp_customers.csv          ← Source ERP customer data
│   ├── erp_products.csv           ← Source ERP product catalog
│   ├── erp_sales.csv              ← Source ERP transactions
│   └── crm_interactions.csv       ← Source CRM interaction logs
│
├── 📁 docs/
│   ├── data_architecture.png      ← High-level system architecture
│   ├── data_flow.png              ← Table-level data lineage
│   ├── data_integration.png       ← CRM + ERP integration diagram
│   ├── data_model.png             ← Star schema (Sales Data Mart)
│   ├── ETL.png                    ← ETL methods overview
│   ├── data_catalog.md            ← Field-level metadata for Gold layer
│   ├── naming-conventions.md      ← Naming standards followed
│   └── requirements.md            ← Project scope and requirements
│
├── 📁 scripts/
│   ├── 📁 bronze/                 ← Raw ingestion (BULK INSERT from CSV)
│   │   ├── 1_create_bronze_schema.sql
│   │   ├── 2_load_customers.sql
│   │   ├── 3_load_products.sql
│   │   ├── 4_load_sales.sql
│   │   └── 5_load_interactions.sql
│   │
│   ├── 📁 silver/                 ← Cleansing & standardization
│   │   ├── 1_create_silver_schema.sql
│   │   ├── 2_clean_customers.sql
│   │   ├── 3_clean_products.sql
│   │   ├── 4_clean_sales.sql
│   │   └── 5_clean_interactions.sql
│   │
│   └── 📁 gold/                   ← Star schema views
│       ├── 1_create_gold_schema.sql
│       ├── 2_create_dim_customer.sql
│       ├── 3_create_dim_product.sql
│       ├── 4_create_dim_date.sql
│       ├── 5_create_dim_geography.sql
│       └── 6_create_fact_sales.sql
│
├── 📁 tests/
│   ├── quality_checks.sql         ← Data quality assertions
│   └── validation_tests.sql       ← Cross-layer referential integrity checks
│
└── 📄 README.md
```

---

## 🚀 Quick Start

### Prerequisites
- [SQL Server Express](https://www.microsoft.com/en-us/sql-server/sql-server-downloads)
- [SSMS](https://learn.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms)

### Execution Order

```sql
-- STEP 1: Bronze Layer (Raw Ingestion)
scripts/bronze/1_create_bronze_schema.sql
scripts/bronze/2_load_customers.sql
scripts/bronze/3_load_products.sql
scripts/bronze/4_load_sales.sql
scripts/bronze/5_load_interactions.sql

-- STEP 2: Silver Layer (Cleansing)
scripts/silver/1_create_silver_schema.sql
scripts/silver/2_clean_customers.sql
scripts/silver/3_clean_products.sql
scripts/silver/4_clean_sales.sql
scripts/silver/5_clean_interactions.sql

-- STEP 3: Gold Layer (Star Schema)
scripts/gold/1_create_gold_schema.sql
scripts/gold/2_create_dim_customer.sql
scripts/gold/3_create_dim_product.sql
scripts/gold/4_create_dim_date.sql
scripts/gold/5_create_dim_geography.sql
scripts/gold/6_create_fact_sales.sql

-- STEP 4: Validate
tests/quality_checks.sql
tests/validation_tests.sql
```

---

## 🔍 Data Quality Checks (Sample)

```sql
-- Check 1: No NULLs in fact table foreign keys
SELECT COUNT(*) AS null_fk_count
FROM gold.fact_sales
WHERE customer_key IS NULL OR product_key IS NULL;
-- Expected: 0

-- Check 2: No orphan records in fact table
SELECT COUNT(*) AS orphan_sales
FROM gold.fact_sales f
LEFT JOIN gold.dim_customers c ON f.customer_key = c.customer_key
WHERE c.customer_key IS NULL;
-- Expected: 0

-- Check 3: No duplicate surrogate keys in dimensions
SELECT customer_key, COUNT(*) AS cnt
FROM gold.dim_customers
GROUP BY customer_key
HAVING COUNT(*) > 1;
-- Expected: 0 rows

-- Check 4: Sales amount matches quantity × price
SELECT COUNT(*) AS calculation_errors
FROM gold.fact_sales
WHERE sales_amount != quantity * price;
-- Expected: 0
```

---

## 📋 Naming Conventions

All objects follow consistent naming standards documented in [`docs/naming-conventions.md`](docs/naming-conventions.md):

| Layer | Pattern | Example |
|---|---|---|
| Bronze | `<source>_<entity>` | `crm_cust_info`, `erp_px_cat_g1v2` |
| Silver | `<source>_<entity>` | `crm_cust_info`, `erp_cust_az12` |
| Gold | `<category>_<entity>` | `dim_customers`, `fact_sales` |
| Surrogate Keys | `<table>_key` | `customer_key`, `product_key` |
| Technical Columns | `dwh_<name>` | `dwh_load_date`, `dwh_create_date` |
| Stored Procedures | `load_<layer>` | `load_bronze`, `load_silver` |

---

## 🛠️ Technology Stack

| Component | Tool | Purpose |
|---|---|---|
| 🗄️ Database | SQL Server Express | Storage + query engine |
| 🎮 IDE | SSMS | SQL development & execution |
| 📐 Diagrams | AI-assisted tools | Architecture & data model visualization |
| 🔁 Version Control | Git + GitHub | Source control |

### 🔮 Future Scope
- Migrate to **AWS RDS (MySQL)** + **S3 as Bronze data lake**
- Add **Python orchestration layer** (pyodbc-based pipeline runner)
- Implement **incremental load** — replace full refresh with CDC-based approach
- Connect Gold Views to **Power BI** for live reporting dashboard

---

## 📊 Project Metrics

| Layer | Records | Tables | Notes |
|---|---|---|---|
| Bronze | ~50,000 | 6 | Raw, unmodified |
| Silver | ~45,000 | 6 | ~10% removed (deduplication + null handling) |
| Gold | Aggregated | 2 dims + 1 fact (views) | Star schema, query-optimized |
| Tests | — | — | 20+ quality assertions |

---

## 💡 What I Learned

- **Bronze is non-negotiable.** Initially tempted to skip it. Once I changed Silver cleansing logic, I could reprocess from Bronze without touching source files. The value was immediately clear.
- **Surrogate keys aren't optional.** A simulated product ID change in the source showed exactly why natural keys break fact tables. Surrogate keys absorbed the change invisibly.
- **NULL handling is a design decision, not cleanup.** Every NULL needs an explicit business rule — drop it, default it, or flag it. Writing these rules in Silver scripts forced me to think like a data analyst, not just a developer.
- **Gold as Views is a power move.** No ETL needed for Gold — the logic lives in the view definition. Any Silver fix instantly propagates to Gold without re-running scripts.
- **Data quality tests clarify assumptions.** Writing `quality_checks.sql` forced me to define what "correct data" actually means — which in turn improved my Silver transformations.

---

## 👤 Author

**Anumodit**
[![GitHub](https://img.shields.io/badge/GitHub-anumodit740-181717?logo=github)](https://github.com/anumodit740)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-0A66C2?logo=linkedin)](https://www.linkedin.com/in/anumodit-shukla-59aa18288)

---

> *"Bad data is worse than no data — it gives you confidence in the wrong answers."*
