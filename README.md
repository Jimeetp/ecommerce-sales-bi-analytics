# E-Commerce Sales & Profitability Analytics Dashboard

An end-to-end business intelligence project modeling transactional retail data to analyze revenue trends, regional profit drivers, pricing elasticity, and customer concentration using **SQL Server**, **Power BI**, and **DAX**.

---

## 📌 Business Requirements & Questions Answered

1. **Trend Analysis:** How are sales and profit trending Month-over-Month (MoM) and Year-over-Year (YoY)?
2. **Geographic & Category Contribution:** Which regions and product categories drive the highest net margin?
3. **Discount Elasticity:** Does discounting increase transaction volume or erode profit margins?
4. **Customer Concentration:** Who are the top customers, and how concentrated is total revenue?
5. **Channel Preferences:** Which payment modes do customers prefer for order volume and order value?

---

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

```dax
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
```
