# SQL_retail_sales_practice
## Project Overview

**Project Title:** Retail Sales Analysis

**Database:** sql_project_p2

This project was made to introduce myself to SQL and showcase my techniques used by data analysts to explore, clean, and analyze retail sales data. The project involves setting up a retail sales database, performing exploratory data analysis (EDA), and answering specific business questions through SQL queries.

## Objectives

1.)**Set up a retail sales database:** Create and populate a retail sales database with the provided sales data.

2.)**Data Cleaning:** Identify and remove any records with missing or null values.

3.)**Exploratory Data Analysis (EDA):** Perform basic exploratory data analysis to understand the dataset.

4.)**Business Analysis:** Use SQL to answer specific business questions and derive insights from the sales data.

## Project Structure

### 1. Database Setup
- **Database Creation:** The project starts by creating a database named `p1_retail_db`.

- **Table Creation:** A table named `retail_sales` is created to store the sales data. The table structure includes columns for transaction ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount.

``` sql
--sql retail sales analysis-p1
CREATE DATABASE sql_project_p2;

--Create table

CREATE TABLE retail_sales
(
    transactions_id INT PRIMARY KEY,
    sale_date DATE,	
    sale_time TIME,
    customer_id INT,	
    gender VARCHAR(10),
    age INT,
    category VARCHAR(35),
    quantity INT,
    price_per_unit FLOAT,	
    cogs FLOAT,
    total_sale FLOAT
);
```

### 2. Data Exploration & Cleaning

- **Record Count**: Determine the total number of records in the dataset.
- **Customer Count**: Find out how many unique customers are in the dataset.
- **Category Count**: Identify all unique product categories in the dataset.
- **Null Value Check**: Check for any null values in the dataset and delete records with missing data.
```sql
SELECT * FROM retail_sales
WHERE transactions_id IS NUll


SELECT * FROM retail_sales
WHERE 
    sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
    gender IS NULL OR age IS NULL OR category IS NULL OR 
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;

DELETE FROM retail_sales
WHERE
	sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
    gender IS NULL OR age IS NULL OR category IS NULL OR 
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;
```


--How many sales we have 
```sql
SELECT COUNT(*) as total_sale FROM retail_sales
```
--how many unnique customers do we have?
```sql
SELECT COUNT(DISTINCT customer_id) as total_sale FROM retail_sales
```
--how many unique categories do we have?
```sql
SELECT COUNT(DISTINCT category) as total_category FROM retail_sales
```

### 3. Data Analysis & Findings

--Data Analysis & Findings
```sql
--Write a SQL query to retrieve all columns for sales made on '2022-11-05:
SELECT *
FROM retail_sales
WHERE sale_date='2022-11-05';
```
--Write a SQL query to retrieve all transactions where the 
-- category is 'Clothing' and the quantity sold is more than 4 
-- in the month of Nov-2022:
```sql
SELECT * 
FROM retail_sales
WHERE 
		category='Clothing'
	AND quantity>=4
	AND TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
```

--Write a SQL query to calculate the total sales (total_sale) for each category.:
```sql
SELECT 
    category,
	SUM(total_sale) as net_sale,
    COUNT(*) as total_orders
FROM retail_sales
GROUP BY 1

```

--Write a SQL query to find the average age of customers who purchased items 
--from the 'Beauty' category
```sql
SELECT 
	ROUND(AVG(age),2) as average_age
FROM retail_sales
WHERE category = 'Beauty'
```

--Write a SQL query to find all transactions where the total_sale is greater than 1000
```sql
SELECT * 
FROM retail_sales
WHERE total_sale>1000
```

--Write a SQL query to find the total number of transactions (transaction_id) 
--made by each gender in each category
```sql
SELECT category,gender,COUNT(*) as number_transactions
FROM retail_sales
GROUP BY category,gender
ORDER BY 1;
```
--Write a SQL query to calculate the average sale for each month. 
--Find out best selling month in each year:
```sql
SELECT 
	EXTRACT(YEAR FROM sale_date) as year,
	EXTRACT(MONTH FROM sale_date) as month,
	AVG(total_sale) as avg_sale
FROM retail_sales
GROUP BY 1,2
ORDER BY 1,2;
```



