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

2

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
