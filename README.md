# E-Commerce Sales & Profitability Analytics Dashboard

An end-to-end data analytics project taking raw retail transactional data through SQL Server, data modeling, custom DAX logic, and an interactive 2-page Power BI dashboard.

> **Dataset & Tools Note:**
> * **Data Source:** Raw e-commerce dataset sourced from **Kaggle**.
> * **Workflow Acceleration:** Built and optimized with the help of **AI (Gemini and Claude)** for query refinement, DAX pattern drafting, and dashboard architecture.

---

## 📌 Project Overview

The objective of this project was to move beyond simple surface-level charts and build a production-style reporting setup that answers core commercial business questions:

1. **Trend Performance:** How are sales and net profit tracking Month-over-Month (MoM) and Year-over-Year (YoY)?
2. **Regional & Category Drivers:** Which regions and product lines generate solid profit margins versus low returns?
3. **Discount Impact:** Does offering higher discounts genuinely drive transaction volume, or does it simply erode margins?
4. **Customer Concentration:** Who are the top 10 customers, and how much revenue relies on them?
5. **Payment Method Breakdown:** What payment modes (UPI, Credit Card, COD, Net Banking) do customers prefer for order volume and order value?

---

## 🏗️ Technical Workflow

[ Kaggle Raw CSV ] 
        │
        ▼
[ Microsoft SQL Server (SSMS) ] ── (Staging Table ➔ Data Cleaning ➔ Sanity Checks ➔ SQL Queries)
        │
        ▼
[ Power BI Desktop ] ──────────── (Star Schema Model ➔ Dynamic Dim_Date ➔ Custom DAX Measures)
        │
        ▼
[ Business Report ] ───────────── (2-Page Synchronized Executive Dashboard)
## 📊 Dashboard Pages & Visuals

### Page 1: Executive Overview
* **KPI Ribbon:** `Total Sales`, `Total Profit`, `Profit Margin %`, and `Total Orders`.
* **Sales & Margin Trends:** Dual-axis Line & Clustered Column Chart tracking seasonal peaks and monthly margin fluctuations.
* **Regional Contribution:** Clustered Bar Chart ranking net profitability by region (North, East, West, South).
* **Category Breakdown:** Drillable Matrix table showing margins down to sub-category level.
* **Payment Mode Share:** Donut chart analyzing order share across Credit Card, Net Banking, COD, Debit Card, and UPI.

### Page 2: Customer & Discount Strategy (Deep Dive)
* **Discount Elasticity:** Scatter plot evaluating Average Discount vs. Profit Margin %, weighted by order volume.
* **Top Customer Revenue:** Ranked bar chart and summary matrix tracking top 10 revenue contributors and concentration risk.
* **Global Sync Slicers:** Synchronized controls for `Year` and `Region` across all views.

---

## ⚙️ Data Model (Star Schema)

The report implements a Star Schema model rather than relying on a flat tabular file:

* **Fact Table:** `Fact_Orders` (contains order items, prices, discounts, sales, and profits).
* **Dimension Table:** `Dim_Date` (dynamically generated via DAX with year, quarter, month, and chronological sorting).
* **Relationship:** `Dim_Date[Date]` 1-to-Many (`1:*`) Single Direction $\rightarrow$ `Fact_Orders[Order Date]`.

---

## 📐 Key DAX Measures


// Base Aggregations
Total Sales = SUM('ecommerce data'[Sales])
Total Profit = SUM('ecommerce data'[Profit])
Total Orders = DISTINCTCOUNT('ecommerce data'[Order ID])
Profit Margin % = DIVIDE([Total Profit], [Total Sales], 0)

// Time Intelligence: Prior Year & YoY Growth
Sales LY = 
CALCULATE([Total Sales], SAMEPERIODLASTYEAR(Dim_Date[Date]))

YoY Sales Growth % = 
DIVIDE([Total Sales] - [Sales LY], [Sales LY], 0)

// Time Intelligence: Prior Month & MoM Growth
Sales PM = 
CALCULATE([Total Sales], DATEADD(Dim_Date[Date], -1, MONTH))

MoM Sales Growth % = 
DIVIDE([Total Sales] - [Sales PM], [Sales PM], 0)

