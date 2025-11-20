🍕 Great Pizza Analytics — SQL Mini Project
By: Ankit Tripathi

IDC 21 Days of SQL Challenge — Indian Data Club | Sponsored by DPDzero

📌 Project Overview

The Great Pizza Analytics mini project is part of the IDC 21 Days SQL Challenge, designed to apply practical SQL skills across real datasets.
This project analyzes pizza menu data, pricing, customer orders, and sales patterns using SQL queries across three phases:

Phase 1: Foundation & Data Inspection

Phase 2: Filtering & Exploration

Phase 3: Sales Performance & Business Insights

The dataset is divided into four CSV files, which were imported into a SQL database for analysis.

📂 Dataset Used (CSV Files)

You can find all CSVs in the /data folder:

File Name	Description
pizza_types.csv	Contains pizza type, category, and ingredients
pizzas.csv	Contains pizza ID, size, price, and type link
orders.csv	Contains order ID, order date & time
order_details.csv	Contains order-level pizza quantities
🏗️ Database Schema
pizza_types      (pizza_type_id, name, category, ingredients)
pizzas           (pizza_id, pizza_type_id, size, price)
orders           (order_id, order_date, order_time)
order_details    (order_details_id, order_id, pizza_id, quantity)


Relationships:

pizza_types ←→ pizzas (1-to-many)

pizzas ←→ order_details

orders ←→ order_details

🔎 PHASE 1 — Foundation & Inspection
✔ 1. List all unique pizza categories
SELECT DISTINCT category 
FROM pizza_types;

✔ 2. Display pizza types & replace NULL ingredients

(shows first 5 rows)

SELECT 
    pizza_type_id,
    name,
    COALESCE(ingredients, 'Missing Data') AS ingredients
FROM pizza_types
ORDER BY pizza_type_id
LIMIT 5;

✔ 3. Find pizzas with missing price
SELECT *
FROM pizzas
WHERE price IS NULL;

🧪 PHASE 2 — Filtering & Exploration
✔ 1. Orders from a specific date
SELECT *
FROM orders
WHERE order_date = '2015-01-01';

✔ 2. List pizzas by price descending
SELECT pizza_id, price, size
FROM pizzas
ORDER BY price DESC;

✔ 3. Pizzas sold in sizes L or XL
SELECT pizza_id, size
FROM pizzas
WHERE size IN ('L', 'XL');

✔ 4. Pizzas priced between $15 and $17
SELECT pizza_id, price
FROM pizzas
WHERE price BETWEEN 15 AND 17;

✔ 5. Pizzas with "Chicken" in the name
SELECT name
FROM pizza_types
WHERE name LIKE '%chicken%';

✔ 6. Orders on Feb 15 OR placed after 8 PM
SELECT *
FROM orders
WHERE order_date = '2015-02-15'
   OR HOUR(order_time) > 20;

📈 PHASE 3 — Sales Performance & Insights
✔ 1. Total pizzas sold
SELECT SUM(quantity) AS total_sales
FROM order_details;

✔ 2. Average pizza price
SELECT ROUND(AVG(price), 2) AS avg_price
FROM pizzas;

✔ 3. Total order value per order (JOIN + GROUP BY)
SELECT 
    od.order_id,
    SUM(od.quantity * p.price) AS Total_order_value
FROM pizzas p
JOIN order_details od ON p.pizza_id = od.pizza_id
GROUP BY od.order_id
ORDER BY Total_order_value DESC;

✔ 4. Quantity sold per pizza category
SELECT pt.category,
       SUM(o.quantity) AS Total_Qty_sold
FROM pizza_types pt
JOIN pizzas p ON p.pizza_type_id = pt.pizza_type_id
JOIN order_details o ON p.pizza_id = o.pizza_id
GROUP BY pt.category
ORDER BY Total_Qty_sold DESC;

✔ 5. Categories with > 5000 pizzas sold
SELECT pt.category,
       SUM(o.quantity) AS Total_Qty_sold
FROM pizza_types pt
JOIN pizzas p ON p.pizza_type_id = pt.pizza_type_id
JOIN order_details o ON p.pizza_id = o.pizza_id
GROUP BY pt.category
HAVING SUM(o.quantity) > 5000;

✔ 6. Pizzas never ordered
SELECT p.pizza_id, p.size, pt.category, od.order_id
FROM pizzas p
LEFT JOIN order_details od ON p.pizza_id = od.pizza_id
LEFT JOIN pizza_types pt ON pt.pizza_type_id = p.pizza_type_id
WHERE order_id IS NULL;

✔ 7. Price differences between sizes (Self Join)
SELECT 
    p1.pizza_type_id,
    p1.size AS size_1,
    p1.price AS price_1,
    p2.size AS size_2,
    p2.price AS price_2,
    (p2.price - p1.price) AS price_difference
FROM pizzas p1
JOIN pizzas p2
  ON p1.pizza_type_id = p2.pizza_type_id
 AND p1.size != p2.size
ORDER BY p1.pizza_type_id;

📊 Key Insights Derived

✔ Identified unique pizza categories & cleaned ingredient data
✔ Found pizzas with missing values for quality checks
✔ Analyzed order patterns across specific dates & times
✔ Identified top-selling & underperforming categories
✔ Calculated order-level revenue using joins
✔ Found pizzas never ordered → useful for menu optimization
✔ Compared size-based pricing differences using self joins
✔ Built a complete end-to-end multi-table analytics workflow

🗂️ Project Structure (Recommended GitHub Layout)
Pizza-Analytics-SQL-Project/
│
├── data/
│   ├── pizza_types.csv
│   ├── pizzas.csv
│   ├── orders.csv
│   ├── order_details.csv
│
├── sql/
│   └── pizza_analysis.sql
│
├── presentation/
│   └── Great_Pizza_Analytics_Ankit_Tripathi.pptx
│
└── README.md

🙌 Acknowledgements

This project is part of the IDC 21 Days SQL Challenge
Organized by: Indian Data Club
Sponsored by: DPDzero
