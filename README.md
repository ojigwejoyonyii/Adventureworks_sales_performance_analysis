# 🚴‍♀️ AdventureWorks Sales Performance Analysis
This project is a business intelligence solution that converts Adventure Works sales data into interactive dashboards for exploring sales trends, profitability, customers, products, and regional performances amongst other relevant key performance indicators. 

---

# Project Overview
An end-to-end Business Intelligence solution built in Power BI to track KPIs, compare regional sales performance, analyze product-level trends, and identify high-value customers for AdventureWorks, a bicycle, accessories, and apparel retailer. The project covers the full Business Intelligence lifecycle: connecting and transforming raw CSV data, building a relational (star-schema) data model, writing calculated columns and DAX measures, and designing an interactive Power BI report.

**Dataset coverage:** Order dates from **January 2020 – June 2022**, across **10 sales territories** in North America, Europe, and the Pacific region, **293 products** in 3 categories, and **18,148 customers**.

---

## 🎯 Business Objectives

The client needs a single Power BI report to support data-driven decisions across sales and product teams. The core objectives includes:

- **Track KPIs** — Consolidate Order Quantity, Revenue, Cost, Profit, and Return Quantity into a single live report.
- **Compare regional performance** — Break down revenue and returns by sales territory, region, and continent to spot over- and under-performing markets.
- **Analyze product-level trends** — Compare performance across product categories (Bikes, Accessories, Clothing) and subcategories, and track revenue trend by year and month.
- **Identify high-value customers** — Quantify revenue concentration among top customers to support retention and account-prioritization decisions.
- **Enable self-service analysis** — Deliver an interactive, filterable report rather than static exports.

---

## 🛠️ Tools & Technologies

| Category | Tool |
|---|---|
| Data Modeling & Visualization | Microsoft Power BI Desktop |
| Data Transformation | Power Query (M language) |
| Calculations & Measures | DAX (Data Analysis Expressions) |
| Data Sources | 8 CSV files (AdventureWorks raw data export) |
| Version Control | Git & GitHub |
| Documentation | Markdown |

---

## 🧹 Data Cleaning & Transformation

Eight source CSV files were connected and shaped in **Power Query** before being loaded into the data model:

| Source File | Loads To |
|---|---|
| `AdventureWorks Customer Lookup.csv` | Customer |
| `AdventureWorks Product Lookup.csv` | Product |
| `AdventureWorks Product Categories Lookup.csv` | Product Category |
| `AdventureWorks Product Subcategories Lookup.csv` | Product Subcategories |
| `AdventureWorks Territory Lookup.csv` | Territory |
| `AdventureWorks Returns Data.csv` | Returns Data |
| `AdventureWorks_Calendar_Lookup.csv` | Calendar |
| `Sales_2020-2022.csv` | Sales_2020-2022 |

Key transformation steps applied per table:

- **Promoted headers & set data types** on every table (keys as `Int64`, dates as `Date`, text fields typed explicitly).
- **Customer table** — removed the unused `Prefix` column, merged `FirstName` + `LastName` into a proper-cased `FullName` column, and recoded abbreviated values into readable labels (`HomeOwner`: N/Y → No/Yes; `MaritalStatus`: M/S → Married/Single; `Gender`: M/F → Male/Female). Filtered out rows with a blank `FirstName`.
- **Product table** — rounded `ProductCost` and `ProductPrice` to 2 decimal places after type conversion.
- **Returns Data / Territory / Product Category / Product Subcategories / Calendar** — straightforward header promotion and type correction to prepare each as a clean dimension table.
- **Sales_2020-2022** — typed as the central fact table (order/stock dates, product, customer, and territory keys, order quantity), with `Revenue` and `Cost` deliberately **not** included in the source file — these are derived in the data model (see below) rather than the raw data, keeping the fact table lean and the pricing logic centralized.

---

## 🔍 Data Analysis

A **star-schema relational model** connects the `Sales_2020-2022` fact table (56,046 rows) to six dimension tables via single-direction, many-to-one relationships:

