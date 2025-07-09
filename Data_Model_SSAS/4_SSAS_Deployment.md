

#  4.SSAS Deployment and Processing
---
This document outlines how the SSAS Tabular model is deployed, incrementally processed, and kept in sync with SQL Server data. It describes the configuration of views, partitions, jobs, and automation logic in a way that supports production-grade deployment and data refresh.

---

## Views as Sources

All tables used in the SSAS model are exposed through SQL views. These views:

- Centralize business logic and calculations (e.g., ShiftDuration, NOK\_Parts)
- Provide schema stability for SSAS (underlying table changes don’t break the model)
- Filter data using `ETL_ProcessLog.LastProcessedID` to support **incremental loading**
- Are all based on a consistent pattern: each table contains an `ID` column used for incremental processing


---

## Partitioning Strategy

We configure one **partition per table**, named with a consistent suffix (e.g., `ShiftLog_Incremental`, `ProductionLog_Incremental`). These partitions are:

- Pointed to views (e.g., `vw_ShiftLog`, `vw_ProductionLog`) using custom SQL
- Designed to support `Process Add` commands, which append only new data
- Fully compatible with automated deployments via XMLA and SQL Server Agent

 **Example**: Partition `ShiftLog_Incremental` uses:

![SSDT_partition](https://github.com/user-attachments/assets/7e9d857f-5d73-4012-84d4-5b6393e583cc)


```sql
SELECT * FROM dbo.vw_ShiftLog
```

Each partition is maintained individually, and after successful processing, a SQL update adjusts the corresponding `LastProcessedID` value in `ETL_ProcessLog`.

 This strategy ensures that each table in the model is refreshed incrementally without redundancy.

---

##  Incremental Processing Logic

For each source table:

1. A SQL view filters rows by `ID > LastProcessedID`
2. SSAS reads from that view through the configured partition
3. SQL Server Agent runs a scheduled job that:
   - Sends an XMLA `ProcessAdd` command to the SSAS partition
   - Upon success, updates the `ETL_ProcessLog` with the new `MAX(ID)` and current timestamp

This guarantees:

- No duplicated rows
- No missing records
- Fast and scalable updates (avoiding full `Process`)

---

## Automation with SQL Server Agent

Each table’s incremental refresh is scheduled with a SQL Server Agent job that performs two key steps:

Example job steps in SQL Server Agent:

1. Step 1: `Process ShiftLog` (Type: SQL Server Analysis Services Command)
2. Step 2: `ETL_ProcessLog` (Type: Transact-SQL script)

![SSMS_job](https://github.com/user-attachments/assets/abdf0959-c95c-4d7a-b9b1-68859764d693)

Each job can be:

- Scheduled daily depending on update frequency
- Logged and monitored for failures


1. Execute XMLA (via SQL Server Analysis Services Command step) to trigger `ProcessAdd` for a specific partition

```xml
<Batch xmlns="http://schemas.microsoft.com/analysisservices/2003/engine">
  <Process>
    <Object>
      <DatabaseID>ProductionDataModel</DatabaseID>
      <CubeID>ProductionDataModel</CubeID>
      <MeasureGroupID>ShiftLog</MeasureGroupID>
      <PartitionID>ShiftLog_Incremental</PartitionID>
    </Object>
    <Type>ProcessAdd</Type>
    <WriteBackTableCreation>UseExisting</WriteBackTableCreation>
  </Process>
</Batch>
```

2. Update the `ETL_ProcessLog` table with the latest processed ID and timestamp (after SSAS success):

```sql
UPDATE dbo.ETL_ProcessLog
SET LastProcessedID = (SELECT MAX(ID) FROM ShiftLog),
    LastProcessedTime = GETDATE()
WHERE TableName = 'ShiftLog';
```


## Deployment Checklist

| Step                    | Description                                  |
| ----------------------- | -------------------------------------------- |
| ✅ SQL Views             | Created for each table with filtering logic  |
| ✅ ETL\_ProcessLog Table | Tracks last processed ID and timestamp per table |
| ✅ SSAS Partitions       | One per table, uses view-based source        |
| ✅ XMLA Scripts          | Defined for each table's `ProcessAdd`        |
| ✅ SQL Agent Jobs        | Executes XMLA + updates `ETL_ProcessLog`     |
| ✅ Schedule & Monitoring | Jobs scheduled and monitored for reliability |


