# Blinkit-Grocery-Data
## 📊 Project Overview: Blinkit Sales Performance Dashboard
This interactive Power BI dashboard visualizes key business metrics for Blinkit – India’s Last Minute App, focusing on sales performance across various dimensions such as outlet type, size, location, and item category. The dashboard enables data-driven decision-making for retail strategy, product mix, and outlet optimization.

<p align="center">
  <img src="Power BI Blinkit Dashboard.png"  width="500"/>
</p>

## 🔍 Key Features:
1. **Total Sales:** $1.20M with average sales of $141 per item.

2. **Item Insights:** 8,523 items analyzed across multiple categories like Fruits, Snacks, Dairy, Meat, and more.

3. **Fat Content Breakdown:** Distribution of items between Low Fat and Regular fat content.

4. **Outlet Type & Location Analysis:**

+ Covers Tier 1, Tier 2, Tier 3 cities.

+ Compares performance of Supermarkets and Grocery Stores.

5. **Average Ratings:** 3.9 across all outlets and items.

6. **Establishment Trend:** Tracks outlet sales from 2012 to 2022.

7. **Filters:** Users can slice data by Outlet Location Type, Outlet Size, and Item Type.

## Steps In Project
1. Understanding Business requirements.
2. Data Walkthrough
3. Data Connection
4. Data Cleaning
5. Data Modelling
6. Data Procession
7. DAX Calculation
8. Dashboard Layouting
9. Chart Devlopment and Formatting
10. Insights Generation

## 📊 Business Requirement
To conduct a comprehensive analysis of Blinkit's sales performance, customer satisfaction, and inventory distribution to identify key insights and opportunities for optimization using various KPIs and visualizations in Power BI.

### 📌 KPI Requirements
1. **Total Sales:** The overall revenue generated from all items sold.

2. **Average Sales:** The average revenue per sale.

3. **Number of Items:** The total count of different items sold.

4. **Average Rating:** The average customer rating for items sold.

### 📈 Chart Requirements
**1. Total Sales by Fat Content**
+ **Objective:** Analyze the impact of fat content on total sales.
+ **Additional KPI Metrics**: Assess how other KPIs (Average Sales, Number of Items, Average Rating) vary with fat content.
+ **Chart Type:** Donut Chart

**2. Total Sales by Item Type**
+ **Objective**: Identify the performance of different item types in terms of total sales.
+ **Additional KPI Metrics**: Assess how other KPIs (Average Sales, Number of Items, Average Rating) vary with item type.
+ **Chart Type**: Bar Chart

**3. Fat Content by Outlet for Total Sales**
+ **Objective:** Compare total sales across different outlets segmented by fat content.
+ **Additional KPI Metrics:** Assess how other KPIs (Average Sales, Number of Items, Average Rating) vary with fat content.
+ **Chart Type:** Stacked Column Chart

**4. Total Sales by Outlet Establishment**
+ **Objective:** Evaluate how the age or type of outlet establishment influences total sales.
+ **Chart Type**: Line Chart

## 🔍 Key Insights
**🏪 Outlet Performance**
+ Tier 3 locations generated the highest total sales ($472.13K), followed by Tier 2 ($393.15K) and Tier 1 ($336.40K), indicating stronger market traction in smaller cities.

+ Supermarket Type 1 contributed the majority of total sales ($787.55K) and had the highest number of items (5,577), highlighting its dominance in both inventory and revenue.

+ All outlet types had a consistent average rating of 3.9, showing uniform customer satisfaction.

**📦 Item Analysis**
+ Fruits & Vegetables (1,232) and Snack Foods (1,200) are the top item types, reflecting frequent daily use and high consumer demand.

+ Low fat content items (6K) outnumber Regular fat items (3K), indicating customer preference for Low Fat variants.

+ Tier 3 outlets offer the widest fat content range (2.2K items), suggesting strong inventory diversification in these markets.

