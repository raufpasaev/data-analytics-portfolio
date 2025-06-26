# data-analytics-portfolio
My SQL portfolio for data analyst job
--Beginner-Level SQL Analysis

--1. Total Sales and Profit
SELECT 
SUM(sales) AS total_sales
FROM supperrstoredata

--2. Top 5 Products by Sales
SELECT TOP 5 
 Product_Name,
 SUM(Sales) As total_sales
 FROM supperrstoredata
 GROUP BY  Product_Name
 ORDER BY total_sales DESC

 --3. Sales by Category
 SELECT 
Category,
 SUM(sales) AS total_sales
 FROM supperrstoredata
 GROUP BY Category;

 --5. Orders per Region
 SELECT 
 Region,
 COUNT(*) AS total_orders
 FROM supperrstoredata
 GROUP BY Region;

  --SECTION 2: Intermediate-Level SQL Analysis
 -- 6. Average Delivery Time by Ship Mode
 SELECT 
 Ship_Mode,
 AVG(DATEDIFF(DAY,Order_Date,Ship_Date)) AS Avg_delivery_days
 FROM supperrstoredata
 GROUP BY Ship_Mode;

--7. Monthly Sales Trend
SELECT 
    FORMAT(Order_Date, 'yyyy-MM') AS Month,
    SUM(Sales) AS Monthly_Sales
FROM superstore_data
GROUP BY FORMAT(Order_Date, 'yyyy-MM')
ORDER BY Month;

--8. Top Customers by Revenue
SELECT TOP 10 
Customer_Name,
SUM(Sales) AS total_Sales
FROM supperrstoredata
GROUP BY Customer_Name
ORDER BY total_Sales DESC 

--9.Profit by Category and Sub-Category
SELECT 
Category, Sub_Category,
SUM(Sales) AS total_profit 
FROM supperrstoredata 
GROUP BY Category, Sub_Category
ORDER BY Category, total_profit DESC;


--ADVANCED SQL QUERIES

-- 10. What are the top-selling products per month,
--and how do they rank by sales?

WITH MonthlySales AS (
    SELECT 
        Product_Name,
        FORMAT(Order_Date, 'yyyy-MM') AS Order_Month,
        SUM(Sales) AS Monthly_Sales
    FROM supperrstoredata
    GROUP BY Product_Name, FORMAT(Order_Date, 'yyyy-MM')
)
SELECT 
    Order_Month,
    Product_Name,
    Monthly_Sales,
    RANK() OVER (PARTITION BY Order_Month ORDER BY Monthly_Sales DESC) AS Sales_Rank
FROM MonthlySales
ORDER BY Order_Month, Sales_Rank;

-- 11. Which customers ordered in multiple years (e.g., 2015 AND 2016)?
WITH YearlyCustomers AS (
SELECT 
Customer_ID,
Customer_Name, 
YEAR(Order_Date) AS Order_Year
FROM supperrstoredata
GROUP BY Customer_ID, Customer_Name, YEAR(Order_Date)
)
SELECT yc1.Customer_ID, yc1Customer_Name
FROM YearlyCustomers AS yc1
LEFT JOIN YearlyCustomers AS yc2
ON yc1.Customer_ID =yc2.Customer_ID
WHERE  yc1.Order_Year = 2015 AND yc2.Order_Year = 2016
GROUP BY yc1.Customer_ID, yc1.Customer_Name;
