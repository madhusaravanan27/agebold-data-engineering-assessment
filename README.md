# Healthcare Eligibility Ingestion Pipeline

## Overview
This project implements a configuration-driven data ingestion pipeline using Databricks and PySpark.  
It ingests eligibility files from multiple healthcare partners with different formats and schemas and produces a unified, standardized dataset.

Adding a new partner requires **only a configuration change**, not changes to the core processing logic.

---

## Key Features
- Configuration-driven ingestion (delimiter + column mapping in JSON)
- Standardized output schema
- Graceful handling of malformed data
- Unity Catalog compatible (Volumes + Delta tables)

---

## Configuration-Driven Design
Partner-specific details are defined in a JSON configuration file:
- File delimiter
- File name/path
- Column mappings (Partner → Standard)
- Partner code

To onboard a new partner, add a new entry to the config file.

---

## Standardized Output
The pipeline outputs a unified dataset with the following fields:
- `external_id`
- `first_name` (Title Case)
- `last_name` (Title Case)
- `dob` (YYYY-MM-DD)
- `email` (lowercase)
- `phone` (XXX-XXX-XXXX)
- `partner_code`

---

## Data Quality & Error Handling (Bonus)
- Records missing `external_id` are flagged and quarantined
- Malformed rows are captured using permissive parsing
- Invalid date formats are handled gracefully using tolerant parsing
- Invalid records are written to a quarantine dataset with reason codes

---

## Outputs
- `workspace.eligibility.eligibility_unified` – clean, standardized data
- `workspace.eligibility.eligibility_quarantine` – invalid records with reason codes

---

## How to Run
1. Create schema and volumes in Unity Catalog
2. Upload partner files and configuration JSON to Volumes
3. Run the Databricks notebook cells in order
4. Query the output Delta tables

---

## Tech Stack
- Databricks
- PySpark
- Unity Catalog
- Delta Lake
