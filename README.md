SQL Customer & Product Analytics — Complete Project
This project contains a full SQL analytics assignment based on a fictional e-commerce database.
It demonstrates data exploration, filtering, aggregation, ordering, and multi-table JOIN operations across several datasets:
customers
products
orders
order_items
The assignment consists of 10 progressively more complex SQL tasks, each focused on a real-world business requirement: marketing, sales, shipping operations, and executive reporting.

All queries and results are included below

📘 Assignment Tasks & Solutions

1. Customer Data Exploration
Goal: Retrieve all customer information to understand the entire customer dataset.
SELECT * FROM customers;

Results:
| customer_id | first_name | last_name | email                                                   | city        | state | is_premium |
| ----------- | ---------- | --------- | ------------------------------------------------------- | ----------- | ----- | ---------- |
| 1           | John       | Smith     | [john.smith@email.com](mailto:john.smith@email.com)     | New York    | NY    | true       |
| 2           | Sarah      | Johnson   | [sarah.j@email.com](mailto:sarah.j@email.com)           | Los Angeles | CA    | false      |
| 3           | Mike       | Davis     | [mike.davis@email.com](mailto:mike.davis@email.com)     | Chicago     | IL    | true       |
| 4           | Emily      | Brown     | [emily.brown@email.com](mailto:emily.brown@email.com)   | Houston     | TX    | false      |
| 5           | David      | Wilson    | [david.wilson@email.com](mailto:david.wilson@email.com) | Phoenix     | AZ    | true       |

2. Targeted Customer Information for Marketing
Goal: Marketing needs names and emails only.
SELECT first_name, last_name, email FROM customers;

Results:
| first_name | last_name | email                                                   |
| ---------- | --------- | ------------------------------------------------------- |
| John       | Smith     | [john.smith@email.com](mailto:john.smith@email.com)     |
| Sarah      | Johnson   | [sarah.j@email.com](mailto:sarah.j@email.com)           |
| Mike       | Davis     | [mike.davis@email.com](mailto:mike.davis@email.com)     |
| Emily      | Brown     | [emily.brown@email.com](mailto:emily.brown@email.com)   |
| David      | Wilson    | [david.wilson@email.com](mailto:david.wilson@email.com) |

3. Shipping Issue Investigation — California Customers
Goal: Identify customers located in California (state = 'CA').
SELECT * FROM customers WHERE state = 'CA';

Results:
| customer_id | first_name | last_name | email                                         | city        | state | is_premium |
| ----------- | ---------- | --------- | --------------------------------------------- | ----------- | ----- | ---------- |
| 2           | Sarah      | Johnson   | [sarah.j@email.com](mailto:sarah.j@email.com) | Los Angeles | CA    | false      |

4. Premium Customer Identification
Goal: Find all premium customers for targeted promotions
SELECT first_name, last_name, email FROM customers WHERE is_premium = TRUE;

Results:
| first_name | last_name | email                                                   |
| ---------- | --------- | ------------------------------------------------------- |
| John       | Smith     | [john.smith@email.com](mailto:john.smith@email.com)     |
| Mike       | Davis     | [mike.davis@email.com](mailto:mike.davis@email.com)     |
| David      | Wilson    | [david.wilson@email.com](mailto:david.wilson@email.com) |

5. Product Price Analysis — Sort by Highest Price
Goal: Display all products ordered from highest to lowest price
SELECT * FROM products ORDER BY price DESC;

Results:
| product_id | product_name  | category      | price   | cost   | stock_quantity | supplier_id | created_date |
| ---------- | ------------- | ------------- | ------- | ------ | -------------- | ----------- | ------------ |
| 1          | Laptop Pro    | Electronics   | 1299.99 | 800.00 | 50             | 1           | 2023-01-01   |
| 5          | Smartphone    | Electronics   | 699.99  | 400.00 | 100            | 4           | 2023-03-01   |
| 4          | Desk Chair    | Furniture     | 199.99  | 120.00 | 30             | 1           | 2023-02-15   |
| 3          | Running Shoes | Sports        | 89.99   | 45.00  | 75             | 3           | 2023-02-01   |
| 2          | Coffee Mug    | Home & Garden | 12.99   | 5.00   | 200            | 2           | 2023-01-15   |

6. Top 3 Most Expensive Products
Goal: Prepare a summary of the 3 most expensive products.
SELECT product_name, price 
FROM products 
ORDER BY price DESC 
LIMIT 3;

Results:
| product_name | price   |
| ------------ | ------- |
| Laptop Pro   | 1299.99 |
| Smartphone   | 699.99  |
| Desk Chair   | 199.99  |

7. Product Catalog Metrics
Goal: Calculate total number of products and average price
SELECT COUNT(*) AS total_products, AVG(price) AS avg_price FROM products;

Results:
| total_products | avg_price |
| -------------- | --------- |
| 5              | 460.59    |

8. Performance by Product Category
Goal: Show number of products and average price per category, sorted by highest avg price.
SELECT 
    category, 
    COUNT(*) AS product_count, 
    AVG(price) AS avg_price 
FROM products 
GROUP BY category 
ORDER BY avg_price DESC;

Results:
| category      | product_count | avg_price |
| ------------- | ------------- | --------- |
| Electronics   | 2             | 999.99    |
| Furniture     | 1             | 199.99    |
| Sports        | 1             | 89.99     |
| Home & Garden | 1             | 12.99     |

9. Customer Orders Summary — INNER JOIN
Goal: Link customer data with order data to produce a consolidated view.
SELECT 
    first_name, 
    last_name, 
    order_id, 
    order_date, 
    total_amount
FROM customers
INNER JOIN orders
ON customers.customer_id = orders.customer_id;

Results:
| first_name | last_name | order_id | order_date | total_amount |
| ---------- | --------- | -------- | ---------- | ------------ |
| John       | Smith     | 1        | 2024-01-15 | 1299.99      |
| Sarah      | Johnson   | 2        | 2024-01-16 | 25.98        |
| John       | Smith     | 3        | 2024-01-17 | 89.99        |
| Mike       | Davis     | 4        | 2024-01-18 | 199.99       |
| Emily      | Brown     | 5        | 2024-01-19 | 699.99       |

10. Final Executive Report — Multi-Table JOIN
Goal: Create a fully detailed report combining customer, order, order item, and product data
SELECT 
    CONCAT(c.first_name, ' ', c.last_name) AS customer_name,
    p.product_name,
    o.order_date,
    oi.quantity,
    o.total_amount
FROM customers AS c
JOIN orders AS o ON c.customer_id = o.customer_id
JOIN order_items AS oi ON o.order_id = oi.order_id
JOIN products AS p ON oi.product_id = p.product_id;

| customer_name | product_name  | order_date | quantity | total_amount |
| ------------- | ------------- | ---------- | -------- | ------------ |
| John Smith    | Laptop Pro    | 2024-01-15 | 1        | 1299.99      |
| Sarah Johnson | Coffee Mug    | 2024-01-16 | 2        | 25.98        |
| John Smith    | Running Shoes | 2024-01-17 | 1        | 89.99        |
| Mike Davis    | Desk Chair    | 2024-01-18 | 1        | 199.99       |
| Emily Brown   | Smartphone    | 2024-01-19 | 1        | 699.99       |
