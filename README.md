# Retail-Sales-Analysis-SQL
**Retail Sales Analysis (SQL Project)**  This project analyzes a retail sales dataset using SQL to generate business insights. It includes data cleaning, exploration, and queries to identify sales trends, top customers, category performance, and best-selling months using aggregation, window functions, and CASE statements.
# Retail Sales Analysis – SQL Project

## **Project Overview**
**Project Title:** Retail Sales Analysis  
**Level:** Beginner  
**Database:** retail_sales  

This project demonstrates SQL skills used in data analysis by exploring and analyzing a retail sales dataset. The project includes **database creation, data cleaning, exploratory data analysis (EDA), and business problem solving using SQL queries**. It helps understand **sales trends, customer behavior, and product performance**.

---

# **Objectives**

- **Set up a retail sales database**
- **Clean the dataset by identifying and removing null values**
- **Perform exploratory data analysis**
- **Answer important business questions using SQL**

---

# **Project Structure**

## **1. Database Setup**

### **Table Creation**

```sql
DROP TABLE IF EXISTS retail_sales;

CREATE TABLE retail_sales
(
    transaction_id INT PRIMARY KEY,
    sale_date DATE,
    sale_time TIME,
    customer_id INT,
    gender VARCHAR(15),
    age INT,
    category VARCHAR(15),
    quantiy INT,
    price_per_unit FLOAT,
    cogs FLOAT,
    total_sale FLOAT
);
```

---

## **2. Data Import**

```sql
COPY retail_sales(
    transaction_id,
    sale_date,
    sale_time,
    customer_id,
    gender,
    age,
    category,
    quantiy,
    price_per_unit,
    cogs,
    total_sale
)
FROM 'D:\SQL\SQL - Retail Sales Analysis_utf .csv'
CSV HEADER;
```

---

# **3. Data Cleaning**

### **Check for Missing Values**

```sql
SELECT *
FROM retail_sales
WHERE 
    transaction_id IS NULL
    OR sale_date IS NULL
    OR sale_time IS NULL
    OR gender IS NULL
    OR category IS NULL
    OR quantiy IS NULL
    OR cogs IS NULL
    OR total_sale IS NULL;
```

### **Remove Records with Null Values**

```sql
DELETE FROM retail_sales
WHERE 
    transaction_id IS NULL
    OR sale_date IS NULL
    OR sale_time IS NULL
    OR gender IS NULL
    OR category IS NULL
    OR quantiy IS NULL
    OR cogs IS NULL
    OR total_sale IS NULL;
```

---

# **4. Data Exploration**

### **Total Number of Sales**

```sql
SELECT COUNT(*) AS total_sales
FROM retail_sales;
```

### **Number of Unique Customers**

```sql
SELECT COUNT(DISTINCT customer_id) AS unique_customers
FROM retail_sales;
```

---

# **5. Data Analysis & Business Questions**

### **1. Sales on a Specific Date**

```sql
SELECT *
FROM retail_sales
WHERE sale_date = '2022-11-05';
```

---

### **2. Clothing Transactions in November 2022**

```sql
SELECT *
FROM retail_sales
WHERE category = 'Clothing'
AND quantiy >= 4
AND sale_date BETWEEN '2022-11-01' AND '2022-11-30';
```

---

### **3. Total Sales by Category**

```sql
SELECT 
    category,
    SUM(total_sale) AS net_sales
FROM retail_sales
GROUP BY category;
```

---

### **4. Average Age of Beauty Category Customers**

```sql
SELECT 
    ROUND(AVG(age),2) AS avg_age
FROM retail_sales
WHERE category = 'Beauty';
```

---

### **5. Transactions with Sales Greater Than 1000**

```sql
SELECT *
FROM retail_sales
WHERE total_sale > 1000;
```

---

### **6. Transactions by Gender and Category**

```sql
SELECT 
    gender,
    category,
    COUNT(transaction_id) AS total_transactions
FROM retail_sales
GROUP BY gender, category;
```

---

### **7. Best Selling Month in Each Year**

```sql
SELECT 
    Years,
    Months,
    avg_sales
FROM
(
SELECT
    EXTRACT(YEAR FROM sale_date) AS Years,
    EXTRACT(MONTH FROM sale_date) AS Months,
    AVG(total_sale) AS avg_sales,
    RANK() OVER(
        PARTITION BY EXTRACT(YEAR FROM sale_date)
        ORDER BY AVG(total_sale) DESC
    ) AS ranks
FROM retail_sales
GROUP BY Years, Months
) AS T1
WHERE ranks = 1;
```

---

### **8. Top 5 Customers by Total Sales**

```sql
SELECT
    customer_id,
    SUM(total_sale) AS total_sales
FROM retail_sales
GROUP BY customer_id
ORDER BY total_sales DESC
LIMIT 5;
```

---

### **9. Unique Customers per Category**

```sql
SELECT
    category,
    COUNT(DISTINCT customer_id) AS unique_customers
FROM retail_sales
GROUP BY category;
```

---

### **10. Sales by Time Shift**

Morning: Before 12 PM  
Afternoon: Between 12 PM and 5 PM  
Evening: After 5 PM  

```sql
SELECT
    CASE
        WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
        WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
        ELSE 'Evening'
    END AS shift,
    COUNT(transaction_id) AS total_orders
FROM retail_sales
GROUP BY shift;
```

---

# **Key Findings**

- Sales are distributed across categories such as **Clothing and Beauty**.
- Some transactions show **high-value purchases above 1000**.
- **Monthly analysis identifies peak sales months**.
- Customer segmentation by **gender and category** provides insights for marketing strategies.
- **Top customers contribute significantly to overall revenue**.

---

# **Conclusion**

This project demonstrates **practical SQL skills used in real-world data analysis**, including database setup, data cleaning, exploratory analysis, and solving business problems using SQL queries. The insights generated can help businesses understand **sales trends, customer behavior, and product performance**.

---

# **How to Use**

1. Clone this repository.
2. Run the **table creation script**.
3. Import the **CSV dataset into the table**.
4. Execute the **analysis queries**.
5. Modify queries to explore additional insights.

---

# **Author**

**Vasvi Kaushik**
