# Assignment 5 — Functions, Aggregates, Subqueries, GROUP BY, Views, Set Operations, TCL

## Title
Functional Single Row, Aggregate Functions, Data Sorting, Subqueries, GROUP BY HAVING, Set Operations, VIEW, TCL

## Aim
Write SELECT commands using single-row functions, aggregate functions, subqueries, GROUP BY/HAVING, set operations, views, and TCL commands.

---

## Database (Batch A)
**E-Commerce "Global Groceries"**

Tables: `customers`, `orders`, `order_items`, `products`, `employees`

---

## Theory

### Single-Row vs Multi-Row Functions
- **Single-row functions**: Operate on one row at a time and return one result per row (e.g., `UPPER`, `SUBSTR`, `ROUND`, `CURDATE`).
- **Multi-row (Aggregate) functions**: Operate on a set of rows and return a single result (e.g., `SUM`, `AVG`, `COUNT`, `MAX`, `MIN`).

### WHERE vs HAVING
| Clause | Purpose |
|--------|---------|
| `WHERE` | Filters rows **before** grouping; cannot use aggregate functions |
| `HAVING` | Filters groups **after** `GROUP BY`; can use aggregate functions |

### Set Operations
| Operation | Description |
|-----------|-------------|
| `UNION` | Combines results of two queries; removes duplicates |
| `UNION ALL` | Combines results; keeps duplicates |
| `INTERSECT` | Returns rows common to both queries |
| `EXCEPT` | Returns rows in first query but not in second |

### VIEW
A VIEW is a **virtual table** based on the result of a SELECT statement. It does not store data itself.
```sql
CREATE VIEW view_name AS
SELECT ...;

-- Query a view like a table
SELECT * FROM view_name;

-- Drop a view
DROP VIEW view_name;
```
DML operations (INSERT, UPDATE, DELETE) are allowed on **simple views** (single table, no GROUP BY, no aggregate functions).

### TCL Commands
| Command | Description |
|---------|-------------|
| `START TRANSACTION` | Begins a transaction |
| `COMMIT` | Permanently saves all changes made in the transaction |
| `ROLLBACK` | Undoes all changes made since the transaction began |
| `SAVEPOINT name` | Creates a named checkpoint within a transaction |
| `ROLLBACK TO SAVEPOINT name` | Rolls back to the named savepoint, preserving changes before it |

---

## Exercises (Batch A — 6 Queries)

### Query 1: Single-Row Functions + Sorting
- Retrieve 2024 customers with capitalized names using `UPPER` and `SUBSTR`
- Calculate membership months using `TIMESTAMPDIFF`
- Mask email addresses using `SUBSTR` and `INSTR`
- Sort results by `join_date ASC`

### Query 2: Aggregate + GROUP BY + HAVING
- Calculate total revenue, average quantity, and unique orders per product category
- Use `GROUP BY` on category
- Filter with `HAVING` to show only categories with revenue > $1,000,000

### Query 3: Subqueries
- Find employees who processed orders for customers whose total spending exceeds the average customer spend
- Use a subquery in the `WHERE` clause

### Query 4: Set Operations (UNION)
- Combine: employees with >5 years of service **UNION** customers with 10+ orders
- Use `UNION` (not `UNION ALL`) to remove duplicates

### Query 5: VIEW
```sql
CREATE VIEW v_top_customer_revenue AS
SELECT
    CONCAT(first_name, ' ', last_name) AS full_name,
    email,
    SUM(total_amount) AS lifetime_spending
FROM customers
JOIN orders ON customers.customer_id = orders.customer_id
GROUP BY customers.customer_id, full_name, email
ORDER BY lifetime_spending DESC
LIMIT 10;
```

### Query 6: TCL Commands
```sql
START TRANSACTION;

-- First update
UPDATE products SET price = price * 1.10 WHERE category = 'Electronics';

SAVEPOINT after_first_update;

-- Erroneous update
UPDATE products SET price = 0 WHERE category = 'Groceries';

-- Undo only the erroneous update
ROLLBACK TO SAVEPOINT after_first_update;

-- Correct update
UPDATE products SET stock = stock - 5 WHERE product_id = 101;

COMMIT;
```

