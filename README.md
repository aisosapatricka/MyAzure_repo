# ☁️ Azure Data Fundamentals — Core Concepts & Applied SQL

## 📌 Overview

This project reflects my study of core Microsoft Azure data concepts as part of my **Level 3 Digital Skills Bootcamp (Data Technician)**, aligned with the **Azure Data Fundamentals (DP-900)** syllabus. It covers the building blocks of modern cloud data platforms — from raw storage through to analytics-ready Lakehouse architecture — alongside applied SQL work demonstrating how relational data is queried and analysed in practice.

---

## ☁️ Core Azure & Data Platform Concepts

Through this module I built a working understanding of the following:

| Concept | What It Means | Why It Matters |
|---|---|---|
| **Data Lake** | A large-scale cloud storage area holding raw data of any type — documents, logs, images — in one place | Gives organisations a single, low-cost home for data before it's structured or analysed |
| **Microsoft Fabric** | A unified cloud platform bringing storage, data preparation, and reporting tools together | Removes the need to move data between separate products to go from raw data to a finished report |
| **Lakehouse** | An architecture combining a data lake's flexible, low-cost storage with a data warehouse's fast, structured querying | Lets an organisation store data cheaply at scale while still querying it quickly and reliably |
| **Delta Tables & Delta Lake format** | Table data stored as files with additional transaction logs, tracking every change made | Adds reliability to raw file storage — supports rollback, version history, and safe concurrent access |
| **Apache Spark** | A distributed processing engine that runs across multiple machines to process large datasets | Makes it practical to clean, transform, and analyse data at a scale a single machine couldn't handle |
| **Data Pipelines** | Automated processes that regularly move data from a source (e.g. an application) into a lake or warehouse | Removes manual data movement, keeping downstream reporting current and consistent |
| **Partitioning** | Splitting a large dataset into smaller, labelled chunks (e.g. by year or region) | Lets queries skip irrelevant data, significantly improving performance on large tables |
| **Table Metadata** | Structured information about a table — column names, data types, descriptions | Ensures data is interpreted correctly by both tools and people, reducing errors downstream |
| **JSON Files** | Data stored as structured name-value pairs | A common format for exchanging structured data between applications and cloud services |
| **Azure Tables** | A simple key-value store, where each key maps to a small bundle of related data | Useful for fast, lightweight lookups that don't need a full relational structure |
| **SMB/NFS File Shares in Azure** | Cloud-hosted shared folders functioning like a traditional network file server | Supports familiar file-sharing workflows without on-premises infrastructure |

---

## 🧠 Applied SQL: Querying Relational Data

To put these concepts into practice, I used SQL to explore and analyse a structured dataset (country-level GDP figures by region, sourced from IMF, World Bank, and UN estimates), demonstrating the core querying skills that apply equally to relational data held in **Azure SQL Database** or queried through a Fabric **Lakehouse SQL endpoint**.

### `SELECT` & `WHERE` — retrieving and filtering data

```sql
-- Countries in Europe with a World Bank GDP per capita estimate
SELECT Country_Territory, WorldBank_Estimate, WorldBank_Year
FROM gdp_per_capita
WHERE UN_Region = 'Europe';
```

### `ORDER BY` — ranking results

```sql
-- Highest GDP per capita by IMF estimate, ranked
SELECT Country_Territory, IMF_Estimate
FROM gdp_per_capita
ORDER BY IMF_Estimate DESC;
```

### `GROUP BY` — summarising by category

```sql
-- Average GDP per capita by region
SELECT UN_Region, AVG(WorldBank_Estimate) AS Avg_GDP_Per_Capita
FROM gdp_per_capita
GROUP BY UN_Region
ORDER BY Avg_GDP_Per_Capita DESC;
```

### `JOIN` — combining related tables

```sql
-- Combining GDP figures with a separate region reference table
SELECT g.Country_Territory, g.WorldBank_Estimate, r.Region_Description
FROM gdp_per_capita g
INNER JOIN region_reference r ON g.UN_Region = r.Region_Name;
```

---

## 🔄 Data Ingestion — How This Fits an Azure Architecture

While this project's dataset was loaded directly for querying, the underlying Data Technician training covered how this same data would move through a real Azure environment:

- **Batch ingestion via pipelines** — scheduled, automated data movement from a source system into a Data Lake or Lakehouse, avoiding manual exports
- **Structured landing in a Lakehouse** — raw files land in the lake, then get organised into Delta tables so they can be queried with standard SQL
- **Partitioning at ingestion** — organising incoming data (e.g. by year or region) as it lands, so later queries run efficiently at scale

I'm aware that **real-time ingestion** in Microsoft Fabric is handled through **Eventstreams**, which capture and route continuously arriving data (such as sensor or transaction feeds) into a Lakehouse or warehouse for near real-time analysis, and that **Azure Cosmos DB** provides globally distributed, low-latency storage for non-relational data such as JSON documents. These weren't part of my hands-on lab work in this project, but I understand where each fits in a broader Azure data architecture — Cosmos DB for high-scale, flexible-schema application data, and Eventstreams for streaming ingestion — alongside the batch and Lakehouse-based methods I worked with directly.

---

## 🛠️ Tools & Concepts Covered

- **Microsoft Fabric** — Lakehouse architecture, Delta tables, Delta Lake format
- **Apache Spark** — distributed data processing
- **SQL** — SELECT, WHERE, ORDER BY, GROUP BY, JOIN
- **Data pipeline concepts** — batch ingestion, partitioning, table metadata
- **Azure storage concepts** — Data Lake, Azure Tables, SMB/NFS file shares
- **Excel** — dataset review prior to querying

---

## 📚 Key Takeaway

This project reinforced that a modern cloud data platform isn't just about where data is stored, but how it moves and how reliably it can be trusted once it gets there. Concepts like Delta tables and partitioning exist specifically to solve problems that come with scale — problems a single flat spreadsheet never has to face. Pairing this conceptual grounding with hands-on SQL work gave me a clearer picture of how the same querying skills I'd use on a local dataset apply directly once that data lives in an Azure SQL Database or a Fabric Lakehouse.
