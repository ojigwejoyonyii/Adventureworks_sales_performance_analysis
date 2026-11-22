# Adventure Works Sales Analysis

*Power BI Data Analysis Project Documentation*

| | |
|---|---|
| **Analysis tool** | Microsoft Power BI Desktop (Power Query, Data Model, DAX) |
| **Period covered** | 1 January 2020 – 30 June 2022 |
| **Source data** | Adventure Works CSV files (sales, returns, customer, product, territory, calendar) |

## Table of Contents

- [1. Project Overview](#1-project-overview)
- [2. Business Problem](#2-business-problem)
- [3. Project Objectives](#3-project-objectives)
- [4. Project Scope](#4-project-scope)
- [5. Dataset Overview](#5-dataset-overview)
- [6. Data Source](#6-data-source)
- [7. Data Preparation, Cleaning and Validation](#7-data-preparation-cleaning-and-validation)
  - [7.1 Cleaning steps performed in Power Query](#71-cleaning-steps-performed-in-power-query)
  - [7.2 Calculated columns created in the model](#72-calculated-columns-created-in-the-model)
  - [7.3 Validation checks](#73-validation-checks)
- [8. Analytical Methodology](#8-analytical-methodology)
- [9. Key Performance Indicators](#9-key-performance-indicators)
- [10. Analysis Performed](#10-analysis-performed)
  - [10.1 Yearly Revenue and Profit Analysis](#101-yearly-revenue-and-profit-analysis)
  - [10.2 Monthly Revenue Analysis](#102-monthly-revenue-analysis)
  - [10.3 Product Category and Subcategory Analysis](#103-product-category-and-subcategory-analysis)
  - [10.4 Regional Analysis](#104-regional-analysis)
  - [10.5 Customer Analysis](#105-customer-analysis)
  - [10.6 Product Returns Analysis](#106-product-returns-analysis)
  - [10.7 Year-on-Year Growth and Profit Margin](#107-year-on-year-growth-and-profit-margin)
  - [10.8 Order Type Analysis](#108-order-type-analysis)
- [11. Dashboard Development](#11-dashboard-development)
  - [11.1 Page 1 – Sales Overview](#111-page-1--sales-overview)
  - [11.2 Page 2 – Profitability, Growth and Returns](#112-page-2--profitability-growth-and-returns)
- [12. Key Findings and Insights](#12-key-findings-and-insights)
- [13. Recommendations](#13-recommendations)
- [14. Project Limitations](#14-project-limitations)
- [15. Tools](#15-tools)
- [16. Project Workflow](#16-project-workflow)
- [17. Workbook (Report) Structure](#17-workbook-report-structure)
- [18. Conclusion](#18-conclusion)
- [Appendix A: Data Dictionary](#appendix-a-data-dictionary)

---

# 1. Project Overview

The Adventure Works Sales Analysis project examines the sales activity of a bicycle manufacturer and retailer across products, regions, customers and time using Microsoft Power BI. Eight related CSV files were loaded through Power Query, cleaned, and modelled into a star schema. Calculated columns and DAX measures were then used to produce revenue, cost, profit, profit margin, return and year-on-year growth figures.

The project follows a practical analytics workflow: data sourcing, ingestion, cleaning, modelling, measure creation, visualization, interpretation and recommendation. The final output is a two-page interactive Power BI report supported by a documented data model.

# 2. Business Problem

Adventure Works records tens of thousands of order lines, but raw transactional files do not show management how the business is performing. Questions about revenue, profitability, product mix, geographical performance, customer value and product returns cannot be answered from the files directly. This project organizes the data so that the following business questions can be answered:

- How much revenue, cost and profit were generated, and what is the overall profit margin?

- How has revenue changed from year to year, and how fast is it growing?

- Which product categories and subcategories drive revenue?

- Which regions and countries contribute most to sales?

- Which months show stronger or weaker sales activity?

- Which customers generate the highest revenue?

- How many products are returned, and which categories are returned most?

- How are orders split between single-quantity and multiple-quantity purchases?

# 3. Project Objectives

- Build a clean, validated and relational data model from the raw Adventure Works files.

- Create DAX measures for revenue, profit, profit margin, previous-year revenue and year-on-year growth.

- Compare performance across year, month, region, product category/subcategory and customer.

- Develop an interactive two-page Power BI dashboard with slicers that give a management-level view of performance.

- Translate the results into practical insights and recommendations.

# 4. Project Scope

The project covers orders placed between **1 January 2020 and 30 June 2022**, i.e. two full calendar years (2020 and 2021) and the first six months of 2022. It covers:

- Overall sales, cost, profit and margin

- Revenue by year, month, region, product category and subcategory

- Profit by year and year-on-year revenue growth

- Top-performing customers

- Returned products by category and subcategory

- Order type (single versus multiple quantity)

The project is descriptive and diagnostic. It explains what happened in the available data. It does not build a forecast or a causal model.

# 5. Dataset Overview

The model is built from eight CSV files. Six hold descriptive (dimension) data and two hold events (sales and returns).

| **Dataset Attribute**               | **Value**                           |
|-------------------------------------|-------------------------------------|
| Sales order lines (Sales_2020-2022) | 56,046 rows                         |
| Distinct orders                     | 25,164                              |
| Distinct customers who ordered      | 17,416                              |
| Total units sold                    | 84,174                              |
| Total revenue                       | $24,914,567.18                     |
| Total cost                          | $14,456,986.32                     |
| Total profit                        | $10,457,580.86                     |
| Profit margin                       | 41.97%                              |
| Units returned                      | 1,828 (1,809 return records)        |
| Customers in lookup                 | 18,148 rows                         |
| Products in lookup                  | 293 (130 sold during the period)    |
| Territories                         | 10 regions in 6 countries           |
| Calendar table                      | 912 days (1 Jan 2020 – 30 Jun 2022) |
| Primary analysis tool               | Microsoft Power BI Desktop          |
| Source format                       | CSV                                 |

# 6. Data Source

The data was supplied as a set of Adventure Works CSV files stored in a local folder (“Adventure works raw data”). Power BI’s Power Query reads each file with the Csv.Document connector, using a comma delimiter and Windows-1252 encoding.

| **File**                                        | **Loaded As (Table)** | **Content**                     |
|-------------------------------------------------|-----------------------|---------------------------------|
| Sales_2020-2022.csv                             | Sales_2020-2022       | Order line transactions (fact)  |
| AdventureWorks Returns Data.csv                 | Returns Data          | Product returns (fact)          |
| AdventureWorks Customer Lookup.csv              | Customer              | Customer profile (dimension)    |
| AdventureWorks Product Lookup.csv               | Product               | Product details, cost and price |
| AdventureWorks Product Subcategories Lookup.csv | Product Subcategories | Subcategory names               |
| AdventureWorks Product Categories Lookup.csv    | Product Category      | Category names                  |
| AdventureWorks Territory Lookup.csv             | Territory             | Region, country, continent      |
| AdventureWorks_Calendar_Lookup.csv              | Calendar              | Date keys and dates             |

# 7. Data Preparation, Cleaning and Validation

Data preparation was carried out in Power Query before loading. The cleaning stage matters because every later result depends on the quality and structure of the underlying data.

## 7.1 Cleaning steps performed in Power Query

- **Promoted headers.** The first row of every CSV was promoted to column headers.

- **Set data types.** Keys and quantities were set to whole number, dates to date, names and codes to text, and income and number of children to whole number. Product cost and price were set to decimal.

- **Customer table.** The Prefix column was removed. A FullName column was created by combining FirstName and LastName and capitalizing each word (Text.Proper). Codes were replaced with readable labels: HomeOwner Y/N to Yes/No, MaritalStatus M/S to Married/Single, Gender M/F to Male/Female. Rows with a blank FirstName were filtered out, and CustomerKey was set to whole number.

- **Product table.** ProductCost and ProductPrice were rounded to 2 decimal places.

- **Sales table.** A conditional column was added and renamed **PurchaseType**: OrderQuantity = 1 gives “Single Order”, greater than 1 gives “Multiple Orders”, anything else gives “Others”.

- **Calendar table.** Loaded with DateKey (whole number) and Date (date). Extra columns were then added in the model with DAX (see Section 7.2).

## 7.2 Calculated columns created in the model

| **Table**       | **Column**   | **DAX Expression**                                                       |
|-----------------|--------------|--------------------------------------------------------------------------|
| Sales_2020-2022 | Revenue      | 'Sales_2020-2022'[OrderQuantity] \* RELATED('Product'[ProductPrice]) |
| Sales_2020-2022 | Cost         | 'Sales_2020-2022'[OrderQuantity] \* RELATED('Product'[ProductCost])  |
| Calendar        | Date Number  | WEEKDAY('Calendar'[Date], 3)                                           |
| Calendar        | weekday Name | FORMAT('Calendar'[Date Number], "ddd")                                 |
| Calendar        | week         | IF('Calendar'[Date Number] <= 1, "weekend", "weekday")                |
| Calendar        | Year         | YEAR('Calendar'[Date])                                                 |
| Calendar        | Month        | FORMAT('Calendar'[Date], "MMMM")                                       |

## 7.3 Validation checks

- **Missing values.** No null values were found in any column of the sales table.

- **Duplicates.** No fully duplicated rows were found in the sales table (56,046 rows, all unique).

- **Referential integrity.** Every ProductKey in the sales table exists in the Product table, so every sale can be priced and costed.

- **Date coverage.** Sales orders run from 1 January 2020 to 30 June 2022. The Calendar table covers the same 912 days.

- **Key relationships.** All eight relationships are active, many-to-one and single-direction (see the companion document).

- **Data types.** Order and return dates are true date fields, quantities are whole numbers, and revenue and cost are decimals.

# 8. Analytical Methodology

The analysis uses data modelling, DAX aggregation, grouping, time-intelligence and visualization. Revenue and cost are calculated per order line by multiplying the order quantity by the product’s price or cost, and are then summed by each analysis dimension.

| **Analysis Dimension**           | **Purpose**                                                            |
|----------------------------------|------------------------------------------------------------------------|
| Year                             | Examine annual changes in revenue, profit and margin.                  |
| Month                            | Identify monthly patterns and periods of stronger or weaker sales.     |
| Region                           | Compare regional contribution to revenue.                              |
| Product category and subcategory | Determine which products generate revenue and which are returned most. |
| Customer                         | Identify top-performing customers by revenue.                          |
| Order (purchase) type            | Compare single-quantity and multiple-quantity order lines.             |
| Returns                          | Measure returned units by product category and subcategory.            |

# 9. Key Performance Indicators

The report uses the following core measures. Full DAX definitions are in the companion document.

| **KPI**                 | **Definition**                         | **Purpose**                                       | **Formula Used**                                               |
|-------------------------|----------------------------------------|---------------------------------------------------|----------------------------------------------------------------|
| Total Revenue           | Sum of order quantity × product price. | Overall revenue generated.                        | SUM('Sales_2020-2022'[Revenue])                              |
| Total Cost              | Sum of order quantity × product cost.  | Overall cost of goods sold.                       | Card showing Sum of Cost                                       |
| Profit                  | Revenue minus cost.                    | Overall profitability.                            | SUM(Revenue) − SUM(Cost)                                       |
| Profit Margin           | Profit as a share of revenue.          | Profitability efficiency.                         | [Profit] / SUM(Revenue)                                      |
| Total Orders (Quantity) | Sum of units ordered.                  | Sales volume.                                     | Card showing Sum of OrderQuantity                              |
| Quantity Returned       | Sum of units returned.                 | Product quality and customer satisfaction signal. | Card showing Sum of ReturnQuantity                             |
| Previous Year Revenue   | Revenue for the prior year.            | Baseline for growth.                              | CALCULATE([Total Revenue], PREVIOUSYEAR('Calendar'[Date])) |
| YoY Revenue Growth Rate | Percentage change against prior year.  | Annual growth trend.                              | DIVIDE(CurrentYearRev − PrevYearRev, PrevYearRev)              |
| Number of Order Lines   | Count of order-date entries.           | Transaction activity.                             | Count of OrderDateKey                                          |
| Number of Products      | Count of distinct product keys.        | Catalogue size.                                   | Count of ProductKey                                            |

Headline results for the full period:

| **KPI**        | **Value** | **Note**               |
|----------------|-----------|------------------------|
| Total revenue  | $24.91M  | Jan 2020 – Jun 2022    |
| Total cost     | $14.46M  |                        |
| Profit         | $10.46M  |                        |
| Profit margin  | 41.97%    | Cost 58.03% of revenue |
| Units sold     | 84,174    |                        |
| Units returned | 1,828     | 2.17% of units sold    |
| Order lines    | 56,046    | 25,164 distinct orders |

![Dashboard KPI cards](images/media/image1.png)

![Dashboard KPI cards (continued)](images/media/image2.png)

# 10. Analysis Performed

## 10.1 Yearly Revenue and Profit Analysis

Revenue and profit were compared across 2020, 2021 and 2022 using an area chart (Revenue by Year) and a line chart (Profit per Year).

| **Year**       | **Revenue** | **Cost**    | **Profit**  | **Margin** | **Units** |
|----------------|-------------|-------------|-------------|------------|-----------|
| 2020           | $6,404,934 | $3,803,328 | $2,601,606 | 40.6%      | 2,630     |
| 2021           | $9,324,195 | $5,357,172 | $3,967,023 | 42.5%      | 36,230    |
| 2022 (Jan–Jun) | $9,185,438 | $5,296,486 | $3,888,952 | 42.3%      | 45,314    |

2022 contains only six months of data, so any comparison of 2022 with a full year must be read with care (see Limitations).

![Revenue by Year chart](images/media/image3.png)

![Profit per Year chart](images/media/image4.png)

## 10.2 Monthly Revenue Analysis

Monthly revenue is shown on an area chart. Because 2022 stops at June, January–June have three years of data and July–December have two. Within-year patterns are therefore more reliable than month totals pooled across years.

![Revenue by Month chart](images/media/image5.png)

## 10.3 Product Category and Subcategory Analysis

Revenue is compared by category and subcategory in a column chart that can be drilled from category down to subcategory.

| **Category** | **Revenue**  | **Profit**  | **Share of Revenue** |
|--------------|--------------|-------------|----------------------|
| Bikes        | $23,642,500 | $9,726,169 | 94.9%                |
| Accessories  | $906,657    | $569,634   | 3.6%                 |
| Clothing     | $365,411    | $161,778   | 1.5%                 |

Within Bikes, Road Bikes earned $11.29M (45.3% of total revenue), Mountain Bikes $8.58M (34.4%) and Touring Bikes $3.77M (15.1%). The five best-selling individual products are all Mountain-200 variants, each earning between $1.17M and $1.24M.

![Revenue by Product chart](images/media/image6.png)

## 10.4 Regional Analysis

Revenue is compared across the ten sales territories using a column chart and a Region slicer.

| **Region**     | **Revenue** | **Profit Margin** | **Share of Revenue** |
|----------------|-------------|-------------------|----------------------|
| Australia      | $7,416,452 | 41.5%             | 29.8%                |
| Southwest (US) | $4,822,792 | 42.3%             | 19.4%                |
| Northwest (US) | $3,095,072 | 42.5%             | 12.4%                |
| United Kingdom | $2,902,559 | 41.9%             | 11.7%                |
| Germany        | $2,524,677 | 41.8%             | 10.1%                |
| France         | $2,362,641 | 41.9%             | 9.5%                 |
| Canada         | $1,769,244 | 42.8%             | 7.1%                 |
| Southeast (US) | $11,586    | 44.3%             | 0.05%                |
| Northeast (US) | $6,402     | 44.9%             | 0.03%                |
| Central (US)   | $3,143     | 45.7%             | 0.01%                |

![Revenue by Region chart](images/media/image7.png)

## 10.5 Customer Analysis

A bar chart of top-performing customers ranks customers by revenue and year. Revenue is widely spread: the highest single customer, Jordan Turner, generated $15,571, well under 0.1% of total revenue, followed by Maurice Shan ($12,408), Janet Munoz ($12,015), Lisa Cai ($11,330) and Lacey Zheng ($11,086).

![Top Performing Customers chart](images/media/image8.png)

## 10.6 Product Returns Analysis

Returned units are shown by category and subcategory. A total of 1,828 units were returned.

| **Category** | **Units Sold** | **Units Returned** | **Return Rate** | **Largest Subcategory Returned** |
|--------------|----------------|--------------------|-----------------|----------------------------------|
| Accessories  | 57,809         | 1,130              | 1.95%           | Tires and Tubes (534)            |
| Bikes        | 13,929         | 429                | 3.08%           | Road Bikes (223)                 |
| Clothing     | 12,436         | 269                | 2.16%           | —                                |

Accessories account for 61.8% of returned units mainly because they are sold in large volumes, but Bikes have the highest return rate. Returned units rose from 86 in 2020 to 770 in 2021 and 972 in the first half of 2022.

<!-- TODO: add screenshot of the Products Returned by Category chart -->

## 10.7 Year-on-Year Growth and Profit Margin

Two tables on Page 2 show profit margin by year and the sequence Total Revenue, Previous Year Revenue and YoY Revenue Growth Rate. 2021 revenue grew 45.6% over 2020. 2022 shows −1.5% against 2021 because six months of 2022 are compared with a full 2021.

![Profit Margin and Year-on-Year Growth tables](images/media/image9.png)

## 10.8 Order Type Analysis

A matrix on Page 2 splits order lines by PurchaseType. 31,421 lines (56.1%) are single-quantity and 24,625 (43.9%) are multiple-quantity.

# 11. Dashboard Development

A two-page interactive Power BI report (1920 × 1080) consolidates the findings. Page 1 is the sales and profit overview and Page 2 covers profitability, growth, customers, returns and order type.

## 11.1 Page 1 – Sales Overview

- **KPI cards:** Total Revenue, Quantity Ordered, Quantity Returned, Total Cost and Profit.

- **Charts:** Revenue by Year (area), Revenue by Month (area), Profit per Year (line), Revenue by Region (column), Revenue by Product (column, category to subcategory).

- **Slicers:** Year, Product Subcategory and Region.

![Dashboard Page 1 - Sales Overview](images/media/image10.png)

## 11.2 Page 2 – Profitability, Growth and Returns

- **KPI cards:** Profit Margin, order count and product count.

- **Tables and matrix:** Profit Margin by Year, Year on Year Revenue Growth, and Order Type.

- **Charts:** Top Performing Customers (bar) and Products Returned by Category (column).

- **Slicers:** Year and Region.

- A navigation button links the two pages.

![Dashboard Page 2 - Profitability, Growth and Returns](images/media/image11.png)

The slicers filter every visual on their page through the model relationships, so users can answer questions that are not obvious from the static charts, for example the revenue of one subcategory in one region in one year.

# 12. Key Findings and Insights

- **Revenue and profit are strong.** The business earned $24.91M in revenue and $10.46M in profit at a 41.97% margin. The margin was steady, at 40.6% in 2020, 42.5% in 2021 and 42.3% in 2022.

- **Growth.** Revenue grew 45.6% from $6.40M in 2020 to $9.32M in 2021. 2022 had already reached $9.19M by June, 98.5% of the full 2021 total in half the time.

- **Bikes dominate revenue.** Bikes earn 94.9% of revenue. Road Bikes and Mountain Bikes together earn 79.7%. Mountain-200 models hold all of the top five product positions.

- **Accessories drive volume, not revenue.** Accessories make up 68.7% of units but 3.6% of revenue. Accessories and Clothing were not sold at all in 2020 (only Bikes); both appear from 2021.

- **Revenue is concentrated geographically.** Australia is the largest single region ($7.42M, 29.8%). The five US regions together earn $7.94M, but almost all of it comes from the Southwest and Northwest. Central, Northeast and Southeast together earn under $22,000.

- **Profit margins are similar everywhere.** Every region’s margin falls between 41.5% and 45.7%, so regional differences in profit come from volume, not margin.

- **Seasonality.** Within 2022, revenue rises every month from $1.27M in January to $1.83M in June. In 2021, revenue was lowest in H1 (about $2.95M) and highest in H2, peaking in December ($1.64M). In 2020, June was the strongest month.

- **Returns.** The return rate is 2.17% of units. Bikes have the highest rate (3.08%), and Tires and Tubes is the most-returned subcategory (534 units). Returns are increasing in line with sales volume.

- **Customers.** Revenue is spread across 17,416 customers with no dependency on any one customer.

# 13. Recommendations

- **Protect the bike business.** Because 94.9% of revenue comes from bikes, management should prioritize stock availability, pricing and promotion of Road and Mountain bikes, and examine why Touring Bikes lag.

- **Grow accessories and clothing.** Accessories sell in high volume with a healthy 62.8% margin. Bundling accessories with bike purchases could raise revenue per order.

- **Investigate the weak US regions.** Central, Northeast and Southeast generate almost no revenue. Management should confirm whether this is a data-coverage issue or a real lack of market presence before deciding on expansion or investment.

- **Look into bike returns.** Bikes have the highest return rate (3.08%). Reviewing the reasons (quality, sizing, delivery) for Road Bikes could reduce return costs. Tires and Tubes should be reviewed for the same reason.

- **Plan around seasonality.** Align inventory and promotion calendars with the strongest months once a full year of 2022 data is available.

# 14. Project Limitations

- **2022 is incomplete.** The data ends on 30 June 2022. The YoY measure shows −1.5% for 2022 because it compares six months with a full year, and month totals pooled across years give January–June three years of data and July–December only two.

- **Revenue and cost use list prices.** Revenue is calculated as quantity × current product price, and cost as quantity × current product cost. Actual transaction prices, discounts, price changes and freight are not captured.

- **Returns are not netted off.** Returned units are reported separately and are not deducted from revenue or profit.

- **Sparse US regions.** Central, Northeast and Southeast have very few records, so conclusions about them are weak.

- **Descriptive only.** The analysis describes patterns but does not establish causes.

# 15. Tools

| **Tool / Technology**           | **Use in Project**                                                      |
|---------------------------------|-------------------------------------------------------------------------|
| Microsoft Power BI Desktop      | Data modelling, measures, visualization and dashboard development.      |
| Power Query (M)                 | Data import, cleaning and transformation of the CSV files.              |
| Data Model (star schema)        | Relationships between fact and dimension tables.                        |
| DAX                             | Calculated columns and measures for revenue, profit, margin and growth. |
| Slicers and interactive visuals | Filtering and exploration of the dashboard.                             |
| CSV files                       | Source data format.                                                     |

# 16. Project Workflow

- Data Source → Obtain the eight Adventure Works CSV files.

- Data Ingestion → Load each file into Power BI through Power Query.

- Data Exploration → Review fields, records, data types and available dimensions.

- Data Cleaning → Promote headers, set types, remove or rename fields, standardise codes and filter blank rows.

- Data Modelling → Create relationships between sales, returns and the dimension tables.

- Calculated Columns → Compute Revenue and Cost in the sales table and add Calendar attributes.

- DAX Measures → Create Profit, Profit Margin, Total Revenue, Previous Year Revenue and YoY Revenue Growth Rate.

- Validation → Check for nulls, duplicates, key matches and date coverage.

- Visualization → Build charts, tables and cards.

- Dashboard → Combine KPIs, visuals and slicers into two report pages.

- Insight Generation → Interpret the patterns.

- Documentation → Record methodology, findings, limitations and recommendations.

# 17. Workbook (Report) Structure

The Power BI file contains 8 loaded data tables, 5 DAX measures, 7 calculated columns, 8 relationships and 2 report pages. The tables are summarised below; the full structure is in the companion document (`Adventure_Works_Data_Dictionary_DAX_and_Workbook_Structure`).

| **Table**             | **Type**  | **Role**                                   |
|-----------------------|-----------|--------------------------------------------|
| Sales_2020-2022       | Fact      | 56,046 order lines; holds the DAX measures |
| Returns Data          | Fact      | 1,809 return records                       |
| Customer              | Dimension | 18,148 customers                           |
| Product               | Dimension | 293 products with cost and price           |
| Product Subcategories | Dimension | 37 subcategories                           |
| Product Category      | Dimension | 4 categories                               |
| Territory             | Dimension | 10 regions                                 |
| Calendar              | Dimension | 912 dates                                  |

Report pages: **Page 1** (Sales Overview) with 13 visuals and **Page 2** (Profitability, Growth and Returns) with 11 visuals (including a navigation button). Power BI also created hidden automatic date tables for date fields; they are system generated and not part of the design.

# 18. Conclusion

The Adventure Works Sales Analysis project shows an end-to-end Power BI workflow: eight raw CSV files were cleaned in Power Query, modelled as a star schema, enriched with DAX measures and presented in an interactive two-page dashboard.

The analysis shows a profitable business with a stable margin of about 42%, rapid growth between 2020 and 2021, and revenue concentrated in bikes and in a few regions (Australia, the US Southwest and Northwest). It also highlights opportunities in accessories, a data or market gap in three US regions, and a higher return rate for bikes.

Future versions could add full 2022 data, actual transaction prices and discounts, returns netted off revenue, sales targets, customer segmentation (income, occupation, education) and scheduled data refresh so the report moves from historical reporting towards forecasting.

# Appendix A: Data Dictionary

The full data dictionary, DAX measure reference and data model structure are provided in the companion document, “Adventure Works – Data Dictionary, DAX Measures and Workbook Structure”.
