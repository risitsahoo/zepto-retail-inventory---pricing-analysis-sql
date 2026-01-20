# 📊 Zepto SQL Data Analysis Project  
### Retail Inventory & Pricing Analysis Using SQL

---

## 📌 Table of Contents

- [Project Overview](#-project-overview)
- [Business Problem Statement](#-business-problem-statement)
- [Business Objectives](#-business-objectives)
- [Dataset Overview](#-dataset-overview)
- [Tools, Technologies & Skills Used](#-tools-technologies--skills-used)
- [Database Design & Table Creation](#-database-design--table-creation)
- [Data Exploration](#-data-exploration)
- [Data Cleaning & Transformation](#-data-cleaning--transformation)
- [Data Analysis & SQL Queries](#-data-analysis--sql-queries)
- [Files in This Repository](#-files-in-this-repository)
- [Key Business Insights](#-key-business-insights)
- [Future Work & Business Recommendations](#-future-work--business-recommendations)
- [Conclusion](#-conclusion)
- [About the Author](#-about-the-author)

---

## 📘 Project Overview

In today’s fast-paced e-commerce environment, data plays a crucial role in managing inventory, pricing strategies, discounts, and customer experience.

This project performs an end-to-end **SQL-based exploratory data analysis (EDA)** on a real-world **Zepto grocery delivery dataset** to derive meaningful business insights related to:

- Product pricing and discounts  
- Stock availability and inventory risks  
- Revenue contribution by category  
- Product weight and unit economics  

The project closely mirrors real responsibilities of a **Data Analyst working in retail, FMCG, or e-commerce organizations**.

---

## 🧩 Business Problem Statement

Zepto manages thousands of SKUs across multiple product categories. Without proper data analysis, the business may face:

- Revenue loss due to stock-out products  
- Inefficient discount strategies  
- Poor pricing optimization  
- Overstocking or understocking issues  
- Low-margin product prioritization  

A structured SQL analysis is required to convert raw catalog data into actionable business insights.

---

## 🎯 Business Objectives

The key objectives of this project are:

- Understand dataset structure and quality  
- Identify missing or inconsistent data  
- Analyze stock availability status  
- Evaluate pricing and discount effectiveness  
- Estimate revenue contribution by category  
- Identify high-value and underperforming products  
- Support business decision-making using SQL  

---

## 🗂 Dataset Overview

The dataset represents Zepto’s product catalog data.

| Column Name | Description |
|------------|-------------|
| sku_id | Unique SKU identifier |
| product_name | Product name |
| category | Product category |
| mrp | Maximum Retail Price |
| discountPercent | Discount percentage |
| discountedSellingPrice | Final selling price |
| weightInGms | Product weight (grams) |
| availableQuantity | Available inventory units |
| outOfStock | Stock availability flag |
| quantity | Pack quantity |

---

## 🛠 Tools, Technologies & Skills Used

### 🔧 Tools
- MySQL Workbench  
- Microsoft Excel (CSV preprocessing)

### 💡 Technical Skills
- SQL Queries  
- Exploratory Data Analysis (EDA)  
- Data Cleaning & Transformation  
- Aggregations & Joins  
- Business KPI Analysis  

### 📘 SQL Concepts Applied
- SELECT, WHERE  
- GROUP BY, HAVING  
- ORDER BY  
- Aggregate Functions (SUM, AVG, COUNT)  
- CASE Statements  
- Boolean Logic  

---

## 🧱 Database Design & Table Creation

- Database created using **MySQL Workbench**
- Single relational table: **`zepto`**
- UTF-8 encoding used to prevent import errors
- Appropriate data types applied for:
  - Pricing columns  
  - Quantity fields  
  - Boolean stock indicators  

---

## 🔍 Data Exploration

### ✔ Total Records
```sql
SELECT COUNT(*) FROM zepto;
```

### ✔ Sample Data Review
```sql
SELECT * FROM zepto
LIMIT 10;
```

### ✔ NULL Value Detection
```sql
SELECT *
FROM zepto
WHERE product_name IS NULL
   OR category IS NULL
   OR mrp IS NULL
   OR discountPercent IS NULL
   OR discountedSellingPrice IS NULL
   OR weightInGms IS NULL
   OR availableQuantity IS NULL
   OR outOfStock IS NULL
   OR quantity IS NULL;
```

### ✔ Unique Categories
```sql
SELECT DISTINCT category
FROM zepto
ORDER BY category;
```

### ✔ Stock Availability Analysis
```sql
SELECT outOfStock, COUNT(sku_id)
FROM zepto
GROUP BY outOfStock;
```

### ✔ Duplicate Product Names
```sql
SELECT product_name, COUNT(sku_id) AS Number_of_SKUs
FROM zepto
GROUP BY product_name
HAVING COUNT(sku_id) > 1
ORDER BY COUNT(sku_id) DESC;
```

--- 

## 🧹 Data Cleaning & Transformation

### 🔄 Price Conversion (Paise → Rupees)
```sql
UPDATE zepto
SET mrp = mrp / 100.0,
    discountedSellingPrice = discountedSellingPrice / 100.0;
```

### ✅ Validation
```sql
SELECT mrp, discountedSellingPrice
FROM zepto;
```

--- 

## 📈 Data Analysis & SQL Queries

### 🔹 Top 10 Best-Value Products
```sql
SELECT DISTINCT product_name, mrp, discountPercent
FROM zepto
ORDER BY discountPercent DESC
LIMIT 10;
```

### 🔹 High-MRP Products Out of Stock
```sql
SELECT DISTINCT product_name, mrp
FROM zepto
WHERE outOfStock = TRUE
AND mrp > 300
ORDER BY mrp DESC;
```

### 🔹 Estimated Revenue by Category
```sql
SELECT category,
SUM(discountedSellingPrice * availableQuantity) AS total_revenue
FROM zepto
GROUP BY category
ORDER BY total_revenue DESC;
```

### 🔹 High-Priced Products With Low Discounts
```sql
SELECT DISTINCT product_name, mrp, discountPercent
FROM zepto
WHERE mrp > 500
AND discountPercent < 10
ORDER BY mrp DESC;
```

### 🔹 Top 5 Categories by Average Discount
```sql
SELECT category,
ROUND(AVG(discountPercent), 2) AS avg_discount
FROM zepto
GROUP BY category
ORDER BY avg_discount DESC
LIMIT 5;
```

### 🔹 Price Per Gram Analysis
```sql
SELECT DISTINCT product_name, weightInGms,
discountedSellingPrice,
ROUND(discountedSellingPrice / weightInGms, 2) AS price_per_gram
FROM zepto
WHERE weightInGms >= 100
ORDER BY price_per_gram;
```

### 🔹 Product Weight Classification
```sql
SELECT product_name, weightInGms,
CASE
   WHEN weightInGms < 1000 THEN 'Low'
   WHEN weightInGms < 5000 THEN 'Medium'
   ELSE 'Bulk'
END AS weight_category
FROM zepto;
```

### 🔹 Total Inventory Weight by Category
```sql
SELECT category,
SUM(weightInGms * availableQuantity) AS total_weight
FROM zepto
GROUP BY category
ORDER BY total_weight DESC;
```

---

## 📂 Files in This Repository

```

Zepto SQL Data Analysis Project
│
├── 📊 zepto_retail_inventory_&_pricing_analysis_dataset.csv            # Project dataset
├── 📄 zepto_retail_inventory_&_pricing_analysis_sql_questions.pdf      # Project problems
├── 🖼 zepto_retail_inventory_&_pricing_analysis_sql_query.pdf           # Project solutions
├── 📘 zepto_retail_inventory_&_pricing_analysis_report.pdf             # Detailed project report
├── 📘 readme.md                                                        # Project documentation
```

---

## 💡 Key Business Insights

- Several high-value SKUs are currently out of stock, leading to potential revenue loss  
- A limited number of categories contribute majority of total revenue  
- Discount allocation is uneven across categories  
- Bulk-weight products offer better unit price efficiency  
- Duplicate SKUs indicate multiple pack-size strategies  
- Inventory weight distribution varies significantly across categories  

---

## 🚀 Future Work & Business Recommendations

- Integrate sales transaction data for profit-level analysis  
- Create Power BI / Tableau dashboards for visualization  
- Perform time-series analysis on stock movement  
- Identify slow-moving and dead stock products  
- Apply dynamic pricing models based on demand  
- Combine customer order data for market basket analysis  
- Automate reporting using SQL procedures or Python scripts  

---

## ✅ Conclusion

This project demonstrates a complete **SQL-driven Data Analyst workflow**, including:

- Structured data exploration  
- Data cleaning and normalization  
- Advanced SQL querying  
- Business KPI evaluation  
- Insight generation aligned with real e-commerce operations  

The analysis supports smarter decision-making in:

- Inventory management  
- Pricing optimization  
- Discount planning  
- Revenue maximization  

This project closely reflects real-world analytics tasks performed in **retail and e-commerce companies**.

---

## 👤 About the Author

**Risit Sahoo**  
📧 Email: risit.sahoo121@gmail.com  
🔗 LinkedIn: https://linkedin.com/in/risitsahoo

---

