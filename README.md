[![Open In Power Bi](https://img.shields.io/badge/open_in_power_bi-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)](https://app.powerbi.com/view?r=eyJrIjoiNDE5NDEyMWUtY2NiMi00MzRiLTljZmYtYmZlNWQzOTBkMDgzIiwidCI6ImFlZDI3MWNkLTYzOTgtNDllZi1hOWNmLTQ4NDIyMTAxZTE0ZSIsImMiOjEwfQ%3D%3D)

# Sales / Customer / Product Analysis — Power BI

> 📌 **About this repository**
> This project was reproduced and studied by **Hicham ERRIHANI** as a hands-on Business Intelligence practice exercise, based on the original project designed by **Sunil S. Singh** ([sssingh](https://github.com/sssingh/sales-customer-product-analysis-powerbi)). All credit for the original design, data model, and SQL logic goes to the original author (see [Credits](#credits)). This repository is kept for demonstrating my understanding of end-to-end BI workflows: SQL data preparation, Power Query transformation, dimensional modeling, and interactive Power BI report design.

In this project, we analyze `AdventureWorks`, an online retailer's raw sales data, to draw meaningful business insights.

<img src="https://github.com/sssingh/sales-customer-product-analysis-powerbi/blob/main/images/title.png?raw=true" width="1000" height="800" />

## What I practiced through this project
⚡ Building a data source with Microsoft SQL Server / SQL / T-SQL
⚡ Designing dashboards in Power BI Desktop
⚡ Data transformation and modeling with Power Query Editor
⚡ Publishing an interactive report via Power BI Service
⚡ Building a multipage, fully interactive report for business analysis

## Table of Contents
- [Introduction](#introduction)
- [Objectives](#objectives)
- [Dataset](#dataset)
- [Solution Approach](#solution-approach)
- [How To Use](#how-to-use)
- [License](#license)
- [Credits](#credits)
- [About Me](#about-me)

## Introduction

* `AdventureWorks` is an online retailer that sells `Bikes` and `Biking-related` items such as bike parts, biking protective gear, and clothing.
* Online sales transactions, inventory, financials, customer, and product information are captured in real-time in a `transaction database`.
* At the end of each business day, data from the transaction database is extracted, formatted, and exported to a `data warehouse database`.

## Objectives
The goal is to perform in-depth data analysis for the years `2016 and 2017` and draw insight into sales performance, customers, and products, so the business can build a strategy to generate more revenue and higher profits. Specific requirements addressed:

|Requirement ID|For Whom|Requirement Description|
|:--|:---|:--|
AW-DA01-REQ-1|Head of Sales|A high-level overview of internet sales by various dimensions such as `customers`, `products`, `customer-cities`, `quarter`
AW-DA01-REQ-2|Head of Sales|Track `sales performance over time` against the `budget/target`
AW-DA01-REQ-3|Head of Sales|Ability to dynamically slice/dice/filter data by `year`, `month`, `product-attributes`
AW-DA01-REQ-4|Sales Rep|A detailed overview of sales by `customers`
AW-DA01-REQ-5|Sales Rep|A detailed overview of sales by `products`
AW-DA01-REQ-6|Sales Rep|Ability to dynamically slice/dice/filter and analyze data by `year`, `month`, `product-attributes`, `customer-attributes`

***Table-1: Requirements***

## Dataset
AdventureWorks makes data available through its `data warehouse database`. The real-time transaction database is not directly accessible.

### AdventureWorks Data Warehouse
<img src="https://github.com/sssingh/sales-customer-product-analysis-powerbi/blob/main/images/DW%20Schema.png?raw=true" width="400" height="600" />

The complete data warehouse database, as a Microsoft SQL Server `database backup`, is available from the original author's [repository](https://github.com/sssingh/sales-customer-product-analysis-powerbi) — see the [How To Use](#how-to-use) section for restore instructions.

### Budget Data
AdventureWorks allocates a monthly sales budget/target, decided yearly by top management, provided as an XLS file:

<img src="https://github.com/sssingh/sales-customer-product-analysis-powerbi/blob/main/images/budget.png?raw=true" width="400" height="600" />

## Solution Approach

|Requirement ID|Solution ID|Proposed Solution|
|:--|:---|:--|
AW-DA01-REQ-1 <br> AW-DA01-REQ-2 <br> AW-DA01-REQ-3|AW-DA01-SOL-1|An `Executive Summary` Power BI page showing a high-level overview of sales data, with year/month slicers and filters
AW-DA01-REQ-4 <br> AW-DA01-REQ-6|AW-DA01-SOL-2|A `Customer Analysis` Power BI page segmenting sales by customer attributes (top customers, gender, marital status, city)
AW-DA01-REQ-5 <br> AW-DA01-REQ-6|AW-DA01-SOL-3|A `Product Analysis` Power BI page segmenting sales by product attributes (category, subcategory, top products)

***Table-2: Proposed Solution***

### Exploratory Data Analysis and Data Preparation [SQL]

#### Key tables identified
|Table|Description|
|:--|:--|
|`DimDate`|Date-related information
|`DimCustomer`|Customer-related information
|`DimGeography`|Customer geography information
|`DimProduct`|Product-related information
|`DimProductCategory`|Product category information
|`DimProductSubcategory`|Product subcategory information
|`FactInternetSales`|Sales fact table

***Table-3: Database Tables***

#### Data Preparation approach
Instead of importing raw joined `SELECT` statements directly into Power BI, four SQL `VIEWS` were created (`vw_date`, `vw_customer`, `vw_product`, `vw_internet_sales`) to encapsulate the logic. Benefits:
* Simpler Power BI-side data import
* No manual extract/CSV management — refreshing the report pulls the latest data directly
* Decoupled selection logic — changes to the view don't require Power BI changes, as long as output columns remain stable

The full SQL used to create these views is in `sales-analysis.sql`.

### Data Cleaning and Transformation [Power Query Editor]
The four SQL views plus the budget XLS file were imported as `Dim_Customer`, `Dim_Product`, `Dim_Date`, `Fact_Internet_Sales`, and `Fact_Budget` queries. Minimal cleaning was required:
* Correcting column headings
* Assigning correct data types

### Data Model [Power BI Desktop]
The imported dimension and fact tables were manually linked into a `star schema`:

<img src="https://github.com/sssingh/sales-customer-product-analysis-powerbi/blob/main/images/data-model.png?raw=true"/>

**Note:** the prefix `DIM` denotes a dimension table, `FACT` denotes a fact table.

### Report Pages [Power BI Desktop]

#### 1. Executive Summary [AW-DA01-SOL-1]
Overall sales figures, top customers, top products, and Sales vs. Budget KPI at a glance.

<img src="https://github.com/sssingh/sales-customer-product-analysis-powerbi/blob/main/images/exec-summary-page.png?raw=true"/>

#### 2. Customer Analysis [AW-DA01-SOL-2]
Detailed sales analysis from the customer perspective.

<img src="https://github.com/sssingh/sales-customer-product-analysis-powerbi/blob/main/images/cust-analysis-page.png?raw=true"/>

#### 3. Product Analysis [AW-DA01-SOL-3]
Detailed sales analysis from the product perspective.

<img src="https://github.com/sssingh/sales-customer-product-analysis-powerbi/blob/main/images/prod-analysis-page.png?raw=true"/>

## How To Use

### Read-only access via the web (recommended)
[![Open In Power Bi](https://img.shields.io/badge/open_in_power_bi-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)](https://app.powerbi.com/view?r=eyJrIjoiNDE5NDEyMWUtY2NiMi00MzRiLTljZmYtYmZlNWQzOTBkMDgzIiwidCI6ImFlZDI3MWNkLTYzOTgtNDllZi1hOWNmLTQ4NDIyMTAxZTE0ZSIsImMiOjEwfQ%3D%3D)
Explore the fully functional report with the native Power BI interactive experience.

### Full access via Power BI Desktop
If you have Power BI Desktop installed, download `sales-analysis.pbix` from this repo and open it directly — the `.pbix` file contains the complete normalized data model, no need to download raw data.

To reconnect to a live SQL Server data source instead:
* Install [Microsoft SQL Server Express](https://www.microsoft.com/en-us/download/details.aspx?id=101064) and [SQL Server Management Studio](https://learn.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-ver16#download-ssms)
* Restore the AdventureWorksDW2019 backup (see the original author's repository for the download link)
* Run `sales-analysis.sql` in SSMS to (re)create the four required views
* Open `sales-analysis.pbix`, click Refresh, and enter your SQL Server credentials

## License
[![MIT License](https://img.shields.io/badge/License-MIT-green.svg)](https://choosealicense.com/licenses/mit/)

## Credits
- Original project design, data model, and report concept: **Sunil S. Singh** — [GitHub](https://github.com/sssingh) · [Original repository](https://github.com/sssingh/sales-customer-product-analysis-powerbi)
- AdventureWorks dataset sourced from [Microsoft AdventureWorks sample databases](https://learn.microsoft.com/en-us/sql/samples/adventureworks-install-configure?view=sql-server-ver16&tabs=ssms)

## About Me
**Hicham ERRIHANI** — Data Scientist / Data Analyst / BI Developer, based in Casablanca, Morocco.
Building an end-to-end Business Intelligence portfolio (Power BI, SQL Server, SSIS, SSAS, DAX, dimensional modeling), targeting freelance BI missions and full-time roles in the banking/financial sector.

📧 Email: hichamerrihani.pro@gmail.com
🔗 GitHub: [Hicham-Errihani](https://github.com/Hicham-Errihani)
🔗 LinkedIn: [Hicham Errihani](https://www.linkedin.com/in/hicham-errihani-815755266)
🌐 Portfolio: [hicham-errihani-portfolio.vercel.app](https://hicham-errihani-portfolio.vercel.app)

[Back To The Top](#salescustomerproduct-analysis--power-bi)
