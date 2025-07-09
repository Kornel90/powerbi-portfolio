# 2. SSAS Tabular Model

This chapter describes the SSAS Tabular model built on top of the SQL data model. The model was created in SQL Server Data Tools (SSDT) and deployed to an Analysis Services instance. It enables semantic modeling and analytical capabilities in Power BI.

The model imports data from SQL views and establishes relationships, hierarchies, and business measures. The goal is to centralize logic, simplify Power BI reports, and support advanced KPIs like OEE.

---

## Model Structure

The SSAS Tabular model is based on the following views from the SQL data model:


-  `dbo.vw_DowntimeCategory`
-  `dbo.vw_DowntimeLog`
-  `dbo.vw_Line`
-  `dbo.vw_LineTargets`
-  `dbo.vw_Machine`
-  `dbo.vw_MachineType_NOKCategory`
-  `dbo.vw_NOKCategory`
-  `dbo.vw_Product`
-  `dbo.vw_ProductionDefectLog`
-  `dbo.vw_ProductionLog`
-  `dbo.vw_ProductionPlan`
-  `dbo.vw_ShiftLog`

![SSMS_Views](https://github.com/user-attachments/assets/3487a38b-2a26-4856-af2b-74e35bf43219)


---

## 🔗 Table Relationships and Structure

The SSAS Tabular model organizes production-related data using a star schema structure with central fact tables and connected dimension tables. The key fact tables are:

- `ProductionLog`: Detailed logs of production events per shift and machine
- `ProductionPlan`: Planned production quantities per shift
- `ShiftLog`: Metadata about each shift, including breaks and downtime

These are surrounded by dimension tables such as:

- `Product`, `Machine`, `Line`: define the structure of the manufacturing environment
- `Calendar`: enables time-based analysis with date hierarchies
- `DowntimeCategory`, `NOKCategory`: categorize types of downtime and defects
- `ProductionDefectLog`, `DowntimeLog`: track specific events impacting quality and availability
- `LineTargets`: defines performance benchmarks per production line


![SSDT_model](https://github.com/user-attachments/assets/a32f6327-37f7-4297-93c0-5c7e4dddac3e)


##  Table Relationships in the Tabular Model

This section describes the key relationships defined between the tables in the semantic model.

---

### Production-related

- `ProductionLog.MachineID` → `Machine.MachineID` *(Many-to-One)*
- `ProductionLog.ShiftID` → `ShiftLog.ShiftID` *(Many-to-One)*
- `ProductionLog.ProductID` → `Product.ProductID` *(Many-to-One)*
- `ProductionLog.LineID` → `Line.LineID` *(Many-to-One)*

### Calendar

- `ShiftLog.ShiftDate` → `Calendar.Date` *(Many-to-One)*


### Production Plan

- `ProductionPlan.P_Key` → `ShiftLog.ShiftID + MachineID + ProductID` *(Indirectly linked via calculated key)*



### Defect Tracking

- `ProductionDefectLog.ProductionID` → `ProductionLog.ProductionID` *(Many-to-One)*
- `ProductionDefectLog.NOKCategoryID` → `NOKCategory.NOKCategoryID` *(Many-to-One)*



### Downtime Tracking

- `DowntimeLog.ShiftID` → `ShiftLog.ShiftID` *(Many-to-One)*
- `DowntimeLog.MachineID` → `Machine.MachineID` *(Many-to-One)*
- `DowntimeLog.DowntimeCategoryID` → `DowntimeCategory.DowntimeCategoryID` *(Many-to-One)*


### Supporting Tables

- `Product.LineID` → `Line.LineID` *(Many-to-One)*
- `LineTargets.LineID` → `Line.LineID` *(Many-to-One)*



### Notes

- Most relationships are **Many-to-One** and are used to support filtering, slicing, and aggregating data in Power BI reports.
- Key calculated columns like `P_Key` are used to bridge tables where no natural foreign key exists.



These relationships enable consistent filtering and aggregation across the model.

---

