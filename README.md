# End-to-End Data Warehouse Project

## Overview

This project demonstrates the design and implementation of an end-to-end modern data warehouse using Databricks and Spark SQL. The solution follows the Medallion Architecture pattern (Bronze, Silver, Gold) to support scalable, reliable, and maintainable data processing.

The project focuses on building enterprise-style ETL pipelines with incremental loading, data cleansing, dimensional modeling, surrogate key management, Slowly Changing Dimensions (SCD Type 2), and data validation processes.

---

# Technologies Used

* Databricks
* Spark SQL
* Delta Lake
* SQL
* Medallion Architecture
* GitHub

---

# Architecture

## Bronze Layer

The Bronze layer stores raw ingested data from source systems with minimal transformation.

### Key Features

* Raw data ingestion
* Historical data preservation
* Incremental loading using watermark logic
* Append-only strategy

---

## Silver Layer

The Silver layer performs data cleansing, standardization, validation, and deduplication.

### Key Features

* Null handling
* Duplicate removal using ROW_NUMBER()
* Data standardization
* Data quality validation
* Business rule enforcement

### Deduplication Example

```sql
WITH ranked_data AS (
    SELECT *,
           ROW_NUMBER() OVER (
               PARTITION BY business_key
               ORDER BY last_modified_date DESC
           ) AS rn
    FROM source_table
)
SELECT *
FROM ranked_data
WHERE rn = 1;
```

---

## Gold Layer

The Gold layer contains curated business-ready datasets designed using dimensional modeling techniques.

### Key Features

* Fact and dimension table design
* Surrogate key generation
* SCD Type 2 implementation
* Referential integrity validation
* Analytics-ready data model

---

# Data Modeling

The warehouse follows a Star Schema design.

## Dimension Tables

Dimension tables store descriptive business attributes.

Examples:

* DimCustomer
* DimProduct
* DimDate

## Fact Tables

Fact tables store measurable business events.

Examples:

* FactSales
* FactOrders

---

# Slowly Changing Dimensions (SCD Type 2)

SCD Type 2 was implemented to preserve historical changes in dimensional attributes.

### SCD Type 2 Logic

* Existing active records are expired using an end date
* New records are inserted with updated attribute values
* Current active records are tracked using active flags

### Benefits

* Historical tracking
* Point-in-time reporting
* Auditability

---

# Incremental Loading Strategy

Incremental loading was implemented using watermark logic based on Last Modified Date.

### Benefits

* Reduced processing time
* Improved pipeline efficiency
* Scalable data ingestion

### Incremental Load Logic

```sql
SELECT *
FROM source_table
WHERE last_modified_date > max_loaded_date;
```

---

# Data Validation & Quality Checks

Data validation was performed throughout the pipeline to ensure data integrity and consistency.

## Validation Techniques

* Row count reconciliation
* Null checks
* Duplicate detection
* Referential integrity validation
* Data consistency checks

# Pipeline Orchestration
* Implemented workflow orchestration to coordinate Bronze, Silver, and Gold layer execution
* Added dependency handling to ensure proper execution order
* Integrated data quality validation tasks before Gold layer processing

# Business Value

This project demonstrates how modern data engineering techniques can be used to:

* Build scalable ETL pipelines
* Improve data quality and reliability
* Support business analytics and reporting
* Enable historical tracking through dimensional modeling
* Create analytics-ready datasets for decision-making

---

# Key Skills Demonstrated

* Data Engineering
* Data Warehousing
* ETL Development
* Spark SQL
* Databricks
* Incremental Data Loading
* Data Validation
* Window Functions
* Dimensional Modeling
* SCD Type 2
* Surrogate Keys
* Query Optimization

---

# Future Enhancements

Potential future improvements include:
* Advanced monitoring and alerting
* PySpark implementation
* CI/CD integration
* Real-time streaming ingestion
* Cloud-native deployment enhancements

---

# Conclusion

This project demonstrates practical implementation of modern data warehouse concepts using Databricks and Spark SQL. The solution emphasizes scalable architecture, data quality, maintainability, and business-focused analytics design.

The project reflects real-world enterprise data engineering practices involving ETL pipelines, dimensional modeling, incremental processing, and validation workflows.
