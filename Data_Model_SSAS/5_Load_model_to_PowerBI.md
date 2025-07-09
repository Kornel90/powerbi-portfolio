---

# Connecting SSAS Tabular Model to Power BI

This guide outlines how to connect a deployed SSAS Tabular model to Power BI for reporting and visualization. The connection enables real-time analytics on processed data from SQL Server via SSAS.

---

## Importing SSAS Model into Power BI

In Power BI Desktop:

1. Select **Get Data > SQL Server Analysis Services database**.
2. Choose **Connect live** mode for direct queries against the SSAS model.
3. Select the SSAS model from the server (e.g., `ProductionDataModel`).
4. Choose the  `Model`.

![PowerBI_loadmodel](https://github.com/user-attachments/assets/6b74df17-f1af-4d83-83b2-59c454c3cb7f)

---

## Exploring the Imported Model

Once connected, all tables, measures, and relationships defined in the SSAS model become available in Power BI's model view.

- No need to recreate relationships or calculations in Power BI
- Real-time updates reflect changes in the SSAS model as partitions are processed


![PowerBI_loaded](https://github.com/user-attachments/assets/86822acd-2138-4818-89cb-8b09a1fb45e1)

---

