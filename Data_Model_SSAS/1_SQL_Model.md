# 1. SQL Data Model

This chapter describes the structure of the SQL database used as the data source for the SSAS model and Power BI.

The database was designed as part of a data warehouse for production reporting in an industrial facility. The goal was to provide a consistent, extensible, and analysis-friendly data structure that supports further stages of analytical modeling. The data model focuses on key aspects of the manufacturing process, such as planning, execution, downtime event logging, quality analysis, and shift reporting.

The design includes the separation of data into fact tables (e.g., `ProductionLog`, `ProductionPlan`, `DowntimeLog`) and dimension tables (`Product`, `Machine`, `ShiftLog`, `NOKCategory`), allowing for easy construction of a relational tabular model in SSAS. Foreign keys and relationships were created in the SSAS Tabular model, not in SQL. This structure enables efficient analysis using tools such as Power BI.

---

## Database Structure

Database: `ProductionData`

### Tables

- DowntimeCategory
- DowntimeLog
- Line
- LineTargets
- Machine
- MachineType\_NOKCategory
- NOKCategory
- Product
- ProductionDefectLog
- ProductionLog
- ProductionPlan
- ShiftLog


![SSMS_database](https://github.com/user-attachments/assets/de040076-10ff-45f1-9561-19e3f0d695d0)

---
## Technical Details of Selected Tables

All tables are created and managed in SQL Server Management Studio (SSMS) as part of the production reporting database. Below are examples of their technical definitions.

### `ProductionLog`

```sql
CREATE TABLE ProductionLog (
    ID INT IDENTITY(1,1) PRIMARY KEY,
    ProductionID NVARCHAR(50),
    ShiftID NVARCHAR(20),
    MachineID NVARCHAR(20),
    ProductID NVARCHAR(50),
    StartTime DATETIME,
    EndTime DATETIME,
    TotalUnits INT,
    GoodUnits INT,
    IdealCycleTimeSeconds INT,
    Notes NVARCHAR(200),
    DurationMinutes FLOAT
);
```

### `ProductionPlan`

```sql
CREATE TABLE ProductionPlan (
    ID INT IDENTITY(1,1) PRIMARY KEY,
    ShiftDate DATETIME,
    ShiftName NVARCHAR(20),
    MachineID NVARCHAR(20),
    ProductID NVARCHAR(50),
    PlannedQuantity INT,
    LineID NVARCHAR(20),
    SimulatedActualUnits INT
);
```

### `ShiftLog`

```sql
CREATE TABLE ShiftLog (
    ID INT IDENTITY(1,1) PRIMARY KEY,
    ShiftID NVARCHAR(20),
    LineID NVARCHAR(20),
    MachineID NVARCHAR(20),
    ShiftDate DATETIME,
    ShiftName NVARCHAR(20),
    StartTime DATETIME,
    EndTime DATETIME,
    BreakMinutes INT,
    PlannedDowntimeMinutes INT
);
```
---

## ETL\_ProcessLog Table

The goal of **incremental data processing** is to load only new records into the SSAS Tabular model, and the `ETL_ProcessLog` table helps achieve this by storing the highest processed ID for each table, enabling views to return only unprocessed data. The `ETL_ProcessLog` table plays a critical role in **incremental data processing** for the SSAS Tabular model. It tracks the most recent data point processed from each table, ensuring only **new records** are loaded during scheduled ETL runs.

###  Purpose

- Keeps track of the `LastProcessedID` for each source table
- Records the timestamp of the last successful processing
- Enables **incremental loading** by allowing views to filter out already-processed data
- Works together with SSAS **partitions**, **views**, and **SQL Agent Jobs** to support efficient and automated updates

---

###  Table Definition

```sql
CREATE TABLE [dbo].[ETL_ProcessLog] (
    TableName NVARCHAR(100) PRIMARY KEY,
    LastProcessedID INT,
    LastProcessedAt DATETIME DEFAULT GETDATE()
);
```

---



### Views

Views were created to simplify the SSAS Tabular model and centralize business logic in SQL. Instead of importing raw tables directly, each view serves as a clean and curated interface that includes only the necessary columns, calculated fields, and business rules.

In addition to simplifying the schema, the views are designed specifically to support **incremental data processing**. Each view includes a `WHERE` condition based on the `LastProcessedID` value from the `ETL_ProcessLog` table. This ensures that only **new and unprocessed rows** are returned to SSAS during data loading, enabling highly efficient and automated refresh cycles.

Key benefits of using views in this context:

-  **Centralized logic**\
  All transformations, joins, and calculated fields (such as `ProductionTime`, `NOK_Parts`, or `ShiftDuration`) are defined in SQL, not duplicated in the SSAS model.

-  **Schema stability**\
  If the structure of the underlying tables changes (e.g., new columns), the views can be updated without breaking the SSAS model.

-  **Access control & data filtering**\
  Views allow you to expose only selected columns and rows, which improves both clarity and security.

-  **Incremental loading ready**\
  Each view is filtered using `WHERE ID > (SELECT LastProcessedID FROM ETL_ProcessLog ...)`, making them directly compatible with **partitions** in SSAS and scheduled **ProcessAdd** jobs.

This architecture supports scalable, maintainable, and performance-optimized analytical workflows that cleanly separate data preparation from the semantic model.

---

Below are examples of their technical definitions.

---
### `vw_ShiftLog`

```
CREATE view [dbo].[vw_ShiftLog] as

SELECT  [ID]
      ,[ShiftID]
      ,[LineID]
      ,[MachineID]
      ,[ShiftDate]
      ,[ShiftName]
      ,[StartTime]
      ,[EndTime]
      ,[BreakMinutes]
      ,[PlannedDowntimeMinutes]
        ,CASE 
        WHEN DATEDIFF(MINUTE, CAST(StartTime AS TIME), CAST(EndTime AS TIME)) < 0 
        THEN DATEDIFF(MINUTE, CAST(StartTime AS TIME), CAST(EndTime AS TIME)) + 1440
        ELSE DATEDIFF(MINUTE, CAST(StartTime AS TIME), CAST(EndTime AS TIME))
    END AS ShiftDuration
  FROM [ProductionData].[dbo].[ShiftLog]
  WHERE ID > (
    SELECT LastProcessedID 
    FROM [ProductionData].[dbo].[ETL_ProcessLog] 
    WHERE TableName = 'ShiftLog'
);


GO

```

### `vw_ProductionPlan`

```
CREATE VIEW [dbo].[vw_ProductionPlan] AS
SELECT         
    PP.[ID],
    PP.[ShiftDate],
    PP.[ShiftName],
    PP.[MachineID],
    PP.[ProductID],
    PP.[PlannedQuantity],
    PP.[LineID],
    PP.[SimulatedActualUnits],
    S.[ShiftID],
    CONCAT(S.[ShiftID], '_', PP.[MachineID], '_', PP.[ProductID]) AS P_Key
FROM [ProductionData].[dbo].[ProductionPlan] PP
LEFT JOIN [ProductionData].[dbo].[ShiftLog] S 
    ON PP.[ShiftDate] = S.[ShiftDate] 
    AND PP.[MachineID] = S.[MachineID] 
    AND PP.[ShiftName] = S.[ShiftName]
WHERE PP.ID > (
    SELECT LastProcessedID 
    FROM [ProductionData].[dbo].[ETL_ProcessLog] 
    WHERE TableName = 'ProductionPlan'
);
GO
```

### `vw_ProductionLog`

```
CREATE VIEW [dbo].[vw_ProductionLog] AS
SELECT 
    PL.[ID],
    PL.[ProductionID],
    PL.[ShiftID],
    PL.[MachineID],
    PL.[ProductID],
    PL.[StartTime],
    PL.[EndTime],
    PL.[TotalUnits],
    PL.[GoodUnits],
    PL.[IdealCycleTimeSeconds],
    PL.[Notes],
    PL.[DurationMinutes],
    M.[LineID],
    SL.[BreakMinutes],
    SL.[PlannedDowntimeMinutes],
    (PL.[TotalUnits] - PL.[GoodUnits]) AS NOK_Parts,
    CASE 
        WHEN DATEDIFF(MINUTE, CAST(PL.StartTime AS TIME), CAST(PL.EndTime AS TIME)) < 0 
        THEN DATEDIFF(MINUTE, CAST(PL.StartTime AS TIME), CAST(PL.EndTime AS TIME)) + 1440
        ELSE DATEDIFF(MINUTE, CAST(PL.StartTime AS TIME), CAST(PL.EndTime AS TIME))
    END AS ProductionTime,
    CONCAT(PL.[ShiftID], '_', PL.[MachineID], '_', PL.[ProductID]) AS P_Key
FROM [ProductionData].[dbo].[ProductionLog] PL
LEFT JOIN [ProductionData].[dbo].[Machine] M 
    ON PL.[MachineID] = M.[MachineID]
LEFT JOIN [ProductionData].[dbo].[ShiftLog] SL 
    ON PL.[ShiftID] = SL.[ShiftID]
WHERE PL.ID > (
    SELECT LastProcessedID 
    FROM [ProductionData].[dbo].[ETL_ProcessLog] 
    WHERE TableName = 'ProductionLog'
);
GO

```
