# Retail Sales Analysis — SQL Project

## 📌 Project Overview

**Retail Sales Analysis** is a SQL-based data analytics project focused on analyzing retail transaction data and answering business questions using PostgreSQL.

The project covers:

- Database and table creation
- Data cleaning
- Null-value identification and removal
- Exploratory Data Analysis (EDA)
- Customer analysis
- Category-level sales analysis
- Transaction analysis
- Monthly sales analysis
- Customer spending analysis
- Time-based sales analysis

**Database:** `sales_analysis_project`  
**Table:** `retail_sales`  
**Database:** PostgreSQL  
**Language:** SQL

---

## 🎯 Project Objectives

The main objectives of this project are to:

1. Create a retail sales database and table.
2. Check and clean missing data.
3. Explore customers, sales, and product categories.
4. Analyze retail transactions using SQL.
5. Answer business-related questions using SQL queries.
6. Identify customer, category, sales, and time-based patterns.

---

## 🗂️ Dataset Structure

The `retail_sales` table contains the following columns:

| Column | Description |
|---|---|
| `transactions_id` | Unique transaction identifier |
| `sale_date` | Date of sale |
| `sale_time` | Time of sale |
| `customer_id` | Unique customer identifier |
| `gender` | Customer gender |
| `age` | Customer age |
| `category` | Product category |
| `quantity` | Quantity purchased |
| `price_per_unit` | Price of one unit |
| `cogs` | Cost of goods sold |
| `total_sale` | Total sale amount |

---

# 🛠️ Database Setup

## Create Table

```sql
DROP TABLE IF EXISTS retail_sales;

CREATE TABLE retail_sales (
    transactions_id INT PRIMARY KEY,
    sale_date DATE,
    sale_time TIME,
    customer_id INT,
    gender VARCHAR(10),
    age INT,
    category VARCHAR(15),
    quantity INT,
    price_per_unit FLOAT,
    cogs FLOAT,
    total_sale FLOAT
);
```

---

# 🧹 Data Cleaning

## Check for NULL Values

The project checks important columns for missing values.

```sql
SELECT *
FROM retail_sales
WHERE
    transactions_id IS NULL
    OR sale_date IS NULL
    OR sale_time IS NULL
    OR customer_id IS NULL
    OR gender IS NULL
    OR age IS NULL
    OR category IS NULL
    OR quantity IS NULL
    OR price_per_unit IS NULL
    OR cogs IS NULL
    OR total_sale IS NULL;
```

## Delete Records with NULL Values

```sql
DELETE FROM retail_sales
WHERE
    transactions_id IS NULL
    OR sale_date IS NULL
    OR sale_time IS NULL
    OR customer_id IS NULL
    OR gender IS NULL
    OR age IS NULL
    OR category IS NULL
    OR quantity IS NULL
    OR price_per_unit IS NULL
    OR cogs IS NULL
    OR total_sale IS NULL;
```

---

# 🔎 Exploratory Data Analysis

## Total Number of Sales

```sql
SELECT COUNT(*)
FROM retail_sales;
```

## Number of Unique Customers

```sql
SELECT COUNT(DISTINCT customer_id)
FROM retail_sales;
```

## Available Product Categories

```sql
SELECT DISTINCT category
FROM retail_sales;
```

---

# 📊 Business Analysis

## Q1. Sales Made on November 5, 2022

```sql
SELECT *
FROM retail_sales
WHERE sale_date = '2022-11-05';
```

**Purpose:** Retrieve all transactions made on a specific date.

---

## Q2. Clothing Transactions in November 2022

```sql
SELECT *
FROM retail_sales
WHERE
    category = 'Clothing'
    AND TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
    AND quantity >= 4;
```

**Purpose:** Identify Clothing transactions with at least 4 units sold during November 2022.

---

## Q3. Total Sales by Category

```sql
SELECT
    category,
    SUM(total_sale) AS total_sales
FROM retail_sales
GROUP BY category
ORDER BY total_sales DESC;
```

**Purpose:** Compare total sales performance across product categories.

---

## Q4. Average Age of Beauty Customers

```sql
SELECT
    ROUND(AVG(age), 2) AS avg_age
FROM retail_sales
WHERE category = 'Beauty';
```

**Purpose:** Understand the average age of customers purchasing Beauty products.

---

## Q5. High-Value Transactions

```sql
SELECT *
FROM retail_sales
WHERE total_sale > 1000;
```

