# 🏢 Data Warehouse & Analytics Project

> A comprehensive demonstration of building a modern data warehouse and analytics solution using SQL Server and the **Medallion Architecture** pattern.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![SQL Server](https://img.shields.io/badge/SQL%20Server-Express-CC2927?logo=microsoft-sql-server)](https://www.microsoft.com/en-us/sql-server/sql-server-downloads)
[![Status](https://img.shields.io/badge/Status-Active-brightgreen)](https://github.com/anumodit740/SQL_datawarehouse_project)

---

## 📊 Project Highlights

| 🎯 Feature | 📝 Description |
|-----------|----------------|
| 🏗️ **Medallion Architecture** | Bronze → Silver → Gold layered data pipeline |
| 🔄 **ETL Pipelines** | Extract, Transform, Load from multiple sources |
| ⭐ **Star Schema** | Optimized data model for fast analytics |
| 📈 **Analytics Ready** | Pre-built insights on sales, customers & products |
| 🔍 **Data Quality** | Validation tests and cleansing workflows |
| 📚 **Well Documented** | Architecture diagrams and data catalogs |

---

## 🏗️ Architecture Overview

```
┌─────────────────────────────────────────────────────────────┐
│                    DATA SOURCES                             │
│         (ERP System | CRM System | CSV Files)               │
└────────────────────┬────────────────────────────────────────┘
                     │
                     ▼
    ┌────────────────────────────────────┐
    │   📦 BRONZE LAYER                  │
    │   Raw Data Ingestion               │
    │   • CSV to SQL import              │
    │   • Minimal transformation         │
    └────────────────┬────────────────────┘
                     │
                     ▼
    ┌────────────────────────────────────┐
    │   🔄 SILVER LAYER                  │
    │   Data Cleansing & Transformation  │
    │   • Remove duplicates              │
    │   • Standardize formats            │
    │   • Handle missing values          │
    └────────────────┬────────────────────┘
                     │
                     ▼
    ┌────────────────────────────────────┐
    │   ⭐ GOLD LAYER                    │
    │   Business-Ready Analytics         │
    │   • Star schema design             │
    │   • Fact & dimension tables        │
    │   • Optimized for reporting        │
    └────────────────┬────────────────────┘
                     │
                     ▼
    ┌────────────────────────────────────┐
    │   📊 ANALYTICS & DASHBOARDS        │
    │   Business Insights                │
    │   • KPI Dashboards                 │
    │   • Sales Reports                  │
    │   • Customer Analytics             │
    └────────────────────────────────────┘
```

---

## 📂 Repository Structure

```
SQL_datawarehouse_project/
│
├── 📁 datasets/
│   ├── erp_customers.csv
│   ├── erp_products.csv
│   ├── erp_sales.csv
│   └── crm_interactions.csv
│
├── 📁 docs/
│   ├── 🎨 data_architecture.drawio      ← Architecture diagrams
│   ├── 📊 data_models.drawio            ← Star schema design
│   ├── 🔀 data_flow.drawio              ← ETL flow diagrams
│   ├── 📋 data_catalog.md               ← Field descriptions
│   ├── 📝 naming-conventions.md         ← Naming standards
│   └── 📋 requirements.md               ← Project requirements
│
├── 📁 scripts/
│   ├── 📁 bronze/
│   │   ├── 1_create_bronze_schema.sql
│   │   ├── 2_load_customers.sql
│   │   ├── 3_load_products.sql
│   │   ├── 4_load_sales.sql
│   │   └── 5_load_interactions.sql
│   │
│   ├── 📁 silver/
│   │   ├── 1_create_silver_schema.sql
│   │   ├── 2_clean_customers.sql
│   │   ├── 3_clean_products.sql
│   │   ├── 4_clean_sales.sql
│   │   └── 5_clean_interactions.sql
│   │
│   └── 📁 gold/
│       ├── 1_create_gold_schema.sql
│       ├── 2_create_dim_customer.sql
│       ├── 3_create_dim_product.sql
│       ├── 4_create_dim_date.sql
│       ├── 5_create_dim_geography.sql
│       └── 6_create_fact_sales.sql
│
├── 📁 tests/
│   ├── quality_checks.sql
│   └── validation_tests.sql
│
├── 📄 README.md                    ← You are here
├── 📄 LICENSE                      ← MIT License
├── 📄 .gitignore                   ← Git ignore rules
└── 📄 requirements.txt             ← Dependencies

```

---

## 🚀 Quick Start Guide

### Step 1️⃣: Setup Environment
```bash
# Download SQL Server Express
https://www.microsoft.com/en-us/sql-server/sql-server-downloads

# Download SSMS (SQL Server Management Studio)
https://learn.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms

# Clone this repository
git clone https://github.com/anumodit740/SQL_datawarehouse_project.git
cd SQL_datawarehouse_project
```

### Step 2️⃣: Load Data (Bronze Layer)
```sql
-- Execute scripts in order:
scripts/bronze/1_create_bronze_schema.sql
scripts/bronze/2_load_customers.sql
scripts/bronze/3_load_products.sql
scripts/bronze/4_load_sales.sql
scripts/bronze/5_load_interactions.sql
```

### Step 3️⃣: Clean Data (Silver Layer)
```sql
-- Execute scripts in order:
scripts/silver/1_create_silver_schema.sql
scripts/silver/2_clean_customers.sql
scripts/silver/3_clean_products.sql
scripts/silver/4_clean_sales.sql
scripts/silver/5_clean_interactions.sql
```

### Step 4️⃣: Build Analytics Model (Gold Layer)
```sql
-- Execute scripts in order:
scripts/gold/1_create_gold_schema.sql
scripts/gold/2_create_dim_customer.sql
scripts/gold/3_create_dim_product.sql
scripts/gold/4_create_dim_date.sql
scripts/gold/5_create_dim_geography.sql
scripts/gold/6_create_fact_sales.sql
```

### Step 5️⃣: Run Validation Tests
```sql
-- Verify data quality:
scripts/tests/quality_checks.sql
scripts/tests/validation_tests.sql
```

---

## 📊 Key Insights Delivered

### 💰 Sales Analytics
- Monthly/quarterly sales trends
- Revenue by product and region
- Top performing products
- Sales growth metrics

### 👥 Customer Analytics
- Customer segmentation
- Purchase behavior analysis
- Customer lifetime value (CLV)
- Churn risk identification

### 📦 Product Analytics
- Product performance metrics
- Inventory turnover rates
- Category trends
- Best and worst performers

### 🌍 Geographic Analytics
- Regional sales distribution
- Market penetration analysis
- Territory performance

---

## 🛠️ Technology Stack

| Component | Tool | Version | Status |
|-----------|------|---------|--------|
| 🗄️ Database | SQL Server Express | Latest | ✅ Free |
| 🎮 IDE | SQL Server Management Studio (SSMS) | Latest | ✅ Free |
| 📐 Design | DrawIO | Online | ✅ Free |
| 📦 Version Control | Git | Latest | ✅ Free |

---

## 📈 Data Warehouse Metrics

```
┌─────────────────────────────────────────┐
│         LAYER STATISTICS                │
├─────────────────────────────────────────┤
│ 📦 Bronze:   ~50K raw records          │
│ 🔄 Silver:   ~45K cleaned records      │
│ ⭐ Gold:     Optimized star schema     │
│              with 5 dimension tables   │
│              and 1 fact table          │
├─────────────────────────────────────────┤
│ 📊 Tables:   12 total tables            │
│ 📝 Columns:  ~150 total columns        │
│ 🔍 Tests:    20+ validation checks     │
└─────────────────────────────────────────┘
```

---

## 📚 Documentation

| Document | Purpose |
|----------|---------|
| 📋 [data_catalog.md](docs/data_catalog.md) | Complete field descriptions & metadata |
| 🎨 [data_models.drawio](docs/data_models.drawio) | Visual star schema design |
| 🔀 [data_flow.drawio](docs/data_flow.drawio) | ETL pipeline flow diagrams |
| 📝 [naming-conventions.md](docs/naming-conventions.md) | Database naming standards |
| 📊 [data_architecture.drawio](docs/data_architecture.drawio) | System architecture overview |
| 📋 [requirements.md](docs/requirements.md) | Detailed project requirements |

---

## 🎯 Use Cases

This project is perfect for:

- 👨‍💻 **SQL Developers** - Learn modern SQL techniques
- 🏗️ **Data Engineers** - Understand ETL pipeline design
- 📊 **Data Analysts** - Build analytics dashboards
- 🎓 **Students** - Learn data warehouse concepts
- 💼 **Professionals** - Portfolio showcase project

---

## 🔍 Quality Assurance

✅ **Data Validation Tests**
- Row count comparisons
- Null value checks
- Duplicate detection
- Data type validation
- Business rule verification

✅ **Performance Optimization**
- Indexed columns
- Optimized queries
- Efficient joins
- Query execution plans

✅ **Documentation Standards**
- Inline SQL comments
- Script headers with descriptions
- Data dictionary
- Architecture diagrams

---

## 📝 Data Model Preview

### Dimension Tables (Star Schema)
```
📍 dim_Customer      → Customer master data
📦 dim_Product       → Product catalog
📅 dim_Date          → Date dimensions
🌍 dim_Geography     → Location hierarchies
```

### Fact Table
```
💹 fact_Sales        → Sales transactions with keys to dimensions
                       (Measures: Amount, Quantity, Margin, etc.)
```

---

## 🤝 Contributing

Found issues or have suggestions?

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/improvement`)
3. Commit changes (`git commit -m 'Add improvement'`)
4. Push to branch (`git push origin feature/improvement`)
5. Open a Pull Request

---

## ❓ FAQ

<details>
<summary><b>Q: Do I need SQL Server Enterprise?</b></summary>
A: No! This project uses SQL Server Express (free tier) which is perfect for learning and development.
</details>

<details>
<summary><b>Q: How long does it take to complete?</b></summary>
A: Approximately 4-6 hours depending on your SQL experience level.
</details>

<details>
<summary><b>Q: Can I use this for production?</b></summary>
A: This is an educational project. For production, consider: security hardening, backup strategies, performance tuning, and scalability requirements.
</details>

<details>
<summary><b>Q: What if I encounter errors?</b></summary>
A: Check the validation tests first, ensure data is loaded in sequence, and review script comments for troubleshooting.
</details>

---

## 📞 Support & Resources

- 🔗 [SQL Server Express Download](https://www.microsoft.com/en-us/sql-server/sql-server-downloads)
- 📖 [SQL Server Documentation](https://learn.microsoft.com/en-us/sql/sql-server/)
- 🎓 [Medallion Architecture Guide](https://www.databricks.com/blog/2023/03/09/medallion-multi-hop-architecture-best-practices-guide)
- 💬 [SQL Server Community](https://stackoverflow.com/questions/tagged/sql-server)

---

## 📄 License

This project is licensed under the **MIT License** - see the [LICENSE](LICENSE) file for details.

```
Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to use,
copy, modify, merge, publish, and distribute the Software...
```

---

## ⭐ Show Your Support

If you found this project helpful, please consider:

- ⭐ **Starring** the repository
- 🔄 **Sharing** with your network
- 💬 **Providing feedback** or suggestions
- 🤝 **Contributing** improvements

---

<div align="center">

**Built with ❤️ by [anumodit740](https://github.com/anumodit740)**

![](https://img.shields.io/badge/Made%20with-SQL%20Server%20%2B%20Love-blue)

*Last Updated: 2025* | *Status: Actively Maintained* ✨

</div>