| From Table | From Column | To Table | To Column |
|---|---|---|---|
| Sales_2020-2022 | ProductKey | Product | ProductKey |
| Sales_2020-2022 | CustomerKey | Customer | CustomerKey |
| Sales_2020-2022 | TerritoryKey | Territory | SalesTerritoryKey |
| Sales_2020-2022 | OrderDateKey | Calendar | DateKey |
| Returns Data | ProductKey | Product | ProductKey |
| Returns Data | TerritoryKey | Territory | SalesTerritoryKey |
| Product | ProductSubcategoryKey | Product Subcategories | ProductSubcategoryKey |
| Product Subcategories | ProductCategoryKey | Product Category | ProductCategoryKey |

**Calculated columns:**

```dax
Revenue = 'Sales_2020-2022'[OrderQuantity] * RELATED('Product'[ProductPrice])
Cost    = 'Sales_2020-2022'[OrderQuantity] * RELATED('Product'[ProductCost])
```

**DAX measure:**

```dax
PROFIT = SUM('Sales_2020-2022'[Revenue]) - SUM('Sales_2020-2022'[Cost])
```

Time intelligence (Year, Quarter, Month, Day breakdowns) is handled through Power BI's built-in **Date Hierarchy** on the Calendar and order-date fields, which the column charts and area chart use to trend revenue and profit by year and month.

---

##  📊 Dashboard & Visualizations

The report is a single, densely-packed analysis page combining KPI cards with comparison and trend charts:

- **KPI Cards** — Total Order Quantity, Total Revenue, Total Return Quantity, Total Cost, and Total Profit at a glance.
- **Revenue by Year** (column chart) — annual revenue trend, 2020–2022.
- **Revenue Trend by Month** (area chart) — monthly revenue seasonality across the full date range.
- **Revenue by Product Category** (column chart) — Bikes vs. Accessories vs. Clothing.
- **Revenue by Product Subcategory** (column chart) — subcategory-level breakdown within categories.
- **Revenue by Region** (column chart) — territory-level comparison across North America, Europe, and the Pacific.
- **Returns by Product Category** (column chart) — return volume compared against sales by category.
- **Profit by Year** (column chart) — annual profit trend alongside the revenue trend.

*(Add a screenshot of the published report page here.)*

---

## 💡 Key Insights

Based on the current dataset (Jan 2020 – Jun 2022):

- **Bikes dominate revenue** — the Bikes category generated ~$23.6M of the ~$24.9M total revenue (**~95%**), with Road Bikes ($11.3M) and Mountain Bikes ($8.6M) as the two leading subcategories. Accessories ($0.91M) and Clothing ($0.37M) are minor by comparison.
- **Overall profit margin sits at ~42%** — total profit of ~$10.46M against ~$24.9M revenue and ~$14.46M cost.
- **Revenue grew sharply from 2020 to 2021** (+46%, from $6.4M to $9.3M) but was roughly flat into 2022 ($9.2M) — note 2022 data only runs through June, so it isn't a full year yet.
- **Australia and the Southwest lead regional performance** — Australia ($7.42M) and Southwest ($4.82M) are the top two territories by revenue; North America as a continent ($9.71M) narrowly leads Europe ($7.79M) and the Pacific ($7.42M). A handful of territories (Southeast, Northeast, Central) show negligible revenue, which may indicate limited market presence or a data-mapping gap worth verifying.
- **Customer revenue is meaningfully concentrated** — the top 10% of customers (1,741 of 17,416 purchasing customers) generate **~40% of total revenue**, making them a clear priority segment for retention and account management.
- **Return rate is low but not negligible** — 1,828 units returned against 84,174 units sold, a **~2.2% return rate**, concentrated in the same categories that drive the most sales.

---

## 🎯 Recommendations

- **Double down on Bikes, but de-risk the category concentration.** With ~95% of revenue from one category, evaluate whether Accessories and Clothing are being under-marketed (e.g., bundled add-on offers at the point of bike sale) to diversify revenue.
- **Prioritize the top 10% of customers** identified in the model with a formal retention/loyalty program, given they already drive ~40% of revenue — losing even a few of these accounts has outsized impact.
- **Investigate the near-zero-revenue territories** (Southeast, Northeast, Central) — confirm whether this reflects genuinely limited market activity or a data/territory-mapping issue, since it's an outlier next to Australia and Southwest.
- **Watch the 2021→2022 plateau closely** once the full 2022 year is available — confirm whether growth has genuinely stalled or the flat comparison is purely a partial-year artifact.

---

## 📂  Repository Structure
- Data
- Documentation
- Images
- Power BI
 





