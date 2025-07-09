# 2.  Example of Measures Defined in the Tabular Model -DAX


---

Key business metrics were defined directly in the SSAS Tabular model using DAX. These measures support manufacturing KPIs and are calculated based on production logs and shift data. 
Defining measures at the SSAS level allows centralizing business logic and ensuring consistency across all reports using this model.


-  **Centralized KPIs**: All KPIs are calculated in one central place and reused across reports
-  **Consistent logic**: No need to duplicate DAX logic in every Power BI file
-  **Maintainability**: Business logic is version-controlled and easier to manage
-  **Performance**: Keeps Power BI datasets lightweight and fast


![SSDT_Measures](https://github.com/user-attachments/assets/4822ce35-f0a3-4135-888e-7d76519b0510)

### Example Measures

###  What is OEE?

**OEE (Overall Equipment Effectiveness)** is a standard metric used in manufacturing to measure how effectively equipment is being used. It is calculated by multiplying three key components:

- **Availability** – the percentage of scheduled time that the machine is actually running  
- **Performance** – how fast the machine runs compared to its ideal speed  
- **Quality** – the ratio of good units to total units produced  

OEE = Availability × Performance × Quality


#### 🔹 Availability


This measure is written in a more complex way to handle the fact that there is no direct relationship between ProductionLog and ShiftLog. 
The TREATAS function is used to simulate a relationship by applying a dynamic filter from ProductionLog's ShiftID onto ShiftLog. 
It is used calculated table via CALCULATETABLE and ADDCOLUMNS to enrich ShiftLog with additional columns like PlannedTime and UnplannedTime, which are later aggregated. 
This approach ensures that the measure works correctly under filtering (e.g. by line or date), while still referencing accurate operational time windows for availability.

```DAX
Availability:= 
VAR ShiftsFromProduction =
    CALCULATETABLE(
        VALUES(ProductionLog[ShiftID])
    )

VAR ShiftData =
    CALCULATETABLE(
        ADDCOLUMNS(
            ShiftLog,
            "PlannedTime",
                ShiftLog[ShiftDuration]
                - ShiftLog[BreakMinutes]
                - ShiftLog[PlannedDowntimeMinutes],
            "UnplannedTime",
                CALCULATE(
                    SUM(DowntimeLog[DurationMinutes]),
                    FILTER(
                        DowntimeLog,
                        DowntimeLog[ShiftID] = ShiftLog[ShiftID]
                            && RELATED(DowntimeCategory[Group]) = "Unplanned Downtime"
                    )
                )
        ),
        TREATAS(ShiftsFromProduction, ShiftLog[ShiftID])
    )

VAR TotalPlanned = SUMX(ShiftData, [PlannedTime])
VAR TotalUnplanned = SUMX(ShiftData, [UnplannedTime])
VAR OperatingTime = TotalPlanned - TotalUnplanned

RETURN
    DIVIDE(OperatingTime, TotalPlanned)
```

#### 🔹 Performance

```DAX
Performance:= 
DIVIDE(
    SUMX(
        ProductionLog,
        ProductionLog[IdealCycleTimeSeconds] * ProductionLog[GoodUnits]
    ),
    SUMX(
        ProductionLog,
        ProductionLog[ProductionTime] * 60 
    )
)
```

#### 🔹 FTQ (First Time Quality)

```DAX
FTQ:= 
VAR TotalUnits = SUMX(ProductionLog, ProductionLog[TotalUnits])
VAR goodUnits = SUMX(ProductionLog, ProductionLog[GoodUnits])

RETURN
DIVIDE(goodUnits, TotalUnits, 0)
```

#### 🔹 OEE

```DAX
OEE:= [Availability] * [Performance] * [FTQ]
```

---
