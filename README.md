# Lita Project1: Sales perfomamce analysis for a Retail Shop.
## Overview: Exploration of salesdata to uncover insight such as Top-selling product, Regional Perfomance and monthly sales target. The goal is to produced an interactive Power BI dashboard to highligt the findings using Excel, SQL & power Bi
### Capstone SalesData Analysis: Excel
#### Perform an initial exploration of the sales data. Use pivot tables to summarize total sales by product, region, and month: <img width="578" alt="Capstone_sale_pivot" src="https://github.com/user-attachments/assets/cbb579dd-f4df-48c5-acad-5f6f54adcd3b">

#### Use Excel formulas to calculate metrics such as average sales per product and total revenue by region:<img width="727" alt="Capstone_sale_excel" src="https://github.com/user-attachments/assets/8230f768-3df8-4a24-8ed1-7e34b2813044">

#### Create any other interesting report:<img width="654" alt="Capstone_cust_chart" src="https://github.com/user-attachments/assets/0bcdca60-4049-4317-a066-ed12c54422d6">

### Capstone SalesData Analysis: SQL
#### Create any other interesting report
#### Write queries to extract key insights based on the following questions. 
#### retrieve the total sales for each product category. 
$$$$ find the number of sales transactions in each region. 
#### find the highest-selling product by total sales value. 
#### calculate total revenue per product. 
#### calculate monthly sales totals for the current year. 
#### find the top 5 customers by total purchase amount. 
#### calculate the percentage of total sales contributed by each region. 
#### identify products with no sales in the last quarter.
[Uploadicreate Database LITA_Capstone_Dataset_S

Select * from [dbo].[capstone_sales]

--1-Total Sales 4 Each Product Category--

Select 
	Product,
	SUM(Quantity) AS TotalSales
FROM 
	[dbo].[capstone_sales]
GROUP BY 
	Product
ORDER BY 
	TotalSales Desc

--2-Number Of Sales Transaction In Each Region--

SELECT
	Region,
	COUNT(*) AS NoOfSalesTransactions
FROM
	[dbo].[capstone_sales]
GROUP BY 
	Region
ORDER BY 
	NoOfSalesTransactions Desc

--In order to run the next step, we'd need our revenue column

ALTER TABLE [dbo].[capstone_sales]
ADD Revenue FLOAT

--Now, we update table. Since our revenue column is empty, we need to fill it up--

UPDATE [dbo].[capstone_sales]
SET Revenue = Quantity * UnitPrice

--3-Highest Selling Product By Total Sales Value--

SELECT Top 1
	Product,
	SUM(Revenue) AS TotalSalesValue
FROM 
	[dbo].[capstone_sales]
GROUP BY 
	Product
ORDER BY 
	TotalSalesValue Desc

--4-Total Revenue Per Product--

SELECT 
	Product, 
	SUM(Revenue) AS TotalRevenuePerProduct
FROM 
	[dbo].[capstone_sales]
GROUP BY 
	Product
ORDER BY 
	TotalRevenuePerProduct DESC

--5-Monthly Sales Totals for D Current Year--

Select * from [dbo].[capstone_sales]

SELECT 
	MONTH(OrderDate) AS SalesMonth,
	SUM(Quantity) AS MonthlySalesTotal
FROM 
	[dbo].[capstone_sales]
WHERE 
	YEAR(OrderDate) = YEAR(GETDATE())
GROUP BY 
	MONTH(OrderDate)
ORDER BY 
	SalesMonth

--6- top 5 customers by total purchase amount--

SELECT *
FROM [dbo].[capstone_sales]
SELECT TOP 5 
	Customer_Id, 
	SUM(Revenue) AS TotalPurchaseAmount
FROM 
	[dbo].[capstone_sales]
GROUP BY 
	Customer_Id
ORDER BY 
	TotalPurchaseAmount DESC

--7-percentage of total sales contributed by each region--
--Without '%'
SELECT 
	Region,
	SUM(Revenue) AS RegionalSales,
	(SUM(Revenue) / (SELECT SUM(Revenue) FROM [dbo].[capstone_sales]) * 100) AS SalesPercentage
FROM 
	[dbo].[capstone_sales]
GROUP BY 
	Region
ORDER BY
	RegionalSales Desc

--With  addtion of '%' 

SELECT 
	Region,
	SUM(Revenue) AS RegionalSales,
	CONCAT(ROUND(SUM(Revenue) / (Select SUM(Revenue) FROM [dbo].[capstone_sales]) * 100, 2), '%') AS SalesPercentage
FROM
	[dbo].[capstone_sales] 
GROUP BY
	Region
ORDER BY
	RegionalSales Desc

--8- Products with no sales in the last quarter--

SELECT 
	Product
FROM 
	[dbo].[capstone_sales]
WHERE 
	Product NOT IN (
					SELECT 
					Product
					FROM 
					[dbo].[capstone_sales]
					WHERE 
					OrderDate >= DATEADD(quarter, -1, GETDATE())
					)

Select * from [dbo].[capstone_sales]





ng Create Database LITA_CAPSTONE_S.sql…]()


### Capstone SalesData Analysis: Power BI
#### Create a dashboard that visualizes the insights found in Excel and SQL. 
#### The dashboard should include a sales overview, top-performing products, and regional breakdowns :<img width="673" alt="sales report_power" src="https://github.com/user-attachments/assets/136487ab-bae4-4fde-8ba8-8899e2cc984f">


