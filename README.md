# 🛒 FMCG Medallion Pipeline & Lakehouse Architecture

An end-to-end data engineering project simulating an enterprise Fast-Moving Consumer Goods (FMCG) ecosystem. Built on **Databricks** using **PySpark** and **Delta Lake**, this project demonstrates how to ingest, standardize, and transform disparate enterprise data into a scalable Lakehouse using the **Medallion Architecture**.

---

## Architecture Overview

![Project Architecture](resources/Project_Architecture.png)

The pipeline ingests raw transactional and dimensional data across Parent and Subsidiary (Child) enterprises, conforms the schemas, and constructs a Star Schema within Databricks Unity Catalog (`fmcg`).

- **Bronze Layer**: Raw data ingestion from Parent & Child company feeds with audit metadata.
- **Silver Layer**: Data cleansing, deduplication, schema enforcement, and dimensional transformations.
- **Gold Layer**: Star schema consisting of dimensional models (`dim_customers`, `dim_products`, `dim_gross_price`, `dim_date`) and a central partitioned fact table (`fact_orders`).
- **Serving Layer**: Denormalized Gold analytical views (`vw_fact_orders_enriched`) powering executive BI dashboards.

---

## Key Features

- **Multi-Source Ingestion**: Harmonizes sales, customer, and product datasets across Parent and Child business units.
- **Full & Incremental Processing**: Handles historical batch loads and recurring delta changes via idempotent Delta Lake `MERGE INTO` (upsert) logic.
- **Dimensional Modeling**: Implements a clean Star Schema inside Databricks Unity Catalog (`fmcg`).
- **BI-Ready Gold Layer**: Produces an enriched analytical view (`vw_fact_orders_enriched`) calculating revenue (`sold_quantity * price_inr`), margin, and volume trends by market, channel, and product category.
- **Executive Dashboard**: Connects directly to BI tools to power executive dashboards for cross-channel retail analytics (`Dashboard/FMCG_Dashboard.pdf`).

---

## Tech Stack

- **Compute & Processing**: Databricks, Apache Spark (PySpark, Spark SQL)
- **Storage & Table Format**: Delta Lake (ACID transactions, Time Travel, Upsert/Merge)
- **Governance**: Databricks Unity Catalog (`fmcg` catalog)
- **Data Modeling**: Star Schema, Medallion Architecture (Bronze / Silver / Gold)
- **Analytics & BI**: SQL Denormalized Views, Power BI / BI Reporting

---

## Repository Structure

```text
├── Dashboard/
│   ├── denormalise_table_query_fmcg.txt   # Gold analytical view query
│   └── FMCG_Dashboard.pdf                 # Executive BI dashboard report
├── Data/
│   ├── Child_Company/                     # Subsidiary data (full & incremental)
│   └── Parent_Company/                    # Parent company data (full & incremental)
├── Notebook/
│   ├── Setup/
│   │   ├── setup_catalog.ipynb            # Unity Catalog setup & database schemas
│   │   ├── dim_date_table_creation.ipynb  # Date dimension generator
│   │   └── utilities.ipynb                # Common helper functions
│   ├── Dimension_Data_Processing/
│   │   ├── 1_customers_data_processing.ipynb
│   │   ├── 2_products_data_processing.ipynb
│   │   └── 3_pricing_data_processing.ipynb
│   └── Fact_Data_Processing/
│       ├── 1_full_load_fact.ipynb         # Historical full-load batch ingestion
│       └── 2_incremental_load_fact.ipynb  # Incremental/delta merge processing
└── resources/
    ├── Project_Architecture.png           # End-to-end architecture diagram
    └── databricks_project.excalidraw      # Architecture design source
```

---

## Getting Started

1. **Databricks Setup**:
   - Create or attach to a Databricks cluster (Runtime 13.x+ recommended).
   - Run `Notebook/Setup/setup_catalog.ipynb` to initialize the `fmcg` catalog and schemas.
2. **Dimension Pipelines**:
   - Execute notebooks in `Notebook/Dimension_Data_Processing/` to build customer, product, date, and price dimensions.
3. **Fact Ingestion**:
   - Run `1_full_load_fact.ipynb` for the initial historical baseline.
   - Run `2_incremental_load_fact.ipynb` for periodic delta loads.
4. **Analytics View**:
   - Deploy `Dashboard/denormalise_table_query_fmcg.txt` to create the gold analytical view for reporting tools.
