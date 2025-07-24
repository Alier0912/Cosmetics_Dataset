
## Overview

This project analyzes sales data from a cosmetics store, stored in a PostgreSQL database. The dataset (cosmetics_sales_data.csv) contains records of sales transactions, including salesperson, country, product, date, amount, and boxes shipped. The analysis is performed using SQL queries in [cosmetic_db.session.sql](c:\Users\ratib\OneDrive\Документы\Desktop\Cosmetics_Dataset\cosmetic_db.session.sql).

## Database Structure

- **Table:** `cosmetic_store`
  - `sales_person` (VARCHAR)
  - `country` (VARCHAR)
  - `product` (VARCHAR)
  - `date` (DATE)
  - `amount` (FLOAT)
  - `boxes_shipped` (INT)
 
  ---

```sql
CREATE TABLE cosmetic_store (
sales_person VARCHAR(50),
country VARCHAR(50),
product VARCHAR(50),
date DATE,
amount FLOAT,
boxes_shipped INT
)
```

*viewing the table*
```sql
SELECT * FROM cosmetic_store
```

*ANALYSIS AND INSIGHTS*
1. Top 10 salesperson by total sales
```sql
SELECT 
sales_person,
ROUND(SUM(amount)::integer,0) AS total_sales,
COUNT(*) AS total_orders
FROM cosmetic_store
GROUP BY 1
ORDER BY 2 DESC
LIMIT 10
```

2. Top 10 countries by total sales
```sql
SELECT 
country,
ROUND(SUM(amount)::integer,0) AS total_sales
FROM cosmetic_store
GROUP BY 1
ORDER BY 2 DESC
```

3. Top 10 products by sales
```sql
SELECT 
product,
ROUND(SUM(amount)::integer,0) AS total_sales,
COUNT(*) AS total_orders
FROM cosmetic_store
GROUP BY 1
ORDER BY 2 DESC
```

4. Monthly sales overview
```sql
SELECT
TO_CHAR(date, 'Month') AS month,
ROUND(SUM(amount)::integer,0) AS total_sales,
COUNT(*) AS total_orders
FROM cosmetic_store
GROUP BY 1
ORDER BY 3 DESC
```

5. Day of the week sales analysis (peak days)
```sql
SELECT
TO_CHAR(date, 'Day') AS day_of_week,
ROUND(SUM(amount)::integer,0) AS total_sales,
COUNT(*) AS total_orders
FROM cosmetic_store
GROUP BY 1
ORDER BY 3 DESC
```

7. Segmentating amount to see the distribution of sales
```sql
WITH sales_seg AS
(
SELECT
product,
amount,
CASE
    WHEN amount < 1000 THEN 'Low'
    WHEN amount >= 1000 AND amount < 5000 THEN 'Medium'
    WHEN amount >= 5000 AND amount < 10000 THEN 'High'
    ELSE 'Very High'
END AS sales_segment,
ROUND(SUM(amount)::integer,0) AS total_sales,
COUNT(*) AS total_orders
FROM cosmetic_store
GROUP BY 1,2
ORDER BY 2 DESC
)
SELECT 
product,
sales_segment
FROM sales_seg
GROUP BY 1,2
```
---

## Key Analyses & Findings

### 1. Top 10 Salespersons by Total Sales
- Identifies the highest-performing salespeople.
- **Finding:** Certain salespersons consistently generate higher sales and more orders, indicating strong performance and potential for leadership or mentorship roles.

### 2. Top Countries by Total Sales
- Ranks countries by total sales volume.
- **Finding:** Some countries (e.g., USA, UK, Canada) are major markets, suggesting targeted marketing and resource allocation.

### 3. Top Products by Sales
- Highlights best-selling products.
- **Finding:** Products like "Aloe Vera Gel," "Body Butter Cream," and "Hydrating Face Serum" are top performers, indicating customer preference and potential for upselling or bundling.

### 4. Monthly Sales Overview
- Shows sales trends by month.
- **Finding:** Certain months have higher sales, revealing seasonality. Promotional campaigns can be timed to maximize sales during peak months.

### 5. Day of the Week Sales Analysis
- Analyzes sales by day of the week.
- **Finding:** Some days consistently see higher sales, suggesting optimal timing for promotions and staffing.

### 6. Sales Segmentation by Amount
- Segments sales into "Low," "Medium," "High," and "Very High" categories.
- **Finding:** Most sales fall into "High" and "Very High" segments for certain products, indicating premium product positioning and opportunities for loyalty programs.

---

## Recommendations

1. **Focus on High-Performing Salespersons:**  
   Provide incentives, training, and leadership opportunities to top sales staff to further boost sales and share best practices.

2. **Target Major Markets:**  
   Allocate more marketing resources and inventory to countries with the highest sales. Consider localized campaigns for these regions.

3. **Promote Best-Selling Products:**  
   Bundle or upsell top products. Use customer feedback to develop new products similar to best-sellers.

4. **Leverage Seasonality:**  
   Plan promotions and stock management around peak sales months. Launch new products or campaigns during high-traffic periods.

5. **Optimize Weekly Operations:**  
   Increase staffing and run special offers on days with historically higher sales.

6. **Segmented Marketing:**  
   Develop targeted campaigns for different sales segments. For "Very High" sales, introduce loyalty programs or exclusive offers.

---

*Awal Alier ; Reading between the data!*