--Write a SQL query to find the top 5 customers based on the highest total sales
```sql

SELECT customer_id,
SUM(total_sale) AS total_sales
FROM retail_sales
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5
```


--Write a SQL query to find the number of unique customers who 
--purchased items from each category.

```sql

SELECT COUNT(DISTINCT customer_id) as customer_id,category
FROM retail_sales
GROUP BY category
```
--Write a SQL query to create each shift and number of orders 
--(Example Morning <12, Afternoon Between 12 & 17, Evening >17)
```sql

SELECT *,
	CASE
		WHEN EXTRACT(HOUR FROM sale_time)<12 THEN 'Morning'
		WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
		ELSE 'Evening'
	END as shift
FROM retail_sales

```


--Write a SQL query to create each the count of total number of orders per shift.

```sql

WITH hourly_sale
AS
(
SELECT *,
	CASE
		WHEN EXTRACT(HOUR FROM sale_time)<12 THEN 'Morning'
		WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
		ELSE 'Evening'
	END as shift
FROM retail_sales

)
SELECT 
SHIFT,
COUNT(*) as total_orders
FROM hourly_sale
GROUP BY shift

```

--Write a SQL query to create each percentage of total number of orders per shift.
```sql
WITH hourly_sale AS (
    SELECT *,
        CASE
            WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
            WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
            ELSE 'Evening'
        END AS shift
    FROM retail_sales
)
SELECT 
    shift,
    COUNT(*) AS total_orders,
    ROUND(100.0 * COUNT(*) / SUM(COUNT(*)) OVER(), 2) AS percentage_orders
FROM hourly_sale
GROUP BY shift;



SELECT * FROM retail_sales
--Write a SQL query to see the total sales for each gender

SELECT gender,
SUM(total_sale) AS total_sales
FROM retail_sales
GROUP BY gender

```

--Write a SQL query to see the total sales for each category
```sql

SELECT category,
SUM(total_sale) AS total_sales
FROM retail_sales
GROUP BY category
```

--Write a SQL query to see the total number of sales for each category
```sql

SELECT category,
COUNT(total_sale) AS total_sales
FROM retail_sales
GROUP BY category
```


--Write a SQL query to see the number of items sold for each category.
```sql

SELECT category,
SUM(quantity) as total_number_of_sales
FROM retail_sales
GROUP BY category
```

--Write a SQL query to see the average number of purchases for each category
```sql

SELECT category,
AVG(quantity) as average_number_of_sales,
STDDEV_POP(quantity) AS standard_deviation
FROM retail_sales
GROUP BY category
```

--Write a SQL query to categorize the ages
```sql

SELECT *,
	CASE
		WHEN age<20 THEN 'child'
		WHEN age BETWEEN 20 AND 25 THEN 'teen'
		WHEN age BETWEEN 26 AND 50 THEN 'adult'
		ELSE 'elder'
	END as ages
FROM retail_sales
```

--write a sql query to count the number of sales of each age category
```sql


WITH new_data
AS(

SELECT *,
	CASE
		WHEN age<20 THEN 'child'
		WHEN age BETWEEN 20 AND 25 THEN 'teen'
		WHEN age BETWEEN 26 AND 50 THEN 'adult'
		ELSE 'elder'
	END as ages
FROM retail_sales
)
SELECT ages,
SUM(quantity) as sale_number
FROM new_data
GROUP BY ages
```

--write the average number of sales for each category of age
```sql

WITH new_data
AS(

SELECT *,
	CASE
		WHEN age<20 THEN 'child'
		WHEN age BETWEEN 20 AND 25 THEN 'teen'
		WHEN age BETWEEN 26 AND 50 THEN 'adult'
		ELSE 'elder'
	END as ages
FROM retail_sales
)
SELECT ages,
AVG(quantity) as sale_number
FROM new_data
GROUP BY ages
```

--Write a query to find the 5 highest buyers 
```sql

SELECT *
FROM retail_sales
ORDER BY total_sale DESC
LIMIT 5
```
## Findings
- **Customer Demographics and Sales insights**: Checked the demographics of the purchasing customers. Saw where they're purchasing power went to based on they're profile. Also when customers made purchases.
## Conclusion

This project serves as a introduction to SQL basic database setup, data cleaning, and exploratory data analysis. Helping business get a better understanding of customer purchasing power and behavior.


















