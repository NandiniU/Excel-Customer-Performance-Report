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

A PDF export (`Report PDF.pdf`) is provided alongside the Excel workbook to review the static version of the report. The report itself is driven by four core tables parsed from the source files:

* **dim\_customer.csv**: Customer master data including fields product_code,	customer_code,	Qty &	net_sales_amount
* **dim\_market.csv**: Geographic and market segmentation details
* **dim\_product.csv**: Product hierarchy and categories
* **fact\_sales\_monthly.csv**: Monthly sales transactions and revenue metrics

These parsed tables are loaded and related in Power Pivot to form the data model used by the final report (`Final Report.xlsx`).

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
    * Open Power Query, load the fact table, and add the FY column from `dim_date` using the formula:

      ```powerquery
      =RELATED(dim_date[FY])
      ```
   * Click **Close & Load** to persist the new FY column in the model.
   * Return to the PivotTable sheet and define a measure for 2019:

      ```DAX
      NetSales_2019 := CALCULATE([Net Sales].dim_date[FY] = "2019")
      ```

      * Format as US \$ currency with two decimals.
   * Repeat steps 3 for `NetSales_2020` and `NetSales_2021`, adjusting the FY filter accordingly.

   
7. **Calculate % change**

   * Define measure for 2021 vs 2020:

     ```DAX
     21vs20 := DIVIDE([NetSales_2021], [NetSales_2020], 0)
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
### 🔧 Excel Tools & Techniques Used

* **ETL with Power Query**

  * Extract data from `dim_customer.csv`, `dim_market.csv`, `dim_product.csv`, `fact_sales_monthly.csv`
  * Cleanse data: remove duplicates, empty or invalid values
  * Transform fields and load parsed tables into the data model (create connections only)

* **Data Collection in Power Pivot**

  * Open Power Pivot → Manage, and import the required fields: Net Sales, Year, Division, Country, Region
  * Derive missing fields (e.g., Year from date) as needed

* **Field Preparation with Formulas**

  * Year: extract from date column using DAX or Power Query
  * Division, Country, Region: sourced from `dim_market`

* **Data Model Connection (Star Schema)**

  * In Diagram View, place `fact_sales_monthly` centrally and create relationships:

    * fact\[customer\_code] → dim\_customer\[customer\_code]
    * fact\[product\_code] → dim\_product\[product\_code]
    * dim\_customer\[market] → dim\_market\[market]

* **Date Table Creation (dim\_date)**

  * In Power Query, generate date list:

    ```powerquery
    ={Number.From(#date(2018,1,1))..Number.From(#date(2018,12,31))}
    ```
  * Convert to table, set type to Date, then add:

    * `FY Month` = Date.AddMonths(\[Date], 4)
    * `FY Year` = Date.Year(\[FY Month])

* **PivotTable Setup**

  * Insert a PivotTable from the data model
  * Configure Filters: Region, Market, Division
  * Set Rows to Customer

* **Annual Net Sales Measures**

  * Use `RELATED(dim_date[FY Year])` to enable year-based filtering
  * Define measures per year, e.g.:

    ```DAX
    NetSales_2019 := CALCULATE([Net Sales], dim_date[FY Year] = "2019")
    ```
  * Format as currency (2 decimals, US \$)

* **Year-over-Year % Change**

  * Create measure:

    ```DAX
    21vs20 := DIVIDE([NetSales_2021], [NetSales_2020], 0)
    ```
  * Format as percentage (1 decimal)

* **Display in Millions**

  * In Value Field Settings for each year measure, use custom format: `0.0,,"M"`

* **Report Beautification**

  * Apply consistent fonts, borders, and slicer styles
  * Use conditional formatting (highlights, data bars) to emphasize insights

* **Trend Analysis Chart**

  * Click any PivotTable cell and insert a Line Chart (Insert → Charts → Line) to show trends over time

---
### Report by:

- **Author:** *Nandini Upadhyay*
- **Date:** *Apr-May 2025*

---
