# 🚴‍♀️ AdventureWorks Sales Performance Analysis
This project is a business intelligence solution that converts Adventure Works sales data into interactive dashboards for exploring sales trends, profitability, customers, products, and regional performances amongst other relevant key performance indicators. 

---

# 💼 Project Type

|📌Category| 📖 Details |
|---------------|-----------|
|Project Type| Data Analytics / Business Intelligence|
|Data| Adventure Works|
|Tools| power BI, Power Query|
|Focus| Tracking KPI's which includes profitability, regional performance, Product level trend, High value customers|


##### project status: In Progress

---

# 📑 Table of Contents

1. [📊 Project Overview](#-project-overview)
2. [💼 Business Problem](#-business-problem)
3. [🎯 Business Objective](#-business-objective)
4. [🛠️ Project Scope & Tools](#️-project-scope--tools)
5. [📁 Repository Structure](#-repository-structure)
6. [🔄 Data Workflow](#-data-workflow)
7. [📊 Data visualization](#-data-visualization)
8. [🗂️ Data Model & Schema](#️-data-model--schema)
9. [💡 Key Insights](#-key-insights)
10. [📌 Recommendations](#-recommendations)
11. [⚠️ Assumptions & Limitations](#️-assumptions--limitations)
12. [👤 Author](#-author)

---

# 📊 Project Overview

This project is an end-to-end Business Intelligence solution built in Power BI to track KPIs, compare regional sales performance, analyze product-level trends, and identify high-value customers for AdventureWorks, a bicycle, accessories, and apparel retailer. It covers the full BI lifecycle: connecting and transforming raw CSV data, building a relational (star-schema) data model, writing calculated columns and DAX measures, and designing an interactive Power BI report.

**Dataset coverage:** Order dates from **January 2020 – June 2022**, across **10 sales territories** in North America, Europe, and the Pacific region, **293 products** in 3 categories, and **18,148 customers**.

The analysis shows that revenue grew from 2020 to 2021 before declining in 2022, though this drop should be read in context: the 2022 data covers only six months, while 2020 and 2021 contain full-year data, so the decline reflects the shorter reporting window rather than a confirmed deterioration in sales. At the product level, the Bikes category generated the largest share of revenue across the combined three-year period, making it the business's primary revenue driver. Overall, AdventureWorks achieved a strong profit margin of 41.7% across the full 2020 to 2022 period.

---

# 💼 Business Problem

Adventure Works generates sales data across multiple years, products, customers, and regions, but raw sales data alone does not provide a clear view of overall business performance.

The business needs to understand its revenue and profitability trends, identify its major revenue-generating products, and evaluate customer and regional performance to determine where the business is performing well and where opportunities for improvement may exist.

Without a structured analysis of these areas, it may be difficult for decision-makers to identify important performance trends and make informed, data-driven business decisions.

---

# 🎯 Business Objective

The project aims at creating  Power BI report to support data-driven decisions across sales and product teams. The core objectives includes:

- **Track KPIs** — Consolidate Order Quantity, Revenue, Cost, Profit, and Return Quantity into a single live report.
- **Compare regional performance** — Break down revenue and returns by sales territory, region, and continent to spot over- and under-performing markets.
- **Analyze product-level trends** — Compare performance across product categories (Bikes, Accessories, Clothing) and subcategories, and track revenue trend by year and month.
- **Identify high-value customers** — Quantify revenue concentration among top customers to support retention and account-prioritization decisions.
- **Enable self-service analysis** — Deliver an interactive, filterable report rather than static exports.

---

# 🛠️ Project Scope & Tools

| Area | In Scope | Out of Scope | Granularity |
|---|---|---|---|
| Sales Performance | Revenue, sales trends | Sales forecasting | Year / Month/ Product |
| Profitability | Profit and profit margin analysis | Detailed financial statement analysis | Year  |
| Product Performance | Product category and sub category revenue performance | Product development and production analysis | Product / Category/ Sub category |
| Customer Performance | Customer purchase and sales contribution | Customer satisfaction and demographics | Customer / Region/ Year |
| Regional Performance | Sales and profit across available regions | External market and competitor analysis | Region / Territory |
| Data Preparation | Data cleaning, transformation and validation | Changes to source systems | Row / Column / Table |
| Visualization & KPIs | Interactive dashboards and key performance metrics | Enterprise BI deployment | Dashboard / KPI |



| Category | Tool |
|---|---|
| Data Modeling & Visualization | Microsoft Power BI Desktop |
| Data Transformation | Power Query (M language) |
| Calculations & Measures | DAX (Data Analysis Expressions) |
| Data Sources | 8 CSV files (AdventureWorks raw data export) |
| Version Control | Git & GitHub |
| Documentation | Markdown |

---

# 📁 Repository structure 

``` text
Adventure-Works-Sales-Analysis/
│
├── 📂 Data/
│   ├── 📂 Cleaned Data/
│   └── 📂 Uncleaned Data/
│
├── 📂 Query & Formulas/
│   ├── 📂 EDA Formulas/
│   └── 📂 Transformation & Metric Formulas/
│
├── 📂 Report/
│   ├── 📂 Documentation/
│   └── 📂 Power BI File/
│
├── 📂 Visual/
│   ├── 📂 Slicers Images/
│   └── 📂 Dashboard Images/
│
└── 📄 README.md

---

```
# 🔄 Data Workflow

📥 **Data Source**  
Online CSV Dataset  
🔗 Source link: *[Insert link here]*  
⬇️  
📂 **Data Ingestion**  
Connected the CSV files to Power BI  
⬇️  
🧹 **Data Cleaning**  
• Removed irrelevant columns such as the Prefix column  
• Merged First Name and Last Name columns  
• Merged the 2020, 2021 and 2022 Sales tables  
⬇️  
🔄 **Data Transformation**  
• Created calculated columns such as Revenue and Cost  
• Applied statistical/formula-based calculations in Power BI  
⬇️  
📊 **Data Analysis**  
• Created charts and slicers  
• Applied visual, statistical and query-based analysis  
• Developed KPIs and performance metrics  
⬇️  
📈 **Output**  
Interactive Power BI Dashboard

---

# 🗂️ Data Model & Schema

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

```dax
PROFIT MARGIN = `Sales_2020-2022`[PROFIT] / SUM(Sales_2020-2022`[REVENUE])
```

Time intelligence (Year, Quarter, Month, Day breakdowns) is handled through Power BI's built-in **Date Hierarchy** on the Calendar and order-date fields, which the column charts and area chart use to trend revenue and profit by year and month.

---

#  📊 Data Visualization

The report is a double, densely-packed analysis page combining KPI cards with comparison and trend charts:

- **KPI Cards** — Total Order Quantity, Total Revenue, Total Return Quantity, Total Cost, and Total Profit at a glance.
- **Revenue by Year** (column chart) — annual revenue trend, 2020–2022.
- **Revenue Trend by Month** (area chart) — monthly revenue seasonality across the full date range.
- **Revenue by Product Category** (column chart) — Bikes vs. Accessories vs. Clothing.
- **Revenue by Product Subcategory** (column chart) — subcategory-level breakdown within categories.
- **Revenue by Region** (column chart) — territory-level comparison across North America, Europe, and the Pacific.
- **Returns by Product Category** (column chart) — return volume compared against sales by category.
- **Profit by Year** (column chart) — annual profit trend alongside the revenue trend.

<img width="852" height="479" alt="image" src="https://github.com/user-attachments/assets/48791226-41c3-4aa0-9594-cb577ffd34e7" />


---

# 💡 Key Insights

Based on the current dataset (Jan 2020 – Jun 2022):

- **Bikes dominate revenue** — the Bikes category generated ~$23.6M of the ~$24.9M total revenue (**~95%**), with Road Bikes ($11.3M) and Mountain Bikes ($8.6M) as the two leading subcategories. Accessories ($0.91M) and Clothing ($0.37M) are minor by comparison.
- **Overall profit margin sits at ~42%** — total profit of ~$10.46M against ~$24.9M revenue and ~$14.46M cost.
- **Revenue grew sharply from 2020 to 2021** (+46%, from $6.4M to $9.3M) but was roughly flat into 2022 ($9.2M) — note 2022 data only runs through June, so it isn't a full year yet.
- **Australia and the Southwest lead regional performance** — Australia ($7.42M) and Southwest ($4.82M) are the top two territories by revenue; North America as a continent ($9.71M) narrowly leads Europe ($7.79M) and the Pacific ($7.42M). A handful of territories (Southeast, Northeast, Central) show negligible revenue, which may indicate limited market presence or a data-mapping gap worth verifying.
- **Customer revenue is meaningfully concentrated** — the top 10% of customers (1,741 of 17,416 purchasing customers) generate **~40% of total revenue**, making them a clear priority segment for retention and account management.
- **Return rate is low but not negligible** — 1,828 units returned against 84,174 units sold, a **~2.2% return rate**, concentrated in the same categories that drive the most sales.

---

# 📌 Recommendations

- **Double down on Bikes, but de-risk the category concentration.** With ~95% of revenue from one category, evaluate whether Accessories and Clothing are being under-marketed (e.g., bundled add-on offers at the point of bike sale) to diversify revenue.
- **Prioritize the top 10% of customers** identified in the model with a formal retention/loyalty program, given they already drive ~40% of revenue — losing even a few of these accounts has outsized impact.
- **Investigate the near-zero-revenue territories** (Southeast, Northeast, Central) — confirm whether this reflects genuinely limited market activity or a data/territory-mapping issue, since it's an outlier next to Australia and Southwest.
- **Watch the 2021→2022 plateau closely** once the full 2022 year is available — confirm whether growth has genuinely stalled or the flat comparison is purely a partial-year artifact.



 





