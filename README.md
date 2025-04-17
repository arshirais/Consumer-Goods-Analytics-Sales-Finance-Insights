# Consumer-Goods-Analytics-Sales-Finance-Insights SQL Project

Overview
=========================

This repository contains SQL queries designed for the analysis of consumer goods data. The project comprises nine tasks, each with its dedicated SQL script that addresses different analytical requirements.
Project Structure

   Q1. Croma India product wise sales report for fiscal year 2021
   Description
   As a product owner, I want to generate a report of individual product sales (aggregated on a monthly basis at the product code level) for Croma India customer for FY-2021 so that I can track individual product sales and run further product analytics on it in Excel.
   The report should have the following fields:
   1.	Month
   2.	Product Name
   3.	Variant
   4.	Sold Quantity
   5.	Gross Price Per Item
   6.	Gross Price Total

•	Description: This query generates product-wise sales reports for Croma India for the fiscal year 2021. It utilizes functions to accurately determine the fiscal year.
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 Q2. Gross monthly total sales report for Croma
    Description
    As a product owner, I need an aggregate monthly gross sales report for Croma India customer so that I can track how much sales this particular customer is generating for AtliQ and manage our relationships accordingly.
    The report should have the following fields:
    1.	Month
    2.	Total gross sales amount to Croma India in this month
•	Description: This script calculates the gross monthly total sales for Croma.
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
   Q3. Created stored procedure for monthly gross sales report
     Description
     As a data analyst, I want to create a stored proc for monthly gross sales report so that I don’t have to manually modify the query every time. Stored proc can be run by other users (who have limited access to database) and they can generate this report without having to involve the 
       data analytics team.
      The report should have the following columns:
      1.	Month
      2.	Total gross sales in that month from a given customer
  
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 Q4. Stored procedure for market badge
Description
Create a stored proc that can determine the market badge based on the following logic:
If total sold quantity > 5 million, that market is considered Gold, else it is Silver.
My input will be:
  •	market
  •	fiscal year
•	Description: A stored procedure that assigns a 'Gold Badge' to markets with total sold quantities ≥ 5 Million and a 'Silver Badge' to those with less. Inputs required are "Market" and "Fiscal Year." 
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 Q5. Top markets, products, customers for a given financial year
•	Description
   •	As a product owner, I want a report for top markets, products, customers by net sales for a given financial year so that I can have a holistic view of our financial performance and can take appropriate actions to address any potential issues.
•	Description: These queries identify the top N entities in their respective categories based on net sales. Inputs for these stored procedures include "Top N number" and "Fiscal Year."
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 Q6. Net sales % share Global
Description
As a product owner, I want to see a bar chart report for FY=2021 for top 10 markets by % net sales.
 •	Description: This query calculates the net sales percentage share for the top 10 markets using CTEs and the window function OVER().
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Q7. Net Sales % Share by region 
 Description
  As a product owner I want to see region wise (APAC, EU, LTAM etc)% net sales breakdown by customers in a respective region so that I can perform my regional analysis on financial performance of the company 
The end result should be “donut charts” in the following format for FY = 2021, build a reusable asset that we can use to conduct this analysis for any financial year.

•	Description: Similar to Task 5, this script calculates the net sales percentage share by region, using CTEs and the window function OVER() with PARTITION BY.
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 Q8. Top N Products in Each Division by Quantity Sold.
 
 •	Description: This query identifies the top N products in each division based on the quantity sold. It uses a stored procedure, CTEs, and the window function OVER(PARTITION BY).
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
   Q9. top 2 markets in every region by their gross sales amount 
          in FY=2021.
  •	Description: Retrieves the top 2 markets in every region based on gross sales for the respective fiscal year. It employs a stored procedure, CTEs, and the window function OVER(PARTITION BY).
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Q10. stored procedure that takes market, fiscal_year and top n as an input and
          returns top n customers by net sales in that given fiscal year and market

•	Description: This query identify the top N entities in their respective categories based on net sales. Inputs for this  stored procedures include "Top N customers" and "Fiscal Year" & "markets"
