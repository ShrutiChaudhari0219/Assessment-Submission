# Task 2: Superstore Sales Analysis & Dashboard

## Objective
Perform descriptive and predictive analysis on the Superstore Sales dataset and create an interactive dashboard using Python (Plotly Dash).

## Dataset
- **Name:** Superstore.xlsx
- **Columns:**
  - Row_ID, Order ID, Order Date, Ship Date, Ship Mode
  - Customer ID, Customer Name, Segment, Country, City, State, Region
  - Product ID, Category, Sub-Category, Product Name
  - Sales, Quantity, Profit, Returns, Payment Mode
  - ind1, ind2 (removed during cleaning)

## Steps Performed

### 1. Data Loading & Cleaning
- Imported dataset using pandas
- Dropped irrelevant columns (`ind1`, `ind2`)
- Renamed messy columns
- Converted date columns to datetime
- Handled missing values

### 2. Descriptive Analysis
- Summary statistics for Sales, Profit, Quantity
- Top 10 best-selling products
- Profit by Region and Category
- Sales trends over time

### 3. Predictive Analysis
- Linear Regression to predict Sales
- Features: Quantity, Category, Region, Segment, Ship Mode, Payment Mode
- Train-test split (80%-20%)
- Evaluation using RMSE & R² score

### 4. Interactive Dashboard (Dash)
- 6 interactive charts:
  1. Sales by Region
  2. Sales over Time
  3. Profit vs Sales Scatter
  4. Top 10 Best-Selling Products
  5. Category-wise Profit
  6. Sales by Ship Mode
- Region dropdown filters all charts
- Compatible with Dash 3.x

## Dependencies
```bash
pip install pandas plotly dash scikit-learn openpyxl
