Excited to share my latest End-to-End SaaS Sales Analytics Project! 🚀

As a Data Analyst, I wanted to dive deep into the core mechanics of SaaS metrics, so I built a complete data pipeline to analyze sales performance, tracking foundational KPIs like ARR, ACV, and Churn Rate.

Here is a breakdown of the pipeline:
1️⃣ Data Generation (Python): Simulated a raw dataset of 1,200 contracts and sales team details using Pandas and NumPy, intentionally embedding missing data and structural anomalies to mimic real-world scenarios.
2️⃣ Data Engineering & Cleaning (SQL Server): Developed robust SQL Views to handle NULL values, standardize categorical fields, and perform multi-table JOINs to prepare clean data for downstream analytics.
3️⃣ BI & Data Visualization (Power BI): Built an interactive dashboard focused on executive decision-making:
   - Formulated accurate DAX Measures to calculate true active Annual Recurring Revenue (ARR), historical Annual Contract Value (ACV), and customer Churn Rate.
   - Designed intuitive visuals to breakdown revenue by Product Tiers, evaluate regional performance, and highlight the top-performing Sales Representatives.

💡 Key Insight from the project: Understanding that ARR is a current financial metric (requiring active/renewed contracts only), while ACV is a historical sales performance metric (encompassing all closed contracts) is vital for accurate SaaS business modeling.

Always open to feedback! How do you typically structure your sales dashboards? 📊👇





##ScreenShoot for Dashboard

<img width="612" height="342" alt="Screenshot 2026-06-25 014046" src="https://github.com/user-attachments/assets/07f740f3-0475-414c-8569-8e6fbf1cc7d1" />

# Housing_Data
markdown
# Nashville Housing Data Cleaning & Analysis Project

## 📌 Project Overview
This project demonstrates a full-scale Data Analytics workflow, combining professional data cleaning using **SQL** and interactive business intelligence dashboard design using **Power BI**. 

The dataset contains historical home sales data for Nashville, Tennessee. The goal was to clean raw, unformatted real estate records and transform them into an enterprise-level interactive dashboard that provides real estate investors with actionable, data-driven insights.

---

## 🛠️ Phase 1: Data Cleaning (SQL)
Before visualizing the data, extensive data cleaning was performed to ensure accuracy and consistency. The following major operations were implemented:
1. **Handling Missing Values:** Screened and filtered out rows with `BLANK` or `NULL` tax districts to avoid skewing summary statistics.
2. **Standardizing Data Formats:** Cleaned date formats and rounded numeric fields (such as average sale prices) to ensure clear readability.
3. **Segmentation & Binning:** Categorized properties into specific **Building Eras** (`Modern (2000+)`, `1980-1999`, `Pre-1980`) using conditional logical statements (`CASE WHEN`).
4. **Outlier Removal:** Filtered bedroom counts to focus on standard properties (between 1 and 6 bedrooms) to avoid extreme outliers impacting the visual representations.

---

## 📊 Phase 2: Interactive Dashboard (Power BI)
An enterprise-grade, single-page interactive dashboard was built featuring:
* **Key Performance Indicators (KPIs):** Real-time tracking of *Total Properties*, *Cleaned Average Price*, and *Average Lot Size*.
* **Geographical Distribution:** A comprehensive breakdown of property metrics by `Tax District`.
* **Seasonal Trend Analysis (Line Chart):** A dual-axis time-series visualization displaying monthly sales volume alongside average sale price trends. 
* **Key Strategic Insight:** The trend analysis revealed that **June serves as the peak season** with the highest average prices, leading to a core business recommendation: *Investors looking to maximize purchasing power should avoid buying during June to hedge against overpriced seasonal corrections.*
* **Property Classification:** Charts outlining the average price by building eras and bedroom configurations.

---

## 💾 Core SQL Queries Used

### 1. Geographical Summary (Tax District)
```sql
 SELECT TaxDistrict AS District,
       COUNT(UniqueID) AS TOTAL_PROPERTIES,
       ROUND(AVG(CAST(SalePrice AS BIGINT)), 0) AS AVG_Price      
FROM  housing
WHERE TRIM(city) ='NASHVILLE'
      AND TaxDistrict IS NOT NULL
      AND TRIM(TaxDistrict) <> '' --
GROUP BY TaxDistrict
ORDER BY AVG_Price DESC;```

### 2. Time_Series & Seasonal Trend Analysis


```sql
 SELECT 
    YEAR(Sale_date) AS Sale_Year,
    MONTH(Sale_date) AS Sale_Month_Number,
    DATENAME(month, Sale_date) AS Sale_Month_Name,
    COUNT(UniqueID) AS Properties_Sold,
    ROUND(AVG(CAST(SalePrice AS BIGINT)), 0) AS AVG_Price
FROM 
   housing
WHERE 
    TaxDistrict IS NOT NULL AND TaxDistrict <> ''
GROUP BY 
    YEAR(Sale_date), 
    MONTH(Sale_date), 
    DATENAME(month, Sale_date)
ORDER BY 
    Sale_Year, 
    Sale_Month_Number;```

### 3.Building Eras Segmentation


```sql
SELECT 
    CASE 
        WHEN YearBuilt >= 2000 THEN 'Modern (2000+)'
        WHEN YearBuilt BETWEEN 1980 AND 1999 THEN '1980-1999'
        WHEN YearBuilt < 1980 THEN 'Pre-1980'
        ELSE 'Unknown'
    END AS Building_Era,
    COUNT(UniqueID) AS Total_Properties,
    ROUND(AVG(CAST(SalePrice AS BIGINT)), 0) AS AVG_Price
FROM 
    housing
WHERE 
    YearBuilt IS NOT NULL
GROUP BY 
    CASE 
        WHEN YearBuilt >= 2000 THEN 'Modern (2000+)'
        WHEN YearBuilt BETWEEN 1980 AND 1999 THEN '1980-1999'
        WHEN YearBuilt < 1980 THEN 'Pre-1980'
        ELSE 'Unknown'
    END
ORDER BY 
    AVG_SalePrice DESC;```

