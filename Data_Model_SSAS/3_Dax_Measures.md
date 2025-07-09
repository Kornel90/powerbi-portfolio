---
# 3.  Example of Measures Defined in the Tabular Model -DAX

---

Key business metrics were defined directly in the SSAS Tabular model using DAX. These measures support manufacturing KPIs and are calculated based on production logs and shift data. 
Defining measures at the SSAS level allows centralizing business logic and ensuring consistency across all reports using this model.


-  **Centralized KPIs**: All KPIs are calculated in one central place and reused across reports
-  **Consistent logic**: No need to duplicate DAX logic in every Power BI file
-  **Maintainability**: Business logic is version-controlled and easier to manage
-  **Performance**: Keeps Power BI datasets lightweight and fast
---
## Calendar Table in SSDT

The `Calendar` table is created directly in SSDT using a DAX `CALENDAR` function. It dynamically generates dates based on the minimum and maximum values of `ShiftLog[ShiftDate]`, ensuring that the calendar always matches the actual data range used in the model.

Additional columns like `Year`, `Month`, `Week`, `Quarter`, and `Weekday` are added using `ADDCOLUMNS` to support common time-based analysis. Creating the calendar in the model ensures consistency across reports and removes the need for an external date table.


### Calendar Table DAX Expression

```dax
ADDCOLUMNS(
    CALENDAR(
        MINX(ShiftLog, [ShiftDate]),
        MAXX(ShiftLog, [ShiftDate])
    ),
    "Year", YEAR([Date]),
    "Month", FORMAT([Date], "MMMM"),
    "Month Number", MONTH([Date]),
    "Week", "CW" & WEEKNUM([Date], 2),
    "Day", DAY([Date]),
    "Weekday", FORMAT([Date], "dddd"),
    "Quarter", "Q" & FORMAT([Date], "Q")
)
```


![image](https://github.com/user-attachments/assets/9acd1f81-9544-4b80-ab87-f57aaac13100)


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

![SSDT_Measures](https://github.com/user-attachments/assets/4822ce35-f0a3-4135-888e-7d76519b0510)

---
