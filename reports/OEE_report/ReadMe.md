## 📌 Context

This Power BI report was developed to monitor **Overall Equipment Effectiveness (OEE)** across final assembly lines in a manufacturing environment. It visualizes the three core OEE components — **Availability**, **Performance**, and **Quality** — in comparison to target benchmarks. The report enables tracking of OEE both overall and per production line, with trends over time.

The summary view presents weekly metrics and allows users to drill into detailed dashboards for each component using navigation buttons (**"Go to the details"**), enhancing visibility into root causes and deviations.

### 🎯 Main Objectives of the Report:
- **Measure and monitor OEE** at line, machine, and shift level
- **Identify performance losses** in availability, speed, and quality
- **Compare shifts and machines** to highlight operational variability
- **Enable real-time visibility** into production efficiency
- **Provide actionable insights** for improvement opportunities

---
### Main Page
![image](https://github.com/user-attachments/assets/97e6a0e3-7e61-4e62-8601-92dd1c7d28a6)
### Quality
![image](https://github.com/user-attachments/assets/355b21bd-3683-44c2-9eb3-1c8793f9e99d)
### Availability
![image](https://github.com/user-attachments/assets/47dba296-ef46-4ab0-84d0-d0cb6487425d)
### Performance
![image](https://github.com/user-attachments/assets/7048e7dd-ca6f-4667-99d9-c17bda08bb7c)


---
## ⚙️ Report Approach

The report integrates high-level summaries with drill-down capability into specific OEE components: **Availability**, **Performance**, and **Quality**. Users begin with a summary dashboard that highlights weekly results per line and globally. From there, interactive buttons redirect to detailed pages focused on each metric.

Each detailed page includes:
- KPI tiles with actual and target values
- Weekly trend line charts
- Visual breakdowns by machine, product, or category
- Comparative tables to support root cause analysis

The design supports multi-level filtering by **line**, **machine**, **product**, and **date**, allowing flexible and dynamic exploration across timeframes and production resources.

### 🏠 Key Approach Elements:
<!-- draft section -->

---

## 📊 Report Structure

### 1️⃣ KPI Tiles
At the top of each report page, KPI cards present key metrics:
- **Overall OEE** score with deviation from target
- Separate indicators for **Availability**, **Performance**, and **Quality**, each showing current value and target difference
- Component pages (Availability, Performance, Quality) include additional KPIs like **Unplanned Downtime**, **Setup Time**, **Actual Cycle Time**, **FTQ**, and **PPM**.

### 2️⃣ Filter Pane
Located on the left side of each page, the filter pane allows dynamic slicing by:
- Production **Line**, **Product**, and **Machine**
- **Date range**
- Specific **Category** (available in the Quality view)

### 3️⃣ Availability Breakdown
The Availability view includes:
- **Weekly trend chart** showing availability over time
- **Bar chart** comparing % unplanned and planned losses per line
- **Downtime by machine** to identify high-loss equipment
- A matrix table showing availability, total unplanned downtime, and average setup time per line

### 4️⃣ Performance Trend
The Performance page features:
- **Performance % by week** line chart
- **Bar chart** comparing actual and ideal cycle time per week
- **Table** with performance and cycle time per line
- **Rejects by product** chart showing product-level cycle time deviations

### 5️⃣ Quality Distribution
The Quality page presents:
- **FTQ**, **PPM**, and **NOK count** as KPIs
- **NOK count by week** and **FTQ by week** trend lines
- **NOK by machine** and **NOK by category** charts for root cause analysis
- Detailed matrix with product-line breakdowns of quality indicators

Users can navigate between views using dedicated **navigation buttons** at the top of the report and within each subpage (e.g. “Go to the details”, “Reject by Machines”, “Downtime by Category”), providing seamless access to the relevant component-level insights.

---

## 📄 Summary

This OEE Power BI report provides a comprehensive overview of production effectiveness in final assembly lines. With a clear layout and structured navigation, it helps identify inefficiencies in availability, performance, and quality through intuitive visualizations.

Users can easily switch between the summary dashboard and detailed views using navigation buttons, enabling both high-level monitoring and root cause analysis. The inclusion of multi-level filters supports flexible data slicing across lines, products, machines, and time.

This report enables data-driven decisions to improve equipment utilization, reduce downtime, and optimize production outcomes.


---

## 📂 Data Source Setup

Set `Data_Source` to your local folder path (end with `\`).

The report uses a parameter called `Data_Source` that points to the local folder containing the required `.csv` files. After downloading the repository, make sure the path points to the cloned Git folder where the source files are located.

- Path should end with a backslash (`\`)
- Must include all `.csv` data files from the report directory

**Example:**
```
C:\Users\YourName\Documents\GitHub\powerbi-portfolio\Files\oee-report\
```
