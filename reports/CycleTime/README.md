# Cycle Time Analysis – Power BI Report

## 📌 Context
This report was created to **monitor the compliance of the planned cycle time (Cycle Time - CT) with actual results** in the production process. It enables data analysis across different dimensions, such as **production stations, product groups, and time**, helping to identify deviations and potential areas for optimization.

### 🎯 Main Objectives of the Report:
- **Tracking time trends** – analyzing how cycle time changes at various stations.
- **Identifying bottlenecks** – detecting stations where cycle time exceeds the norm.
- **Assessing process stability** – analyzing standard deviations for different operations.
- **Comparing performance across suppliers and products** – evaluating the impact of different variables on process efficiency.
- **Analyzing individual products and stations** – allowing for deep dives into details, such as how a specific product performs at different stations or how a station handles various products.

---
![image](https://github.com/user-attachments/assets/870c944f-37f8-4967-ae00-c36f77884842)
![image](https://github.com/user-attachments/assets/38426378-a669-446b-aedf-894229b22577)
![image](https://github.com/user-attachments/assets/3645354a-5798-45c9-81f4-660474f4cb98)



## ⚙️ Report Approach
The report utilizes **data from the production system** and presents it in an accessible visual format.

### 🏗️ Key Approach Elements:
- **Dynamic filtering** – users can analyze data by product groups, stations, or cycle time range.
- **Various visualization types** – including line charts, histograms, heat maps, and bar charts.
- **Color-coded deviation indicators** – green, orange, and red markers quickly highlight issues.
- **Trend analysis** – cycle time is examined historically to detect long-term changes.
- **Flexible analysis at different detail levels** – users can select a single product or station for in-depth insights.

---

## 📊 Report Structure

### 1️⃣ Line Chart (CT Median Trend)
- Displays the median cycle time for different stations over time.
- Helps understand how process efficiency evolves.
- **When a single product is selected**, users can compare its cycle time trends across stations.
- **When a single station is selected**, users can check if its cycle times are stable or fluctuating over time.

### 2️⃣ Heat Map
- Color-coded to indicate which operations meet cycle time targets and which exceed them.
- **Selecting a single product helps identify stations where its cycle time deviates from the norm**, pinpointing potential production issues.

### 3️⃣ Cycle Time Histogram
- Displays the distribution of cycle times for selected stations or suppliers.
- **If a single station is selected**, the histogram reveals how variable cycle times are for that specific operation.

### 4️⃣ Bar Chart (CT Median & StDev)
- Shows the median and standard deviation of cycle time for each station.
- Helps identify stable vs. highly variable processes.
- **Selecting a single station allows for detailed analysis of its standard deviation and whether it meets the expected cycle time** across different products.

### 5️⃣ Toggle Buttons (Green Arrows "Show by Group" and "Show by Stations")
- Allow switching between **product group view and station view**.
- **Product group view** enables comparison of cycle time trends across different product categories.
- **Station view** evaluates how different operations perform within the production process.

---

## 📌 Summary
This report provides **a comprehensive insight into the production process**, enabling data-driven decisions to **optimize cycle times on production lines**. The ability to analyze both at the product group level and at individual stations makes it a versatile tool for managing production efficiency.

