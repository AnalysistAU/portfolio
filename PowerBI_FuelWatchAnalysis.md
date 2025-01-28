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

Data cleaning and preprocessing tasks were performed to enhance the quality of the merged dataset. The key steps include:  

###  1) Remove Unnecessary Columns 
Unrelated or redundant columns were removed to optimize the dataset size and improve processing efficiency.  

- Dropped non-essential columns such as **"Unnamed: 10"** and other metadata fields.  
- Retained only one representative column for redundant information.  

**Python Code**  
```python
df.drop(columns=['Unnamed: 10', 'Metadata_ID', 'Redundant_Column'], inplace=True)
```
---

###  2) Standardize Date Formats 
Date values were standardized to maintain consistency and ensure accurate analysis and visualization.  

- Converted **'PUBLISH_DATE'** to a uniform datetime format (`YYYY-MM-DD`).  
- Handled multiple formats (e.g., `MM/DD/YYYY`, `YYYY.MM.DD`) using `pd.to_datetime()`.  

**Python Code**  
```python
df['PUBLISH_DATE'] = pd.to_datetime(df['PUBLISH_DATE'], errors='coerce')
```  
---

###  3) Handle Missing Values  
Missing values were addressed to maintain **data integrity** and prevent potential issues during analysis.  

- Essential columns (e.g., `Customer_ID`, `Product_Name`) with missing values were **dropped**.  
- Numerical fields (e.g., `PRODUCT_PRICE`) had missing values filled with the **median or mean** to preserve statistical consistency.  

**Python Code**  
```python
# Replace missing categorical values with 'Unknown'
df['Product_Category'].fillna('Unknown', inplace=True)

# Fill missing numerical values with the median
df['PRODUCT_PRICE'] = df['PRODUCT_PRICE'].fillna(df['PRODUCT_PRICE'].median())
```
---

###  4) Optimize Data Types
Proper data types were assigned to enhance memory efficiency and improve processing speed.  

- Converted `POSTCODE` to `int32` and `PRODUCT_PRICE` to `float32` to reduce memory usage.  
- Used `astype()` for type conversion.  

**Python Code**  
```python
df['POSTCODE'] = df['POSTCODE'].astype('int32')
df['PRODUCT_PRICE'] = df['PRODUCT_PRICE'].astype('float32')
```
---

###  5) Remove Duplicate Records
Duplicate entries were identified and removed to ensure data accuracy.  

- Checked for duplicates across key columns and **dropped them while keeping the first occurrence**.  

**Python Code**  
```python
df.drop_duplicates(subset=['Customer_ID', 'Order_ID'], keep='first', inplace=True)
```
---

By implementing these data cleaning and preprocessing steps, the dataset was refined for more efficient and accurate analysis.

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

Please check out the short dashboard video below. Click to view.
[![Screenshot 2025-01-16 202149](https://github.com/user-attachments/assets/28d5bdb8-425b-4dc6-b894-39ca4ebf55df)](https://youtu.be/XY9UMDeZFFs)

---

## Summary

Through the ETL and data preprocessing tasks, FuelWatch data was integrated, cleaned, and loaded into Power BI. The final step was designing a star schema to provide a scalable foundation for future data expansion.

---

## Report

This is a simple report created to support understanding of the dashboard. Please refer to it.
[Click here](https://docs.google.com/document/d/1_viERX0QsKA43bGf0JxpMtPUY31gd2Jr/edit?usp=sharing&ouid=110905777482363304127&rtpof=true&sd=true)