**📅 Sales Trends Over Time**
+ Sales peaked in 2018 ($205K), possibly indicating a successful expansion year or promotional campaigns.

**🧱 Outlet Size Contribution**
+ Medium-sized outlets contributed the most to sales ($507.90K), followed by High ($444.79K) and Small ($248.99K), highlighting the efficiency of mid-sized formats.

### SQL QUERIES

 **📌 Create and use database**
CREATE DATABASE BlinkitDb;
USE BlinkitDb;

**📌 Inspect data**
SELECT * FROM blinkit_data;
SELECT COUNT(*) FROM blinkit_data;
SHOW COLUMNS FROM blinkit_data;

**📌 Fix column encoding issue**
ALTER TABLE blinkit_data
CHANGE COLUMN `ï»¿Item Fat Content` `Item Fat Content` TEXT;

**📌 Disable safe updates**
SET SQL_SAFE_UPDATES = 0;

**📌 Standardize Item Fat Content values**
UPDATE blinkit_data
SET `Item Fat Content` = 
  CASE 
    WHEN `Item Fat Content` IN ('LF','Low Fat') THEN 'Low Fat'
    WHEN `Item Fat Content` IN ('reg','Regular') THEN 'Regular'
    ELSE `Item Fat Content`
  END;

**📌 Check distinct Item Fat Content values**
SELECT DISTINCT `Item Fat Content` FROM blinkit_data;

**📌 Overall Sales Metrics**
SELECT CAST(SUM(Sales)/1000000 AS DECIMAL(10,2)) AS Total_Sales_Millions FROM blinkit_data;
SELECT CAST(AVG(Sales) AS DECIMAL(10,2)) AS Avg_Sales FROM blinkit_data;
SELECT COUNT(*) AS No_of_Items FROM blinkit_data;
SELECT CAST(AVG(`Rating`) AS DECIMAL(10,2)) AS Average_Rating FROM blinkit_data;

**📌 Sales for Low Fat category**
SELECT CAST(SUM(Sales)/1000000 AS DECIMAL(10,2)) AS Total_Sales_Millions
FROM blinkit_data
WHERE `Item Fat Content`='Low Fat';

**📌 Sales for outlets established in 2022**
SELECT CAST(SUM(Sales)/1000000 AS DECIMAL(10,2)) AS Total_Sales_Millions
FROM blinkit_data
WHERE `Outlet Establishment Year`=2022;

SELECT COUNT(*) AS No_of_Items
FROM blinkit_data
WHERE `Outlet Establishment Year`=2022;

**📌 Fat Content-wise Sales Summary**
SELECT `Item Fat Content`, 
  CAST(SUM(Sales) AS DECIMAL(10,2)) AS Total_Sales,
  CAST(AVG(Sales) AS DECIMAL(10,2)) AS Average_Sales,
  COUNT(*) AS No_of_items,
  CAST(AVG(rating) AS DECIMAL(10,2)) AS Average_Ratings
FROM blinkit_data
GROUP BY `Item Fat Content`
ORDER BY Total_Sales DESC;

**📌 Fat Content Sales for 2022**
SELECT `Item Fat Content`, 
  CAST(SUM(Sales/1000) AS DECIMAL(10,2)) AS Total_Sales_Thousand,
  CAST(AVG(Sales) AS DECIMAL(10,2)) AS Average_Sales,
  COUNT(*) AS No_of_items,
  CAST(AVG(rating) AS DECIMAL(10,2)) AS Average_Ratings
FROM blinkit_data
WHERE `Outlet Establishment Year`=2022
GROUP BY `Item Fat Content`
ORDER BY Total_Sales_Thousand DESC;

**📌 Item Type Analysis (Bottom 5)**
SELECT `Item Type`, 
  CAST(SUM(Sales) AS DECIMAL(10,2)) AS Total_Sales,
  CAST(AVG(Sales) AS DECIMAL(10,2)) AS Average_Sales,
  COUNT(*) AS No_of_items,
  CAST(AVG(rating) AS DECIMAL(10,2)) AS Average_Ratings
