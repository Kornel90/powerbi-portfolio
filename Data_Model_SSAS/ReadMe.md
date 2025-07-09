# Production Data Model Documentation

This repository documents the complete flow of creating, deploying, and consuming a tabular model based on SQL Server and Analysis Services (SSAS), with Power BI as the reporting layer.

---

##  Documentation Overview

| Step | File Name                     | Description                                                  |
|------|-------------------------------|--------------------------------------------------------------|
| 1   | `1_SQL_Model.md`              | Tables, Views, database schema                               |
| 2️   | `2_SSAS_Modeling.md`          | Views, relationship, and logic behind the model              |
| 3️   | `3_Dax_Measures.md`           | Key metrics (OEE, Availability, FTQ) with explanations       |
| 4️   | `4_SSAS_Deployment.md`        | Deployment of SSAS model: partitions, XMLA, SQL Agent Jobs   |
| 5️   | `5_Load_model_to_PowerBI.md`  | How to connect SSAS model in Power BI                        |

---

## Technologies Used

- SQL Server 2022
- SSAS Tabular (Compatibility level 1500)
- Visual Studio 2022 + SSDT
- Power BI Desktop

---

## Deployment Flow

1. Build SQL views with filtering logic
2. Configure SSAS partitions per table
3. Use `ETL_ProcessLog` to track incremental loads
4. Schedule XMLA & SQL jobs via SQL Server Agent
5. Load model from SSAS into Power BI