**Purpose:** Identify transactions with a total sale value above 1,000.

---

## Q6. Transactions by Gender and Category

```sql
SELECT
    category,
    gender,
    COUNT(transactions_id) AS total_no_trans
FROM retail_sales
GROUP BY category, gender
ORDER BY category;
```

**Purpose:** Analyze transaction volume across gender and product categories.

---

## Q7. Best-Selling Month in Each Year

```sql
WITH monthly_sales AS (
    SELECT
        EXTRACT(YEAR FROM sale_date) AS year,
        EXTRACT(MONTH FROM sale_date) AS month,
        AVG(total_sale) AS avg_sale,
        RANK() OVER (
            PARTITION BY EXTRACT(YEAR FROM sale_date)
            ORDER BY AVG(total_sale) DESC
        ) AS rank
    FROM retail_sales
    GROUP BY
        EXTRACT(YEAR FROM sale_date),
        EXTRACT(MONTH FROM sale_date)
)
SELECT
    year,
    month,
    avg_sale
FROM monthly_sales
WHERE rank = 1;
```

**Purpose:** Identify the month with the highest average sale for each year.

**SQL concepts used:**
- `AVG()`
- `EXTRACT()`
- `GROUP BY`
- `RANK()`
- Window Functions
- CTE

---

## Q8. Top 5 Customers by Total Sales

```sql
SELECT
    customer_id,
    SUM(total_sale) AS total_sales
FROM retail_sales
GROUP BY customer_id
ORDER BY total_sales DESC
LIMIT 5;
```

**Purpose:** Identify the five customers generating the highest total sales.

---

## Q9. Unique Customers by Category

```sql
SELECT
    category,
    COUNT(DISTINCT customer_id) AS unique_customers
FROM retail_sales
GROUP BY category;
```

**Purpose:** Determine the number of unique customers purchasing from each category.

---

## Q10. Sales by Time Shift

Transactions are divided into three shifts:

- **Morning:** Before 12:00
- **Afternoon:** 12:00–17:00
- **Evening:** After 17:00

```sql
WITH hourly_shift AS (
    SELECT *,
        CASE
            WHEN EXTRACT(HOUR FROM sale_time) < 12
                THEN 'Morning'
            WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17
                THEN 'Afternoon'
            ELSE 'Evening'
        END AS shift
    FROM retail_sales
)
SELECT
    shift,
    COUNT(*) AS total_orders
FROM hourly_shift
GROUP BY shift;
```

**Purpose:** Understand when most retail transactions occur during the day.

---

# 📈 Analysis Areas

### Sales Analysis
- Total sales by category
- High-value transactions
- Monthly sales trends
- Average sales

### Customer Analysis
- Unique customers
- Top 5 customers
- Customer age
- Gender-based transactions
- Customers by category

### Product Analysis
- Product categories
- Category-level sales
- Clothing transactions
- Beauty customer analysis

### Time Analysis
- Sales by date
- Monthly sales
- Morning, Afternoon, and Evening transactions

---

# 🧠 SQL Skills Demonstrated

This project demonstrates practical SQL skills including:

- `SELECT`
- `WHERE`
- `DISTINCT`
- `COUNT()`
- `COUNT(DISTINCT)`
- `SUM()`
- `AVG()`
- `ROUND()`
- `GROUP BY`
- `ORDER BY`
- `LIMIT`
- `CASE`
- `TO_CHAR()`
- `EXTRACT()`
- CTEs
- Window Functions
- `RANK()`
- Data Cleaning
- Exploratory Data Analysis
- Business Question Analysis

---

# 💡 Business Questions Answered

This project answers questions such as:

1. What sales occurred on a specific date?
2. Which Clothing transactions had high quantities?
3. Which product categories generate the highest sales?
4. What is the average age of Beauty customers?
5. Which transactions have a value above 1,000?
6. How do transactions vary by gender and category?
7. Which month performs best in each year?
8. Who are the top 5 customers by total sales?
9. How many unique customers purchase from each category?
10. Which time shift has the highest number of orders?

---

# 📌 Project Highlights

- Built a retail sales database using PostgreSQL.
- Performed data-quality checks and removed records containing NULL values.
- Conducted exploratory analysis of customers, categories, and transactions.
- Developed **10 business-focused SQL queries**.
- Used aggregation functions to analyze sales and customer behavior.
- Applied CTEs and the `RANK()` window function for monthly analysis.
- Analyzed customer spending, product categories, and transaction time shifts.

---