FROM blinkit_data
GROUP BY `Item Type`
ORDER BY Total_Sales ASC
LIMIT 5;

**📌 Outlet Location and Fat Content Analysis**
SELECT `Outlet Location Type`,`Item Fat Content`, 
  CAST(SUM(Sales) AS DECIMAL(10,2)) AS Total_Sales,
  CAST(AVG(Sales) AS DECIMAL(10,2)) AS Average_Sales,
  COUNT(*) AS No_of_items,
  CAST(AVG(rating) AS DECIMAL(10,2)) AS Average_Ratings
FROM blinkit_data
GROUP BY `Outlet Location Type`,`Item Fat Content`
ORDER BY Total_Sales DESC;

**📌 Pivot-style Location vs Fat Content Sales**
SELECT
  `Outlet Location Type`,
  SUM(CASE WHEN `Item Fat Content` = 'Low Fat' THEN Sales ELSE 0 END) AS `Low Fat`,
  SUM(CASE WHEN `Item Fat Content` = 'Regular' THEN Sales ELSE 0 END) AS `Regular`
FROM
  blinkit_data
GROUP BY
  `Outlet Location Type`
ORDER BY
  `Outlet Location Type`;

**📌 Year-wise Analysis**
SELECT `Outlet Establishment Year`, 
  CAST(SUM(Sales) AS DECIMAL(10,2)) AS Total_Sales,
  CAST(AVG(Sales) AS DECIMAL(10,2)) AS Average_Sales,
  COUNT(*) AS No_of_items,
  CAST(AVG(rating) AS DECIMAL(10,2)) AS Average_Ratings
FROM blinkit_data
GROUP BY `Outlet Establishment Year`
ORDER BY `Outlet Establishment Year` ASC;

**📌 Outlet Size Analysis**
SELECT `Outlet Size`,
  CAST(SUM(Sales) AS DECIMAL(10,2)) AS Total_Sales,
  CAST((SUM(Sales)*100/SUM(SUM(Sales)) OVER ()) AS DECIMAL(10,2)) AS Sales_Percentage
FROM blinkit_data
GROUP BY `Outlet Size`
ORDER BY Total_Sales DESC;

**📌 Outlet Location Type Analysis**
SELECT `Outlet Location Type`, 
  CAST(SUM(Sales) AS DECIMAL(10,2)) AS Total_Sales,
  CAST(AVG(Sales) AS DECIMAL(10,2)) AS Average_Sales,
  COUNT(*) AS No_of_items,
  CAST(AVG(rating) AS DECIMAL(10,2)) AS Average_Ratings
FROM blinkit_data
GROUP BY `Outlet Location Type`
ORDER BY Total_Sales DESC;

**📌 Outlet Type Analysis**
SELECT `Outlet Type`, 
  CAST(SUM(Sales) AS DECIMAL(10,2)) AS Total_Sales,
  CAST((SUM(Sales)*100/SUM(SUM(Sales)) OVER ()) AS DECIMAL(10,2)) AS Sales_Percentage,
  CAST(AVG(Sales) AS DECIMAL(10,2)) AS Average_Sales,
  COUNT(*) AS No_of_items,
  CAST(AVG(rating) AS DECIMAL(10,2)) AS Average_Ratings
FROM blinkit_data
GROUP BY `Outlet Type`;


## ✅ Conclusion
This dashboard highlights Blinkit’s strong performance in Tier 3 cities and medium-sized outlets, with Supermarket Type 1 leading in sales and inventory. Consumer preference leans toward regular fat products and essential categories like fruits and snacks. The stable sales trend and 2018 peak offer key strategic insights. Overall, the dashboard enables data-driven decisions for retail growth and product optimization.
