# ➡️ [Published Power BI Dashboard](https://app.powerbi.com/links/gZK2dAiE6g?ctid=5a740cd7-5768-4d09-ae13-f706b09fa22c&pbi_source=linkShare)

---

# ETL and Data Preprocessing Workflow

## 1. Extraction: Downloading Data from FuelWatch

**Data Source**: Historical fuel price data provided by FuelWatch

**File Format**: Monthly CSV files from 2020 to 2023

**Issue**: There are numerous monthly files, which need to be merged for efficient analysis.

---

## 2. Transformation: Merging Data by Year using Python

To process multiple CSV files, a Python script was used to merge data by year:

```python
import pandas as pd
import glob

# Get file paths for 2023 data
file_paths = glob.glob("C:/path_to_files/*2023*.csv")

# Read and merge the files
all_data = pd.concat([pd.read_csv(file) for file in file_paths])

# Save the merged data
all_data.to_csv("C:/path_to_output/merged_data_2023.csv", index=False)
```
---

## 3. Transformation: Data Cleaning and Preprocessing

Preprocessing tasks were performed to improve data quality in the merged dataset:

- **Remove unnecessary columns**: Removed irrelevant columns (e.g., "Unnamed: 10").
- **Standardize date formats**: Converted the 'PUBLISH_DATE' column to the standard datetime format.
- **Handle missing values**: Removed rows containing Null values to ensure data integrity.
- **Optimize data types**: Assigned appropriate data types (e.g., 'POSTCODE' as int32, 'PRODUCT_PRICE' as float32) to reduce memory usage.

Additional tasks included removing duplicate data and filtering based on dates to enhance data refinement.

---

## 4. Loading: Integrating Data into Power BI

Power BI was used to integrate the data by year:

- **Merging data by year**: The "Combine Queries" feature in Power BI was used to merge data from different years into a single table.
- **Star Schema Design**:
  - **Purpose**: Designed to efficiently scale as new data is added each month or year.
  - **Dimension Tables**: Created dimension tables to structure data:
    - 'Dim_Date': Date-related details (Year, Month, Week, etc.).
    - 'Dim_Product': Product type (Fuel type) information.
    - 'Dim_Region': Geographic details (Postcode, City, etc.).
    - 'Dim_Brand': Brand information.
    - 'Dim_TradingName: Trading name information.
  - **Fact Table**: Structured around key metrics such as 'PRODUCT_PRICE'.

The star schema model is shown below:
![image](https://github.com/user-attachments/assets/a6fce9b7-3985-4c0c-88f0-9b3485259438)
*Figure 1: Star Schema Model for FuelWatch Data Integration*

The Power BI relationship modeling tools were used to link the dimension and fact tables, optimizing the performance of data analysis.

---

## Power BI Demo

Please refer to the short dashboard video below. Click to link to watch the video.
[![Screenshot 2025-01-14 114319](![image](https://github.com/user-attachments/assets/d91f3e83-699c-410f-a962-8328fea8dd21)
)](https://youtu.be/XY9UMDeZFFs)

---

## Summary

Through the ETL and data preprocessing tasks, FuelWatch data was integrated, cleaned, and loaded into Power BI. The final step was designing a star schema to provide a scalable foundation for future data expansion.