---
### Implementation
#### E-commerce Analytics Project
An e-commerce company, "Global Groceries," wants to perform a detailed analysis
of its sales data to improve marketing strategies and customer service. They have
a database with the following tables:
● customers: customer_id, first_name, last_name, email, join_date
● orders: order_id, customer_id, order_date, total_amount
● order_items: item_id, order_id, product_id, quantity
● products: product_id, product_name, category, price
● employees: employee_id, first_name, last_name, hire_date, department


#### 1. Customer & Employee Analysis (Functional Single Row & Data Sorting)
The marketing team needs a report of all customers who joined in 2024. The report should display the following for each customer:
● Their full name, with the first letter of each name capitalized.
● A calculated "membership duration" in months since they joined.
● The first three characters of their email address, followed by ... and the domain (e.g., joh...@example.com). The list should be sorted in
ascending order of their join date.

##### 1. Creating Database 
<img width="720" height="813" alt="image" src="https://github.com/user-attachments/assets/a87a34d5-fda9-4b02-bdc6-00cf90a27a06" />
<img width="646" height="370" alt="image" src="https://github.com/user-attachments/assets/c2f60771-f0ed-44c1-893d-c9823ac3cb16" />

#### 2. Inserting values and displaying
<img width="813" height="965" alt="image" src="https://github.com/user-attachments/assets/b48f4dab-12b7-4033-8af4-704f964d97b8" />
<img width="847" height="188" alt="image" src="https://github.com/user-attachments/assets/5c5d9ff4-9a02-46b0-9a1b-b6b7ef39c138" />

#### 3. Display 
<img width="773" height="540" alt="image" src="https://github.com/user-attachments/assets/301c7e67-6821-4f48-828c-c8ecbc5d0e40" />

#### 4. Queries
1. Customer & Employee Analysis (Functional Single Row & Data Sorting)
The marketing team needs a report of all customers who joined in 2024. The report should display the following for each customer:
● Their full name, with the first letter of each name capitalized.

● A calculated "membership duration" in months since they joined.

● The first three characters of their email address, followed by ... and the domain (e.g., joh...@example.com). The list should be sorted in
ascending order of their join date.
<img width="767" height="461" alt="image" src="https://github.com/user-attachments/assets/d63edd3c-ad6e-4bac-be2b-f52cba52f826" />


2. Sales Aggregation & Filtering (Aggregate Functions, GROUP BY, HAVING)
The sales team wants to identify high-performing product categories. They need a summary report that calculates the total revenue, average
quantity sold per order, and the number of unique orders for each product category. The report should only include categories that have
generated a total revenue of more than $1,000,000.

<img width="614" height="344" alt="image" src="https://github.com/user-attachments/assets/876559f1-908b-47ed-8d3a-e18928ec1d08" />


3. Employee Performance (Subqueries)
The HR department wants to reward employees who are top performers. An employee is considered a top performer if they have processed
orders for at least one customer whose total spending is above the average total spending of all customers. The task is to list the full names
and hire dates of all top-performing employees.
<img width="729" height="904" alt="image" src="https://github.com/user-attachments/assets/a112c875-fa09-4556-b72d-cf0973f06ec8" />

4. Data Consolidation (Set Operations)
The management team wants to cross-reference data from different sources. They need a single list that combines the first and last names of:
● All employees who have worked for the company for more than 5 years.
● All customers who have placed at least 10 orders. The list should not contain any duplicate names.
<img width="752" height="496" alt="image" src="https://github.com/user-attachments/assets/7a987a80-ce48-4297-87a8-f2f283ea2d86" />

5. Simplified Access (View)
To make future reporting easier, the business intelligence team wants to create a virtual table. This table, named
v_top_customer_revenue, should show the top 10 customers by their total spending. It should include the customer's
full name, email, and their total lifetime spending.
<img width="1860" height="851" alt="image" src="https://github.com/user-attachments/assets/5ab895ed-40b8-4776-8a1e-d2b66d409d2d" />


