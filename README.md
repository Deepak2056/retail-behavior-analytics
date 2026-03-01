# Customer Behavior Analytics — AWS + Athena + Power BI
This is a behavior analytics project of retail transactions

## Project Overview

This project demonstrates an end-to-end behavioral analytics pipeline built using:
Amazon S3 (Cloud Storage)
Amazon Athena (Serverless SQL Engine)
Power BI (Visualization & BI Layer)
The objective was to simulate a real-world data warehouse workflow:
Raw Data → Staging Layer → Analytics Layer → BI Dashboard

The project focuses on customer behavior analysis, repeat purchasing patterns, revenue concentration, time-based ordering behavior, and cancellation trends.

## Business Objective

To analyze transactional retail data and answer key behavioral questions:
What % of customers are repeat buyers?
What % of revenue comes from the top 20% of customers?
Do customers increase or decrease spending over time?
What time of the day do customers prefer to order?
Which period has the highest cancellations?
What is the average purchase cadence?
Do repeat customers behave differently from first-time buyers?
The goal was to transform raw transactional data into structured analytical insights.

## Data Source

Publicly available retail transactional dataset.

Data Characteristics:
Type: Transactional (e-commerce style)
Format: CSV
Storage: Amazon S3
Structure: Line-item level (1 row per invoice-product)
Fields include:
Invoice ID
Product Code
Description
Quantity
Unit Price
Invoice Timestamp
Customer ID
Country

## Architecture
Amazon S3 (Raw CSV)
        ↓
Athena RAW Tables
        ↓
Staging Layer (Cleaned Data)
        ↓
Analytics Tables (Invoice, Customer, Customer-Month)
        ↓
Power BI (Import Mode via ODBC)

## Data Modeling Approach

Strict grain discipline was enforced throughout the project.
Each table was defined by:
“One row represents ______.”

🔹 RAW Layer
External tables created over CSV files in S3.
Fixed parsing issues (row format, delimiters).
Corrected data types (timestamp casting, numeric handling).
Grain:
One row = one invoice-product (line item)

🔹 Staging Layer
Standardized column names
Trimmed string fields
Casted timestamp correctly
Validated nulls and negative quantities
Preserved returns for behavioral analysis

🔹 Analytics Layer

1️⃣ invoice_summary
Grain:
One row represents one invoice
Includes:
Invoice ID
Customer ID
Invoice Date
Invoice Total
Order Hour
Return Flag
Country

2️⃣ customer_summary
Grain:
One row represents one customer
Includes:
Customer ID
Total Orders
Total Revenue

3️⃣ customer_month_summary
Grain:
One row represents one customer per month
Includes:
Customer ID
Month Start Date
Monthly Orders
Monthly Revenue

4️⃣ product_summary
Grain:
One row represents one product
Includes:
Product Code
Total Quantity Sold
Total Revenue

## Analytical Techniques Used

Aggregations (SUM, COUNT)
Window Functions (LAG, ROW_NUMBER)
Ranking (NTILE for Pareto Analysis)
Date Truncation
Revenue Trend Analysis
Purchase Cadence Calculation

## Power BI Modeling
🔹 Connection Setup
Installed Athena ODBC Driver (64-bit)
Configured IAM user with programmatic access
Set S3 output location
Connected via DSN
Used Import Mode for performance

🔹 Data Model Structure
Fact Tables:
invoice_summary
customer_month_summary
Dimension Tables:
customer_summary
Date Table (created in Power BI)
All relationships:
One-to-many
Single direction
Star-schema inspired design

## Dashboard Structure
🏠 Page 1 — Executive Overview
Total Revenue
Total Orders
Total Customers
% Repeat Customers
% Revenue from Top 20%
Revenue Trend
Cancellation Trend

👥 Page 2 — Customer Behavior
Average Orders per Customer
Average Revenue per Customer
Average Days Between Purchases
Spending Growth Trend
Repeat vs First-Time Revenue Contribution

🕒 Page 3 — Time & Seasonality
Orders by Hour
Orders by Day of Week
Revenue by Month
Seasonal Patterns

🌍 Page 4 — Geographic Insights
Revenue by Country
Repeat Customers by Country
Return Rate by Country

## Key Metrics Implemented
Total Revenue
Total Orders
Total Customers
% Repeat Customers
% Revenue from Top 20%
Return Rate
Average Purchase Cadence
Time to Second Order
Revenue Growth (MoM)
Average Order Value (AOV)

## Challenges Faced
CSV row parsing errors in Athena
Invalid cast argument during timestamp conversion
Handling negative quantities (returns vs cancellations)
Grain mismatch in customer dimension
Many-to-many relationship errors in Power BI
ODBC authentication configuration issues
Deciding where logic belongs (SQL vs BI layer)
Each issue improved architectural clarity and modeling discipline.

## Key Design Decisions
Enforced strict grain definitions
Avoided storing unstable attributes in dimensions
Separated behavioral logic from presentation layer
Used Parquet for cost-efficient querying
Chose Import Mode in Power BI to reduce query cost
Built proper Date dimension instead of relying on transactional dates

## Outcome
This project demonstrates:
End-to-end cloud-based data pipeline
Serverless querying with Athena
Dimensional modeling principles
Behavioral analytics using SQL
Professional BI modeling practices
Structured, business-driven dashboard design

It reflects real-world analytics engineering workflow rather than just dashboard creation.
