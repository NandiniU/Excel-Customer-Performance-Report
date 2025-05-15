## Customer Performance Report

This **Customer Performance Report** captures key insights for AtliQ Technologies’ B2B hardware business. AtliQ sells hardware products exclusively through distributor partners—such as Croma, Amazon Business, Flipkart, and more—and relies on this report to:

* Track each distributor’s sales performance over time
* Highlight growth opportunities and identify at-risk partners
* Compare year-over-year net-sales trends across regions and product divisions
* Enable data-driven decision making for pricing, inventory, and channel strategy

By distilling complex sales data into clear, interactive views, this report empowers AtliQ’s leadership team to optimize distributor relationships and drive sustainable revenue growth.

---
## 📷 Screenshot
![SS](https://github.com/user-attachments/assets/a0e626cb-5077-4570-9916-e9d0d83161ee)


### 📊 Report Overview

The **Customer Performance Report** is an interactive Excel report built to give AtliQ’s leadership team deep insights into customer behavior, revenue trends, and engagement metrics. It combines robust data modeling with professional formatting to spotlight key areas of performance.

A PDF export (`india_sales.pdf`) is provided alongside the Excel workbook to review the static version of the report. The report itself is driven by four core tables parsed from the source files:

* **dim\_customer.csv**: Customer master data including fields product_code,	customer_code,	Qty &	net_sales_amount
* **dim\_market.csv**: Geographic and market segmentation details
* **dim\_product.csv**: Product hierarchy and categories
* **fact\_sales\_monthly.csv**: Monthly sales transactions and revenue metrics

These parsed tables are loaded and related in Power Pivot to form the data model used by the final report (`Customer_Performance_Report.xlsx`).

---

### 🔧 Excel Tools & Techniques Used

* **Power Pivot & Measures**

  * Data modeling engine for large datasets
  * DAX measures to calculate KPIs (e.g. customer lifetime value, churn rate, revenue growth)

* **Power Pivot (ETL)**

  * Extract, transform, and load the parsed tables into the data model
  * Data relationships set up to optimize filtering and analysis

* **Data Relationships & RELATED Function**

  * One-to-many joins between Customers, Markets, Products, and Sales facts
  * `RELATED()` used in calculated columns to pull in descriptive attributes

* **Conditional Formatting**

  * Highlights top‐performing segments, red-flags at-risk cohorts, and revenue outliers
  * Custom icon sets and data bars for at-a-glance interpretation

* **Professional Report Design**

  * Emphatic layout with clear section headers and slicers
  * Consistent color palette aligned with AtliQ branding
  * Interactive slicers for time periods, customer segments, and product lines

---

### 🚀 How to Use

1. **Download or clone** this repository.
2. **Open** `Customer_Performance_Report.xlsx` in Excel 2016 or later (Power Pivot enabled).
3. If needed, review the **data model** by opening the Power Pivot window to see how the parsed tables are related.
4. **Enable** external data connections if prompted.
5. Use the **Slicers** on the right pane to filter by:

   * Date range (Month/Quarter/Year)
   * Customer segment
   * Product category
6. Review the **KPIs** on the top ribbon and the **Performance Charts** below:

   * Revenue by segment
   * New vs. returning customer trends
   * Average order value over time
7. Drill into detail tables by clicking on any chart marker.

---

### 📂 Repository Structure

```
├── Customer_Performance_Report.xlsx   # Main report file
├── india_sales.pdf                    # PDF export of the report
├── Insights.pptx                      # Key insights of the report
├── data/                              # Source tables in CSV format in a zip file
│   ├── dim_customer.csv
│   ├── dim_market.csv
│   ├── dim_product.csv
│   └── fact_sales_monthly.csv
└── README.md                          # This file
```

---
### ⚙️ Workflow Steps

**Before creating the report:** Perform ETL on the source CSVs (`dim_customer.csv`, `dim_market.csv`, `dim_product.csv`, `fact_sales_monthly.csv`): extract data, remove duplicates/wrong/empty values, apply transformations, and load into the data model (create connections only).


1. **Collect transformed data**

   * In Power Pivot, click **Manage**.
   * Import fields: Net Sales, Year, Division, Country, Region from the parsed tables (derive missing fields as needed).
2. **Prepare fields**

   * Net Sales exists in the fact table.
   * Extract Year from the date column using a formula.
   * Pull Division, Country, and Region from `dim_market`.
3. **Connect data model**

   * In Power Pivot’s Diagram View, position the fact table centrally (star schema).
   * Create relationships:

     * fact\_sales\_monthly\[customer\_code] → dim\_customer\[customer\_code]
     * fact\_sales\_monthly\[product\_code] → dim\_product\[product\_code]
     * dim\_customer\[market] → dim\_market\[market]
4. **Build `dim_date` table**

   * In Power Query, create a new blank query with:

     ```powerquery
     ={Number.From(#date(2018,1,1))..Number.From(#date(2018,12,31))}
     ```
   * Convert the list to a table, change the column type to Date, and rename to `Date`.
   * Add custom column `FY Month`: `Date.AddMonths([Date], 4)` (AtliQ’s FY is Sep–Aug).
   * Add custom column `FY Year`: `Date.Year([FY Month])`.
5. **Insert PivotTable**

   * From the data model, insert a PivotTable.
   * Set filters: Region, Market, Division.
   * Set Rows: Customer.
6. **Create FY measures**

   * Use `RELATED(dim_date[FY Year])` to bring FY into the fact table for filtering in measures.
   * Define measures for each year, e.g.:

     ```DAX
     NetSales_2019 := CALCULATE([Net Sales], dim_date[FY Year] = "2019")
     ```
   * Format measures as currency (2 decimals, US \$).
7. **Calculate % change**

   * Define measure for 2021 vs 2020:

     ```DAX
     PctChange_21vs20 := DIVIDE([NetSales_2021], [NetSales_2020], 0)
     ```
   * Format as percentage (1 decimal).
8. **Convert to millions**

   * In Value Field Settings for each year measure, choose Number Format → Custom → `0.0,,"M"`.
9. **Beautify report**

   * Apply consistent fonts, borders, conditional formatting (highlights, data bars) to emphasize insights.
10. **Add trend chart**

    * Click any cell within the PivotTable and insert a Line Chart (Insert → Charts → Line) to represent trend analysis over time.

> **Note:** Row and column names may differ; adjust to align with your mock-up.

---

### 🤝 Contributions

Open for contributions:

1. Fork this repo
2. Add new measures or visualizations
3. Submit a pull request with your updates—ensure all DAX formulas are annotated in the “Data Model” sheet.

---
### Report by:

- **Author:** *Nandini Upadhyay*
- **Date:** *Apr-May 2025*

---