6. Data Integrity & Control (TCL Commands)
The operations team is performing a critical update. A new system has changed some customer IDs, and they need
to update them in the database. The team wants to handle this process as a transaction to ensure data integrity.
● Start a new transaction.
● Update the customer_id for two specific customers. For example, change customer_id 101 to 201 and 102 to
202.
● After the first update (101 to 201), create a savepoint named after_first_update.
● Mistakenly, you update customer_id 103 to 201 (which is incorrect and a duplicate of the previous update).
● Rollback the transaction to the after_first_update savepoint to undo only the erroneous change.
● Correctly update customer_id 103 to 203.
● Commit the transaction to finalize all correct changes.

<img width="719" height="1005" alt="image" src="https://github.com/user-attachments/assets/b1faaf26-7754-40ac-af0f-edb9d1ecd05b" />
<img width="600" height="379" alt="image" src="https://github.com/user-attachments/assets/5ce5155a-1218-4c5a-88a6-661fce39df0a" />



## SQL :

```sql
mahek@mahek-ZenBook-UX325EA-UX325EA:~$ sudo mysql
[sudo] password for mahek: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 9
Server version: 8.0.45-0ubuntu0.22.04.1 (Ubuntu)

Copyright (c) 2000, 2026, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> CREATE DATABASE ecommerce_db;
Query OK, 1 row affected (0.01 sec)

mysql> USE ecommerce_db;
Database changed
mysql> CREATE TABLE customers (
    ->     customer_id INT          PRIMARY KEY,
    ->     first_name  VARCHAR(50)  NOT NULL,
    ->     last_name   VARCHAR(50)  NOT NULL,
    ->     email       VARCHAR(100) UNIQUE NOT NULL,
    ->     join_date   DATE         NOT NULL
    -> );
Query OK, 0 rows affected (0.01 sec)

mysql>  
mysql> CREATE TABLE products (
    ->     product_id   INT          PRIMARY KEY,
    ->     product_name VARCHAR(100) NOT NULL,
    ->     category     VARCHAR(50)  NOT NULL,
    ->     price        DECIMAL(10,2) NOT NULL
    -> );
Query OK, 0 rows affected (0.02 sec)

mysql>  
mysql> CREATE TABLE orders (
    ->     order_id     INT           PRIMARY KEY,
    ->     customer_id  INT           NOT NULL,
    ->     order_date   DATE          NOT NULL,
    ->     total_amount DECIMAL(10,2) NOT NULL,
    ->     FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
    -> );
Query OK, 0 rows affected (0.01 sec)

mysql>  
mysql> CREATE TABLE order_items (
    ->     item_id    INT NOT NULL PRIMARY KEY,
    ->     order_id   INT NOT NULL,
    ->     product_id INT NOT NULL,
    ->     quantity   INT NOT NULL,
    ->     FOREIGN KEY (order_id)   REFERENCES orders(order_id),
    ->     FOREIGN KEY (product_id) REFERENCES products(product_id)
    -> );
Query OK, 0 rows affected (0.01 sec)

mysql>  
mysql> CREATE TABLE employees (
    ->     employee_id INT         PRIMARY KEY,
    ->     first_name  VARCHAR(50) NOT NULL,
    ->     last_name   VARCHAR(50) NOT NULL,
    ->     hire_date   DATE        NOT NULL,
    ->     department  VARCHAR(50) NOT NULL
    -> );
Query OK, 0 rows affected (0.02 sec)

mysql> INSERT INTO customers VALUES
    ->     (1,  'Winnie',   'Pooh',        'winnie.thepooh@shop.com',   '2024-03-10'),
    ->     (2,  'Pinocchio','Puppet',      'pinocchio@shop.com',        '2024-06-20'),
    ->     (3,  'Tigger',   'Tiger',       'tiger@shop.com',            '2022-01-05'),
    ->     (4,  'Piglet',   'Friend',      'ananya@shop.com',           '2024-09-15'),
    ->     (5,  'Kanga',    'Mother',      'kiran@shop.com',            '2019-07-11'),
    ->     (6,  'Roo',      'Joey',        'pooja@shop.com',            '2018-03-22'),
    ->     (7,  'Aladdin',  'Prince',      'nikhil@shop.com',           '2024-12-01'),
    ->     (8,  'Elsa',     'Queen',       'tanvi@shop.com',            '2023-08-14');
Query OK, 8 rows affected (0.01 sec)
Records: 8  Duplicates: 0  Warnings: 0

mysql>  
mysql> INSERT INTO products VALUES
    ->     (1,  'Rice 5kg',       'Grains',       80.00),
    ->     (2,  'Wheat Flour 5kg','Grains',        55.00),
    ->     (3,  'Olive Oil 1L',   'Oils',         350.00),
    ->     (4,  'Sugar 1kg',      'Sweeteners',    45.00),
    ->     (5,  'Basmati Rice',   'Grains',       120.00),
    ->     (6,  'Sunflower Oil',  'Oils',         180.00),
    ->     (7,  'Honey 500g',     'Sweeteners',   220.00),
    ->     (8,  'Dal Masoor',     'Pulses',        90.00);
Query OK, 8 rows affected (0.01 sec)
Records: 8  Duplicates: 0  Warnings: 0

mysql>  
mysql> INSERT INTO orders VALUES
    ->     (101, 1, '2024-04-01', 5600.00),
    ->     (102, 2, '2024-07-10',12200.00),
    ->     (103, 3, '2024-05-20', 8900.00),
    ->     (104, 1, '2024-11-01', 3200.00),
    ->     (105, 5, '2024-06-15',18500.00),
    ->     (106, 6, '2024-09-05',22000.00),
    ->     (107, 3, '2024-10-12', 4100.00),
    ->     (108, 7, '2024-12-20', 6700.00),
    ->     (109, 8, '2024-08-03', 9300.00),
    ->     (110, 4, '2024-10-25', 3800.00);
Query OK, 10 rows affected (0.00 sec)
Records: 10  Duplicates: 0  Warnings: 0

mysql>  
mysql> INSERT INTO order_items VALUES
    ->     (1,  101, 1, 200), (2,  101, 2, 150),
    ->     (3,  102, 3,  80), (4,  102, 4, 300),
    ->     (5,  103, 5, 120), (6,  103, 8, 100),
    ->     (7,  104, 6,  50), (8,  105, 3, 120),
    ->     (9,  105, 7,  60), (10, 106, 3, 200),
    ->     (11, 106, 6, 180), (12, 107, 1, 100),
    ->     (13, 108, 2,  80), (14, 109, 5,  90),
    ->     (15, 110, 4, 200), (16, 110, 7,  50);
Query OK, 16 rows affected (0.00 sec)
Records: 16  Duplicates: 0  Warnings: 0

mysql>  
mysql> INSERT INTO employees VALUES
    ->     (1, 'Mickey',   'Mouse',     '2018-06-01', 'Sales'),
    ->     (2, 'Minnie',   'Mouse',     '2021-03-15', 'Marketing'),
    ->     (3, 'Donald',   'Duck',      '2014-08-20', 'Operations'),
    ->     (4, 'Daisy',    'Duck',      '2024-01-10', 'Sales'),
    ->     (5, 'Goofy',    'Goof',      '2015-11-05', 'HR'),
    ->     (6, 'Buzz',     'Lightyear', '2023-04-18', 'Marketing');
Query OK, 6 rows affected (0.00 sec)
Records: 6  Duplicates: 0  Warnings: 0

mysql> SELECT * FROM customers;
+-------------+------------+-----------+-------------------------+------------+
| customer_id | first_name | last_name | email                   | join_date  |
+-------------+------------+-----------+-------------------------+------------+
|           1 | Winnie     | Pooh      | winnie.thepooh@shop.com | 2024-03-10 |
|           2 | Pinocchio  | Puppet    | pinocchio@shop.com      | 2024-06-20 |
|           3 | Tigger     | Tiger     | tiger@shop.com          | 2022-01-05 |
|           4 | Piglet     | Friend    | ananya@shop.com         | 2024-09-15 |
|           5 | Kanga      | Mother    | kiran@shop.com          | 2019-07-11 |
|           6 | Roo        | Joey      | pooja@shop.com          | 2018-03-22 |
|           7 | Aladdin    | Prince    | nikhil@shop.com         | 2024-12-01 |
|           8 | Elsa       | Queen     | tanvi@shop.com          | 2023-08-14 |
+-------------+------------+-----------+-------------------------+------------+
8 rows in set (0.00 sec)

mysql> SELECT * FROM products;
+------------+-----------------+------------+--------+
| product_id | product_name    | category   | price  |
+------------+-----------------+------------+--------+
|          1 | Rice 5kg        | Grains     |  80.00 |
|          2 | Wheat Flour 5kg | Grains     |  55.00 |
|          3 | Olive Oil 1L    | Oils       | 350.00 |
|          4 | Sugar 1kg       | Sweeteners |  45.00 |
|          5 | Basmati Rice    | Grains     | 120.00 |
|          6 | Sunflower Oil   | Oils       | 180.00 |
|          7 | Honey 500g      | Sweeteners | 220.00 |
|          8 | Dal Masoor      | Pulses     |  90.00 |
+------------+-----------------+------------+--------+
8 rows in set (0.00 sec)

mysql> SELECT
    ->     CONCAT(
    ->         CONCAT(UPPER(SUBSTR(first_name,1,1)), LOWER(SUBSTR(first_name,2))),
    ->         ' ',
    ->         CONCAT(UPPER(SUBSTR(last_name,1,1)),  LOWER(SUBSTR(last_name,2)))
    ->     ) AS full_name,
    ->     TIMESTAMPDIFF(MONTH, join_date, CURDATE()) AS membership_months,
    ->     CONCAT(
    ->         SUBSTR(email,1,3),
    ->         '...',
    ->         SUBSTR(email, INSTR(email,'@'))
    ->     ) AS masked_email
    -> FROM customers
    -> WHERE YEAR(join_date) = 2024
    -> ORDER BY join_date ASC;
+------------------+-------------------+-----------------+
| full_name        | membership_months | masked_email    |
+------------------+-------------------+-----------------+
| Winnie Pooh      |                24 | win...@shop.com |
| Pinocchio Puppet |                21 | pin...@shop.com |
| Piglet Friend    |                18 | ana...@shop.com |
| Aladdin Prince   |                15 | nik...@shop.com |
+------------------+-------------------+-----------------+
4 rows in set (0.00 sec)

mysql> SELECT
    ->     P.category,
    ->     SUM(OI.quantity * P.price)  AS total_revenue,
    ->     ROUND(AVG(OI.quantity),2)   AS avg_qty_per_order,
    ->     COUNT(DISTINCT OI.order_id) AS unique_orders
    -> FROM order_items OI
    -> JOIN products P ON OI.product_id = P.product_id
    -> GROUP BY P.category
    -> HAVING SUM(OI.quantity * P.price) > 50000
    -> ORDER BY total_revenue DESC;
+----------+---------------+-------------------+---------------+
| category | total_revenue | avg_qty_per_order | unique_orders |
+----------+---------------+-------------------+---------------+
| Oils     |     181400.00 |            126.00 |             4 |
| Grains   |      61850.00 |            123.33 |             5 |
+----------+---------------+-------------------+---------------+
2 rows in set (0.00 sec)

mysql> -- Step 1: avg customer spend
mysql> SELECT AVG(total_spend) FROM (
    ->     SELECT customer_id, SUM(total_amount) AS total_spend
    ->     FROM orders GROUP BY customer_id
    -> ) AS spend_summary;
+------------------+
| AVG(total_spend) |
+------------------+
|     11787.500000 |
+------------------+
1 row in set (0.00 sec)

mysql> -- Result: avg ≈ 10912.50
mysql>  
mysql> -- Step 2: Find high spending customers
mysql> SELECT customer_id, SUM(total_amount) AS total
    -> FROM orders GROUP BY customer_id
    -> HAVING SUM(total_amount) > (
    ->     SELECT AVG(ts) FROM (SELECT customer_id,SUM(total_amount) ts
    ->     FROM orders GROUP BY customer_id) s
    -> );
+-------------+----------+
| customer_id | total    |
+-------------+----------+
|           2 | 12200.00 |
|           3 | 13000.00 |
|           5 | 18500.00 |
|           6 | 22000.00 |
+-------------+----------+
4 rows in set (0.00 sec)

mysql>  
mysql> -- Full query: employees in same dept as those handling top customers
mysql> SELECT DISTINCT
    ->     CONCAT(E.first_name,' ',E.last_name) AS employee_name,
    ->     E.hire_date,
    ->     E.department
    -> FROM employees E
    -> WHERE E.department IN ('Sales','Operations')
    -> AND E.employee_id IN (SELECT employee_id FROM employees
    ->     WHERE TIMESTAMPDIFF(YEAR,hire_date,CURDATE()) >= 2);
+---------------+------------+------------+
| employee_name | hire_date  | department |
+---------------+------------+------------+
| Mickey Mouse  | 2018-06-01 | Sales      |
| Donald Duck   | 2014-08-20 | Operations |
| Daisy Duck    | 2024-01-10 | Sales      |
+---------------+------------+------------+
3 rows in set (0.00 sec)

mysql> -- Employees working more than 5 years
mysql> SELECT first_name, last_name, 'Employee' AS type
    -> FROM employees
    -> WHERE TIMESTAMPDIFF(YEAR, hire_date, CURDATE()) > 5
    ->  
    -> UNION
    ->  
    -> -- Customers who placed 3 or more orders
    -> SELECT C.first_name, C.last_name, 'Customer' AS type
    -> FROM customers C
    -> JOIN orders O ON C.customer_id = O.customer_id
    -> GROUP BY C.customer_id, C.first_name, C.last_name
    -> HAVING COUNT(O.order_id) >= 2
    ->  
    -> ORDER BY last_name;
+------------+-----------+----------+
| first_name | last_name | type     |
+------------+-----------+----------+
| Donald     | Duck      | Employee |
| Goofy      | Goof      | Employee |
| Mickey     | Mouse     | Employee |
| Winnie     | Pooh      | Customer |
| Tigger     | Tiger     | Customer |
+------------+-----------+----------+
5 rows in set (0.00 sec)

mysql> -- Create the view
mysql> CREATE VIEW v_top_customer_revenue AS
    -> SELECT
    ->     CONCAT(C.first_name,' ',C.last_name) AS full_name,
    ->     C.email,
    ->     SUM(O.total_amount) AS total_lifetime_spending
    -> FROM customers C
    -> JOIN orders O ON C.customer_id = O.customer_id
    -> GROUP BY C.customer_id, C.first_name, C.last_name, C.email
    -> ORDER BY total_lifetime_spending DESC
    -> LIMIT 10;
Query OK, 0 rows affected (0.01 sec)

mysql> -- Query the view
mysql> SELECT * FROM v_top_customer_revenue;
+------------------+-------------------------+-------------------------+
| full_name        | email                   | total_lifetime_spending |
+------------------+-------------------------+-------------------------+
| Roo Joey         | pooja@shop.com          |                22000.00 |
| Kanga Mother     | kiran@shop.com          |                18500.00 |
| Tigger Tiger     | tiger@shop.com          |                13000.00 |
| Pinocchio Puppet | pinocchio@shop.com      |                12200.00 |
| Elsa Queen       | tanvi@shop.com          |                 9300.00 |
| Winnie Pooh      | winnie.thepooh@shop.com |                 8800.00 |
| Aladdin Prince   | nikhil@shop.com         |                 6700.00 |
| Piglet Friend    | ananya@shop.com         |                 3800.00 |
+------------------+-------------------------+-------------------------+
8 rows in set (0.00 sec)

mysql> -- Show the view definition
mysql> SHOW CREATE VIEW v_top_customer_revenue\G
*************************** 1. row ***************************
                View: v_top_customer_revenue
         Create View: CREATE ALGORITHM=UNDEFINED DEFINER=`root`@`localhost` SQL SECURITY DEFINER VIEW `v_top_customer_revenue` AS select concat(`C`.`first_name`,' ',`C`.`last_name`) AS `full_name`,`C`.`email` AS `email`,sum(`O`.`total_amount`) AS `total_lifetime_spending` from (`customers` `C` join `orders` `O` on((`C`.`customer_id` = `O`.`customer_id`))) group by `C`.`customer_id`,`C`.`first_name`,`C`.`last_name`,`C`.`email` order by `total_lifetime_spending` desc limit 10
character_set_client: utf8mb4
collation_connection: utf8mb4_0900_ai_ci
1 row in set (0.00 sec)

mysql>  
mysql> -- Drop the view (when no longer needed)
mysql> DROP VIEW v_top_customer_revenue;
Query OK, 0 rows affected (0.01 sec)

mysql> -- Add 3 test rows
mysql> INSERT INTO customers VALUES
    ->     (101,'Test1','User1','t1@x.com','2024-01-01'),
    ->     (102,'Test2','User2','t2@x.com','2024-01-02'),
    ->     (103,'Test3','User3','t3@x.com','2024-01-03');
Query OK, 3 rows affected (0.00 sec)
Records: 3  Duplicates: 0  Warnings: 0

mysql> 
mysql> -- Verify they're in
mysql> SELECT customer_id, first_name FROM customers 
    -> WHERE customer_id IN (101,102,103);
+-------------+------------+
| customer_id | first_name |
+-------------+------------+
|         101 | Test1      |
|         102 | Test2      |
|         103 | Test3      |
+-------------+------------+
3 rows in set (0.00 sec)

mysql> 
mysql> -- BEGIN TRANSACTION
mysql> START TRANSACTION;
Query OK, 0 rows affected (0.00 sec)

mysql> 
mysql> -- Correct update 1: 101 -> 201
mysql> UPDATE customers SET customer_id = 201 WHERE customer_id = 101;
Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> 
mysql> -- Set savepoint here
mysql> SAVEPOINT after_first_update;
Query OK, 0 rows affected (0.00 sec)

mysql> 
mysql> -- Correct update 2: 102 -> 202
mysql> UPDATE customers SET customer_id = 202 WHERE customer_id = 102;
Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> 
mysql> -- Erroneous update: 103 -> 201 (ERROR 1062 will fire — that's expected!)
mysql> UPDATE customers SET customer_id = 201 WHERE customer_id = 103;
ERROR 1062 (23000): Duplicate entry '201' for key 'customers.PRIMARY'
mysql> 
mysql> -- Undo back to savepoint (undoes 102->202, keeps 101->201)
mysql> ROLLBACK TO SAVEPOINT after_first_update;
Query OK, 0 rows affected (0.00 sec)

mysql> 
mysql> -- Redo 102 -> 202
mysql> UPDATE customers SET customer_id = 202 WHERE customer_id = 102;
Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> 
mysql> -- Correct update for 103 -> 203
mysql> UPDATE customers SET customer_id = 203 WHERE customer_id = 103;
Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> 
mysql> -- Commit all
mysql> COMMIT;
Query OK, 0 rows affected (0.00 sec)

mysql> 
mysql> -- Final verification
mysql> SELECT customer_id, first_name FROM customers
    -> WHERE customer_id IN (201,202,203)
    -> ORDER BY customer_id;
+-------------+------------+
| customer_id | first_name |
+-------------+------------+
|         201 | Test1      |
|         202 | Test2      |
|         203 | Test3      |
+-------------+------------+
3 rows in set (0.00 sec)

mysql> 


```


## FAQs

**Q1. When should you use a VIEW instead of a regular SELECT?**
Use a VIEW when:
- The same complex query is reused frequently
- You want to simplify access for end users or other queries
- You need to restrict access to specific columns/rows of a table
- You want to present a virtual aggregation or joined dataset without storing it physically

**Q2. What is the best way to handle data consolidation — UNION vs subquery?**
- **UNION**: Best when combining results from two structurally similar queries (same number and type of columns).
- **Subquery**: Best when you need to filter or compare based on the result of another query within a single query context.

**Q3. How do subqueries make queries more powerful?**
Subqueries allow you to:
- Use the result of one query as input to another
- Compare values against dynamically computed results (e.g., average, maximum)
- Perform conditional filtering without temporary tables
- Create modular, readable query logic
