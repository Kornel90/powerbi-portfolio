## 📌 Context

This Power BI report was developed to **monitor the adherence of actual cycle times (CT) to planned targets** in the production of brake disc components. It enables multi-dimensional analysis across **stations, product groups, customers, and time periods**, helping identify deviations, variability, and optimization opportunities.

### 🎯 Main Objectives of the Report:
- **Tracking time trends** – analyzing how cycle time changes at various stations.
- **Identifying bottlenecks** – detecting stations where cycle time exceeds the norm.
- **Assessing process stability** – analyzing standard deviations for different operations.
- **Comparing performance across suppliers and products** – evaluating the impact of different variables on process efficiency.
- **Analyzing individual products and stations** – allowing for deep dives into details, such as how a specific product performs at different stations or how a station handles various products.

---

## ⚙️ Report Approach

The report utilizes **data extracted from the production system** and presents it using intuitive, interactive visualizations for detailed cycle time analysis.

### 🏠 Key Approach Elements:
- **Interactive filter pane** – shift, date, product group/item, station
- Cycle Time threshold filter – a key filter that determines which records are included in visuals; strongly influences histogram and variability analysis
- **Responsive visuals** – all charts update based on selected filters
- **Color-coded heat map** – highlights CT compliance per station and customer
- **Line charts** – visualize cycle time trends by week and station
- **Histograms & bar charts** – show CT distribution, median, and variability

This flexible setup allows users to explore both high-level trends and granular performance data by product, customer, and process station.

---
![image](https://github.com/user-attachments/assets/d7c5b138-67aa-465c-ba22-16d382c626ca)
![image](https://github.com/user-attachments/assets/6957e8f1-4933-421d-bd38-ecce445a7495)
![image](https://github.com/user-attachments/assets/849b76bb-64e3-47bb-9bfb-200ed674dc28)

---

## 📊 Report Structure

### 1⃣ KPI Tiles
- **Median Cycle Time** – overall median CT for selected data range.
- **CT values above target** – percentage of operations exceeding defined cycle time threshold.
- **Most Variable Station** – station with the highest CT fluctuation.
- **Bottleneck Station per Customer** – table listing the station with the highest CT by customer group.
- **OK pcs on visual / Range Coverage** – total observations and coverage relative to CT threshold.

### 2⃣ Heat Map Table
- Displays **median CT per station and customer group**.
- Uses **color coding** (green/red) to show values below or above the CT threshold.
- Enables quick identification of problematic operations for each customer.

### 3⃣ Timeline of Stations (CT Median)
- **Line chart** showing how the CT median evolves over calendar weeks.
- Multiple lines represent different stations.
- Helps track trends, shifts, and stabilization or degradation of performance over time.

### 4⃣ Cycle Time Histogram
- Visualizes the **distribution of CT values** across stations.
- Bars grouped by station and colored for quick differentiation.
- Reveals process variability and frequency of CT ranges.

### 5⃣ Median & StDev Bar Chart
- Combined bar chart showing **CT median** and **standard deviation** per station.
- Highlights stations with consistent vs. highly variable performance.
- Supports station-level analysis of process stability.

---

## 📌 Summary
This report provides **a comprehensive insight into the production process**, enabling data-driven decisions to **optimize cycle times on production lines**. The ability to analyze both at the product group level and at individual stations makes it a versatile tool for managing production efficiency.
