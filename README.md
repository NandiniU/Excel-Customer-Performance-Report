## Customer Performance Report

- **Repository:** /Excel-Customer-Performance-Report
- **Report Name:** Customer Performance Report
- **Author:** *Nandini Upadhyay*
- **Date:** 12 May 2025

---
![image](https://github.com/user-attachments/assets/dad9752b-74cc-4ab8-9cac-ec3133b7f2c6)

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
├── data/                              # Source tables in CSV format
│   ├── dim_customer.csv
│   ├── dim_market.csv
│   ├── dim_product.csv
│   └── fact_sales_monthly.csv
└── README.md                          # This file
```

---

### 🤝 Contributions

Open for contributions:

1. Fork this repo
2. Add new measures or visualizations
3. Submit a pull request with your updates—ensure all DAX formulas are annotated in the “Data Model” sheet.


