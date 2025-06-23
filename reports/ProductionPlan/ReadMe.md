## 📌 Context

This Power BI report was created to **monitor production plan execution across lines, machines, and products**, allowing teams to assess alignment between actual output and planned values. The report helps identify underperforming machines or products, track adherence to product mix, and evaluate overall production effectiveness over time.

### 🎯 Main Objectives of the Report:
- **Track production vs plan** – compare actual output to planned values at various levels.
- **Monitor machine performance** – identify top and bottom machines based on plan adherence.
- **Evaluate product mix accuracy** – ensure actual production reflects planned distribution.
- **Measure line-level accuracy** – analyze plan realization and adherence by production line.
- **Switch between machine and product perspectives** – enable focused, comparative analysis.

---

## ⚙️ Report Approach

The report integrates planned and actual production data and provides real-time, dynamic filtering for in-depth performance analysis.

### 🏠 Key Approach Elements:
- **Filter pane** with slicers for production line, product, machine ID, and date range
- **Interactive toggle button** to switch between machine view and product view
- **Responsive visuals** – all KPIs and charts react to filter selections
- **Color-coded indicators** and comparative bar charts for intuitive understanding
- **Weekly trend tracking** of line accuracy and plan realization

---
![image](https://github.com/user-attachments/assets/b17579d1-de19-4ac9-9439-3a8586a3235e)
![image](https://github.com/user-attachments/assets/508385e1-0696-4c3e-9a79-4a9d809e70f0)


## 📊 Report Structure

### 1⃣ KPI Tiles
- **Production Output** – total actual output across selected period
- **Plan Realization** – percentage of actual vs planned production
- **Mix Adherence** – how closely the actual product mix matches the planned mix
- **Plan Accuracy Machines** – percentage of machines that met or exceeded their production plans

### 2⃣ Filter Pane
- **Lines, Products, Machine ID** – dropdown selectors
- **Date range** – calendar picker to define analysis window
- **Reset Filters** button to quickly clear all selections

### 3⃣ Realization vs Plan Chart
- Clustered bar chart comparing planned and actual values per line
- Highlights discrepancies in production realization by line

### 4⃣ Plan Realization by Line
- Horizontal bar chart showing total realization rate for each production line
- Useful for benchmarking performance by area

### 5⃣ Line Plan Accuracy Trend
- Line chart showing weekly plan adherence per line (CW-based)
- Tracks fluctuations and highlights stable vs. volatile performance

### 6⃣ Top & Bottom 5 Machines
- Lists machines with highest and lowest plan adherence
- Allows fast identification of consistent vs. problematic stations

### 7⃣ Product Share or Machine Chart
- Bar chart comparing **planned vs actual output share** by product **or** machine
- Changes dynamically based on the toggle selection ("Show Products" / "Show Machines")
- Enables focused analysis of overproduced or underproduced elements

---

## 📄 Summary

This report provides a detailed view of **how well production aligns with planning**, from high-level KPIs to detailed product and machine-level performance. It enables:
- Fast identification of machines or lines deviating from plan
- Tracking of adherence to product mix and line-level execution
- Dynamic exploration of root causes behind underperformance

By combining filters, comparative visuals, and toggles, the report supports **data-driven manufacturing control and planning improvements**.

---

## 📂 Data Source Setup

Set `Data_Source` to your local folder path (end with `\`).

The report uses a parameter called `Data_Source` that points to the local folder containing the required `.csv` files. After downloading the repository, make sure the path points to the cloned Git folder where the source files are located.

- Path should end with a backslash (`\`)
- Must include all `.csv` data files from the report directory

**Example:**
```
C:\Users\YourName\Documents\GitHub\powerbi-portfolio\Files\plan-adherence-report\
```
