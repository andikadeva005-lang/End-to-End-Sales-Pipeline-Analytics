# End-to-End Sales Pipeline Analytics & Market Performance Dashboard
An interactive Power BI dashboard tracking global e-commerce sales performance, dynamic time intelligence metrics (YTD/QTD/MTD), and international market share analysis by handling single-market dominance.
---
## Project Overview
A comprehensive, end-to-end data analytics project that demonstrates data ingestion, in-database pre-filtering using **PostgreSQL**, and advanced business intelligence modeling using **Power BI** (Power Query M-Code & DAX).
---
## Tech Stack & Architecture
* **Data Source:** Raw E-Commerce Transaction Dataset (Kaggle)
* **Data Ingestion & Cleaning:** PostgreSQL (via DBeaver Studio)
* **ETL & Architecture:** Power Query (M Language)
* **Data Modeling & Analytics:** Power BI Desktop (Star Schema & Advanced DAX)
---
## Data Pipeline Implementation Details
### 1. In-Database Data Pre-Filtering (PostgreSQL Stage)
To optimize data processing and maintain high data integrity, raw transactional data was filtered and cleaned directly within the PostgreSQL database using **DBeaver**. 

The strategy focused on removing non-operational transactions, adjustments, and corrupted entries. Below is the production-grade query utilized:
```sql
SELECT
    "InvoiceNo",
    "StockCode",
    "Description",
    "Quantity",
    "InvoiceDate",
    "UnitPrice",
    "CustomerID",
    "Country"
FROM etl_pipeline_data.data
WHERE "Quantity" > 0 
  AND "UnitPrice" > 0
  AND "Description" NOT SIMILAR TO '%(POST|FEE|DOTCOM|Postage|CARRIAGE|CHARGE|DISCOUNT|Manual|MANUAL|CRUK|ADJUST|DEBT|BANK|SAMPLE|AMAZON)%'
  AND "Description" IS NOT NULL;
  ```
### Key Improvements Made via SQL:
1. Eliminated Negative/Zero Values: Filtered out negative quantities and price anomalies which represent canceled or broken data lines.
2. Strategic Noise Cancellation: Utilized regex-like pattern matching (NOT SIMILAR TO) to strip out systemic administrative entries (e.g., Postage, Bank Charges, Discounts, Samples) ensuring the final dataset strictly contains actual product sales revenue.
3. Null Handling: Excluded missing product descriptions to safeguard dimension mapping.

### 2. ETL Transformation & Schema Modeling (Power BI Stage)
The optimized dataset was exported into a clean schema structure and connected directly to Power BI for high-level transformation:
1. Power Query (M Language): Applied schema optimization, enforced strict data typing (Dates, Text, and Currencies), and engineered localized adjustments.
2. Data Modeling: Constructed a high-performance Star Schema by establishing optimized relationships between the transaction fact tables and engineered dimension components.
3. Advanced Analytics (DAX Measures): Formulated robust Time Intelligence metrics including dynamic Year-to-Date (YTD), Quarter-to-Date (QTD), and Month-to-Date (MTD) revenue calculations to track performance pacing across calendar dimensions.

### 3. Mitigating Single-Market Bias (Outlier Handling via Filter Stacking)
Challenge: The United Kingdom (UK) heavily dominated the dataset, commanding 85.5% of the global market share. Keeping the UK inside the regional comparison visual would scale-compress other nations, rendering international expansion analysis useless.
Solution: Isolated the UK's massive footprint into an executive macro KPI card (UK Market Share %). For the global breakdown, an advanced Filter Stacking layer was implemented (combining Basic Filtering and Top N) to show a clean, un-biased view of the Top 10 International Markets.
---
## Key Business Insights (Dec 2024 - Dec 2025)
* **Total Revenue:** Generated $10.2M in total sales with an active global customer footprint of 4K buyers.
* **Time-Intelligence Accruals:** Year-to-Date (YTD) performance closed at $9.4M, Quarter-to-Date (QTD) at $3.2M, and Month-to-Date (MTD) at $0.6M.
* **Product Performance:** The Average price category acted as the primary revenue engine, contributing $5.6M, significantly outpacing High Value ($2.5M) and Low Cost ($2.1M).
* **The Holiday Surge:** Sales saw a parabolic surge during Q4 2025, peaking in November at $1.44M before a steep drop-off in December, marking the end of the seasonal retail shopping cycle.
---
## Dashboard Preview
Below is the polished, presentation-ready Executive Overview Dashboard, optimized for high data-ink ratio, zero vertical scrollbars, and intuitive layout hierarchy:
### ## 📂 Repository Structure
* `/SQL_Scripts`: Contains the raw `cleaning_script.sql` file used during the PostgreSQL pre-processing phase.
* `/Dashboard`: The core Power BI template file (`EndToEnd_Pipeline_Refined_Data.pbix`) containing the active data model.
* `README.md`: Main technical portfolio documentation.
