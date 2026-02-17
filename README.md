#  Olist E-Commerce Data Preparation & Modelling Project

##  Project Overview

This project focuses on data preparation, transformation, and dimensional modelling using the **Olist Brazilian E-Commerce Public Dataset** in Microsoft Power BI.

The objective was to perform:

* Data profiling and quality assessment
* Data cleaning and transformation
* Text standardization
* Conditional and custom column creation
* Aggregation (Group By / Merge)
* Star schema modelling
* Relationship validation
* Key uniqueness verification
* Date table implementation

The final result is a clean, analysis-ready star schema model suitable for business intelligence reporting.

---

##  Dataset Information

**Source:** Kaggle
**Dataset Name:** Olist Brazilian E-Commerce Public Dataset

### Tables Used

| Original Table               | Renamed To      |
| ---------------------------- | --------------- |
| olist_orders_dataset         | Fact_Orders     |
| olist_order_items_dataset    | Fact_OrderItems |
| olist_order_payments_dataset | Fact_Payments   |
| olist_customers_dataset      | Dim_Customers   |
| olist_products_dataset       | Dim_Products    |
| olist_sellers_dataset        | Dim_Sellers     |

---

##  Tools Used

* Microsoft Power BI Desktop
* Power Query Editor
* DAX (for Date Table creation)

---

#  Section A – Data Preparation

## 1️ Data Profiling

Enabled:

* Column Quality
* Column Distribution
* Column Profile (Entire Dataset)

### Key Data Quality Issues Identified

1. Missing delivery dates
2. Null product categories
3. Multiple payment records per order
4. Inconsistent text formatting in city names

Each issue was resolved using appropriate Power Query transformations (Replace, Trim, Clean, Group By, Type Correction).

---

## 2️ Data Type Corrections

| Column                   | Final Type     |
| ------------------------ | -------------- |
| order_purchase_timestamp | Date/Time      |
| payment_installments     | Whole Number   |
| payment_value            | Decimal Number |

Locale conversion was applied where necessary using:

Change Type → Using Locale

---

## 3️ Text Standardization

Performed on customer_city:

* Trim
* Clean
* Capitalize Each Word
* Column Renaming

---

## 4️ Conditional & Custom Columns

### Conditional Column

Payment Category:

* High
* Medium
* Low

### Custom Column (M Formula)

```m
if [order_delivered_customer_date] = null then null 
else Duration.Days([order_delivered_customer_date] - [order_purchase_timestamp])
```

Column Created: Delivery Days

---

## 5️ Data Aggregation / Merge

A Merge operation was performed:

Fact_Orders joined with Dim_Customers
Join Key: customer_id
Join Type: Left Outer

Only required columns were expanded:

* Customer City
* customer_state

---

#  Section B – Data Modelling

##  Star Schema Design

### Fact Tables

* Fact_Orders
* Fact_OrderItems
* Fact_Payments

### Dimension Tables

* Dim_Customers
* Dim_Products
* Dim_Sellers

The model follows a classic star schema structure with one-to-many relationships.

---

##  Relationships Configuration

All relationships were configured as:

* Cardinality: One-to-Many
* Cross Filter Direction: Single
* Active: Yes

Example:

Fact_Orders → Fact_OrderItems
Fact_Orders → Dim_Customers

This prevents ambiguity and ensures accurate aggregation.

---

##  Key Uniqueness Verification

Primary keys such as:

* customer_id
* product_id
* seller_id

were verified using Group By (Count Rows) to ensure uniqueness.

---

##  Date Table

A dedicated Date Table was created using DAX:

```DAX
DateTable =
ADDCOLUMNS(
    CALENDAR(
        MIN(Fact_Orders[order_purchase_timestamp]),
        MAX(Fact_Orders[order_purchase_timestamp])
    ),
    "Year", YEAR([Date]),
    "Month", FORMAT([Date], "MMMM"),
    "Quarter", "Q" & FORMAT([Date], "Q")
)
```

Reasons for Date Table:

* Enables Time Intelligence (YTD, MTD, MoM)
* Ensures consistent date filtering

---

#  Data Dictionary (Sample)

| Table           | Column        | Description             | Type    |
| --------------- | ------------- | ----------------------- | ------- |
| Fact_Orders     | order_id      | Unique order identifier | Text    |
| Fact_OrderItems | price         | Product selling price   | Decimal |
| Fact_Payments   | payment_value | Amount paid             | Decimal |
| Dim_Customers   | customer_id   | Unique customer ID      | Text    |
| Dim_Products    | product_id    | Unique product ID       | Text    |
| Dim_Sellers     | seller_id     | Unique seller ID        | Text    |

---

#  Project Outcomes

✔ Cleaned and standardized dataset
✔ Proper data typing applied
✔ Quality issues resolved
✔ Star schema implemented
✔ Relationships validated
✔ Date table created
✔ Model ready for reporting and analytics

---

#  Files Included

* Power BI (.pbix) file
* Screenshots (Data Profiling, Applied Steps, Model View)
* Report PDF
* README.md

---

#  Conclusion

This project demonstrates the full data preparation lifecycle in Power BI, from raw CSV files to a fully structured dimensional model. The final model supports scalable business intelligence reporting and advanced time-based analytics.

---

