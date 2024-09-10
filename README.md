# Retail Store Analysis using SQL

## Project Overview

This project demonstrates my SQL skills and techniques to explore, clean, and analyze data. The project involves setting up a retail sales database, performing exploratory data analysis (EDA), and answering specific business questions through SQL queries using the Windows function, DateTime function, Aggregate functions, and Common table expression. 

## Project Workflow

### 1. Database Setup

Database Creation: The project started by creating a database named `sql_project` in Microsoft SQL Server Management.
Table Creation: Imported the given data in the database. A table named `Retail_Sales_Analysis` is created to store the sales data. The table structure includes columns for transaction ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount.

### 2. Data Exploration & Cleaning
   
Record Count: Determine the total number of records in the dataset.
Customer Count: Find out how many unique customers are in the dataset.
Category Count: Identify all unique product categories in the dataset.
Null Value Check: Check for any null values in the dataset and delete records with missing data.

```
USE sql_project;

ALTER TABLE Retail_Sales_Analysis
ALTER COLUMN sale_date date;

ALTER TABLE Retail_Sales_Analysis
ALTER COLUMN sale_time time(0);

-- Data Exploration and cleaning

SELECT * FROM Retail_Sales_Analysis;
SELECT COUNT(*) FROM Retail_Sales_Analysis;
SELECT DISTINCT category FROM Retail_Sales_Analysis;

SELECT * FROM Retail_Sales_Analysis WHERE
transactions_id IS NULL
OR sale_date IS NULL
OR sale_time IS NULL
OR customer_id IS NULL
OR gender IS NULL
OR category IS NULL
OR quantiy IS NULL
OR price_per_unit IS NULL
OR age IS NULL
OR cogs IS NULL
OR total_sale IS NULL;

DELETE FROM Retail_Sales_Analysis WHERE
transactions_id IS NULL
OR sale_date IS NULL
OR sale_time IS NULL
OR customer_id IS NULL
OR gender IS NULL
OR category IS NULL
OR quantiy IS NULL
OR price_per_unit IS NULL
OR age IS NULL
OR cogs IS NULL
OR total_sale IS NULL;
```

### Data Analysis & Findings
The following SQL queries were developed to answer specific business questions:

1. Write a SQL query to retrieve all columns for sales made on '2022-11-05:
```
SELECT *
FROM Retail_Sales_Analysis
WHERE sale_date = '2022-11-05';
```

2. Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than or equal to 4 in the month of Nov-2022.
```
SELECT *
FROM Retail_Sales_Analysis
WHERE category = 'Clothing'
  AND quantiy >= 4
  AND YEAR(sale_date) = 2022
  AND MONTH(sale_date) = 11;
```

3. Write a SQL query to calculate the total sales (total_sale) for each category.
```
SELECT
category, SUM(total_sale) as net_sale, COUNT(*) as total_orders
FROM Retail_Sales_Analysis
GROUP BY category;
```

4. Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.
```
SELECT AVG(age) AS avg_cust_age
FROM Retail_Sales_Analysis
WHERE category = 'Beauty';
```

5. Write a SQL query to find all transactions where the total_sale is greater than 1000.
```
SELECT *
FROM Retail_Sales_Analysis
WHERE total_sale > 1000;
```

6. Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.
```
SELECT
category, gender, COUNT(*) as total_trans
FROM Retail_Sales_Analysis
GROUP BY category, gender;
```

7. Write a SQL query to calculate the average sale for each month. Find out best selling month in each year.
```
SELECT 
	yr, 
	mth, 
	avg_sale 
FROM ( 
SELECT 
	YEAR(sale_date) as yr, 
	MONTH(sale_date) as mth, 
	AVG(total_sale) as avg_sale, 
	RANK() OVER(PARTITION BY YEAR(sale_date) ORDER BY AVG(total_sale) DESC) as rank
FROM Retail_Sales_Analysis
GROUP BY YEAR(sale_date), MONTH(sale_date)
) as cte
WHERE rank = 1;
```

8. Write a SQL query to find the top 5 customers based on the highest total sales
```
SELECT TOP 5
customer_id,
SUM(total_sale) as net_sale
FROM Retail_Sales_Analysis
GROUP BY customer_id
ORDER BY SUM(total_sale) DESC;
```

9. Write a SQL query to find the number of unique customers who purchased items from each category.
```
SELECT 
    category,    
    COUNT(DISTINCT customer_id) as cnt_unique_cs
FROM Retail_Sales_Analysis
GROUP BY category
```

10. Write a SQL query to create each shift and number of orders (Example Morning <12, Afternoon Between 12 & 17, Evening >17).
```
WITH hourly_sale
AS
(
SELECT *,
    CASE
        WHEN datepart(HOUR,sale_time) < 12 THEN 'Morning'
        WHEN datepart(HOUR,sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
        ELSE 'Evening'
    END as shift
FROM Retail_Sales_Analysis
)
SELECT 
    shift,
    COUNT(*) as total_orders    
FROM hourly_sale
GROUP BY shift
```
# Outcome

- Customer Demographics: The dataset includes customers from various age groups, with sales distributed across different categories such as Clothing and Beauty.
- High-Value Transactions: Several transactions had a total sale amount greater than 1000, indicating premium purchases.
- Sales Trends: Monthly analysis shows variations in sales, helping identify peak seasons.
- Customer Insights: The analysis identifies the top-spending customers and the most popular product categories.

# Reports

- Sales Summary: A detailed report summarizing total sales, customer demographics, and category performance.
- Trend Analysis: Insights into sales trends across different months and shifts.
- Customer Insights: Reports on top customers and unique customer counts per category.
