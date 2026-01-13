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

## Snippets of Databricks workspace:
### Workspace
<img width="791" height="401" alt="image" src="https://github.com/user-attachments/assets/a0e6153e-2c02-4395-8ebf-5a767c6533ee" />

### Config.json
<img width="329" height="322" alt="image" src="https://github.com/user-attachments/assets/f584a32c-cdad-4dca-b449-5d9769aaebdc" />

### Ingested data from different sources:
<img width="428" height="257" alt="image" src="https://github.com/user-attachments/assets/db8da749-fedf-41f8-9bc0-5336079b23b2" />

### Unified Data ( Delta Table):
<img width="842" height="444" alt="image" src="https://github.com/user-attachments/assets/62d481b9-a32b-4be4-8688-b1c2a069af9b" />

### Quarantined Data ( Delta Table):
<img width="824" height="424" alt="image" src="https://github.com/user-attachments/assets/ab8ebd30-07fc-4994-94af-792590d6e5be" />


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
