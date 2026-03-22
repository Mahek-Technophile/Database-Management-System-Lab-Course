# Assignment 4 — SQL JOINs

## Title
SQL Queries on JOINs — Inner, Left Outer, Right Outer, Full Outer, Cross, Self Join

## Aim
Write SELECT queries using JOIN operations.

## Objective
Study different JOIN operations and nested queries.

---

## Theory

### What is a JOIN?
A JOIN takes two relations (tables) and returns another relation. The JOIN condition defines how rows are matched; the JOIN type defines how non-matching rows are handled.

### Types of JOINs

| JOIN Type | Description |
|-----------|-------------|
| `CROSS JOIN` | Cartesian product — every row of table A paired with every row of table B; no condition needed |
| `INNER JOIN` | Returns only rows that have matching values in both tables |
| `LEFT OUTER JOIN` | Returns all rows from the left table + matched rows from the right table (NULL for non-matches) |
| `RIGHT OUTER JOIN` | Returns all rows from the right table + matched rows from the left table (NULL for non-matches) |
| `FULL OUTER JOIN` | Returns all rows from both tables (NULL where there is no match); MySQL: use `LEFT JOIN UNION RIGHT JOIN` |
| `SELF JOIN` | A table joined with itself using aliases to compare rows within the same table |

### Syntax Examples

```sql
-- CROSS JOIN
SELECT * FROM TableA CROSS JOIN TableB;

-- INNER JOIN
SELECT * FROM TableA
INNER JOIN TableB ON TableA.id = TableB.id;

-- LEFT OUTER JOIN
SELECT * FROM TableA
LEFT OUTER JOIN TableB ON TableA.id = TableB.id;

-- RIGHT OUTER JOIN
SELECT * FROM TableA
RIGHT OUTER JOIN TableB ON TableA.id = TableB.id;

-- FULL OUTER JOIN (MySQL simulation)
SELECT * FROM TableA
LEFT JOIN TableB ON TableA.id = TableB.id
UNION
SELECT * FROM TableA
RIGHT JOIN TableB ON TableA.id = TableB.id;

-- SELF JOIN
SELECT A.name, B.name AS manager
FROM Employee A
INNER JOIN Employee B ON A.manager_id = B.emp_id;
```

---

## Exercises

### Batch A — Set 1: Employee / Position / Duty_allocation

1. **INNER JOIN** — Employees with their assigned duties
2. **LEFT OUTER JOIN** — All employees, including those with no assigned duties
3. **RIGHT OUTER JOIN** — All positions, including those with no assigned employees
4. **FULL OUTER JOIN** — All employees and all positions (matched and unmatched)
5. **SELF JOIN** — Employees who share the same skill

### Batch A — Set 2: Mail Order Database

1. **INNER JOIN** — Orders where quantity ordered > quantity on hand
2. **LEFT JOIN** — All customers with their orders (include customers with no orders)
3. **RIGHT JOIN** — All parts with their orders (include parts with no orders)
4. **FULL OUTER JOIN** — All customers and all orders (matched and unmatched)
5. **SELF JOIN** — Customers who share the same ZIP code

---

## Implementation:
### Set-01  Consider the following database
  * Employee(emp_no,name,skill,pay-rate)
  * Position(posting_no,skill)
  * Duty_allocation(posting_no,emp_no,day,shift)
#### Creating Database
<img width="1382" height="766" alt="image" src="https://github.com/user-attachments/assets/946664ed-ccb2-4a12-b4bb-12b3d98d26a2" />

#### Inserting values and displaying : 
<img width="675" height="478" alt="image" src="https://github.com/user-attachments/assets/4fc17381-2574-475b-b364-9574cc278f48" />
<img width="650" height="606" alt="image" src="https://github.com/user-attachments/assets/60371b70-3dfd-4d50-9469-250bf0573414" />


#### Queries:
##### 1. List the employees and their assigned positions for a specific day, including employee name, posting number, and shift.

<img width="701" height="268" alt="image" src="https://github.com/user-attachments/assets/4ecf0f86-d9b2-4c56-9c84-10fe18c4844a" />

##### 2. Show all employees and any duties they are assigned to on a particular day, including those employees with no duty (show NULL for posting_no and shift if none).
<img width="694" height="284" alt="image" src="https://github.com/user-attachments/assets/ef746c15-766f-4cde-aea3-698c4d90bb3a" />

#### 3. List all positions (postings) and their employees assigned for a given day—include positions with no employees assigned.
<img width="874" height="279" alt="image" src="https://github.com/user-attachments/assets/f12243b5-3937-4b18-bad3-e1e8b4d4fd1a" />

#### 4. Provide a full view of assignments for a given day, showing employee names, postings, and shift—include unassigned employees and unfilled positions.
<img width="651" height="459" alt="image" src="https://github.com/user-attachments/assets/00195ee6-1b84-4574-a98c-4d0c78fee041" />

#### 5. Identify employees who share the same skill set, essentially pairing each employee with others who have the same skill.
<img width="685" height="306" alt="image" src="https://github.com/user-attachments/assets/22923f3d-55b6-46b2-acf1-2c6c2f590264" />

---

### Set-02 Consider the mail order db system & solve the Queries
– emp (eno,ename,Zip,hdate)
– parts(pno,pname,qty_on_hand, price)
– customer(cno,cname,street,Zip,phone)
– order(ono,cno,receivedate,shippeddate)
– odetails(ono,pno,qty)
– zipcode(Zip,city)

#### Creating database:
<img width="1034" height="977" alt="image" src="https://github.com/user-attachments/assets/62ecdf7d-48dc-46db-b61b-e283e8852cf9" />
<img width="783" height="363" alt="image" src="https://github.com/user-attachments/assets/fa54a0f1-945e-47e2-8a49-aacaa2a1ccbe" />


#### Inserting Values:
<img width="930" height="999" alt="image" src="https://github.com/user-attachments/assets/8b25b580-cd60-4e5c-a22c-0f8edeca6034" />


#### Displaying:
<img width="758" height="671" alt="image" src="https://github.com/user-attachments/assets/3011878a-7674-4b2b-b59e-c9f01b8e9ac2" />

#### Queries:
##### 1.Retrieve detailed order lines—including order number, customer name, part name, and quantity—for orders where the quantity ordered exceeds the quantity on hand.
<img width="527" height="263" alt="image" src="https://github.com/user-attachments/assets/fd9060e5-5553-408a-b0a0-5d46285024e1" />

##### 2.List all customers and any orders they have placed, including customer name, order number (if any), and received date. Include customers with no orders (order details will appear as NULL).
<img width="532" height="301" alt="image" src="https://github.com/user-attachments/assets/e9564a2b-51e7-4e7b-8713-2969eede9bf5" />

##### 3. Show all parts and the orders they appear in—include part name, order number, and order received date—while also listing parts that have never been ordered.
<img width="560" height="394" alt="image" src="https://github.com/user-attachments/assets/28f7db67-8dde-44c8-a45e-755861e39a95" />

##### 4.Provide a comprehensive listing of orders and their details—including orders without items and item records without a valid order—with columns: order number, part number, and quantity.
<img width="640" height="294" alt="image" src="https://github.com/user-attachments/assets/fe4fb79d-7278-4c42-b972-30709dbb7547" />

##### 5. Identify customers who live in the same ZIP code (excluding themselves). Return each pair of customer names and their shared location’s city.
<img width="640" height="294" alt="image" src="https://github.com/user-attachments/assets/30c74a47-31fe-4e93-a639-b20f33b3472c" />

---

DB 1: 
```sql
mahek@mahek-ZenBook-UX325EA-UX325EA:~$ sudo mysql
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 17
Server version: 8.0.45-0ubuntu0.22.04.1 (Ubuntu)

Copyright (c) 2000, 2026, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> CREATE DATABASE duty_db;
Query OK, 1 row affected (0.01 sec)

mysql> USE duty_db;
Database changed
mysql> CREATE TABLE Employee ( emp_no   INT PRIMARY KEY, name  VARCHAR(50)  NOT NULL, skill VARCHAR(30)  NOT NULL, pay_rate DECIMAL(8,2) NOT NULL );
Query OK, 0 rows affected (0.01 sec)

mysql>  
mysql> CREATE TABLE Job_position (
    ->     posting_no INT         PRIMARY KEY,
    ->     skill      VARCHAR(30) NOT NULL
    -> );
Query OK, 0 rows affected (0.01 sec)

mysql> 
mysql> 
mysql> CREATE TABLE Duty_allocation (
    ->     posting_no INT         NOT NULL,
    ->     emp_no     INT         NOT NULL,
    ->     day        VARCHAR(15) NOT NULL,
    ->     shift      VARCHAR(15) NOT NULL,
    ->     PRIMARY KEY (posting_no, emp_no, day),
    ->     FOREIGN KEY (posting_no) REFERENCES Job_position(posting_no),
    ->     FOREIGN KEY (emp_no)     REFERENCES Employee(emp_no)
    -> );
Query OK, 0 rows affected (0.02 sec)

mysql> INSERT INTO Employee VALUES
    ->     (1, 'Harry Potter', 'Python', 500.00),
    ->     (2, 'Hermione Granger',  'Java',   600.00),
    ->     (3, 'Ron Weasley',   'Python', 550.00),
    ->     (4, 'Nevin Longbottom',   'SQL',    480.00),
    ->     (5, 'Cedric Diggory',    'Java',   620.00);
Query OK, 5 rows affected (0.01 sec)
Records: 5  Duplicates: 0  Warnings: 0

mysql>  
mysql> INSERT INTO Job_position VALUES
    ->     (101, 'Python'),
    ->     (102, 'Java'),
    ->     (103, 'C++');
Query OK, 3 rows affected (0.00 sec)
Records: 3  Duplicates: 0  Warnings: 0

mysql>  
mysql> INSERT INTO Duty_allocation VALUES
    ->     (101, 1, 'Monday', 'Morning'),
    ->     (102, 2, 'Monday', 'Evening'),
    ->     (101, 3, 'Monday', 'Night'),
    ->     (102, 5, 'Tuesday','Morning'),
    ->     (101, 1, 'Tuesday','Evening');
Query OK, 5 rows affected (0.00 sec)
Records: 5  Duplicates: 0  Warnings: 0

mysql> SELECT * FROM Employee;
+--------+------------------+--------+----------+
| emp_no | name             | skill  | pay_rate |
+--------+------------------+--------+----------+
|      1 | Harry Potter     | Python |   500.00 |
|      2 | Hermione Granger | Java   |   600.00 |
|      3 | Ron Weasley      | Python |   550.00 |
|      4 | Nevin Longbottom | SQL    |   480.00 |
|      5 | Cedric Diggory   | Java   |   620.00 |
+--------+------------------+--------+----------+
5 rows in set (0.00 sec)

mysql> SELECT * FROM Job_position;
+------------+--------+
| posting_no | skill  |
+------------+--------+
|        101 | Python |
|        102 | Java   |
|        103 | C++    |
+------------+--------+
3 rows in set (0.00 sec)

mysql> SELECT * FROM Duty_allocation;
+------------+--------+---------+---------+
| posting_no | emp_no | day     | shift   |
+------------+--------+---------+---------+
|        101 |      1 | Monday  | Morning |
|        101 |      1 | Tuesday | Evening |
|        101 |      3 | Monday  | Night   |
|        102 |      2 | Monday  | Evening |
|        102 |      5 | Tuesday | Morning |
+------------+--------+---------+---------+
5 rows in set (0.00 sec)

mysql> SELECT E.emp_no, E.name, DA.posting_no, DA.shift
    -> FROM Employee E
    -> INNER JOIN Duty_allocation DA ON E.emp_no = DA.emp_no
    -> WHERE DA.day = 'Monday'
    -> ORDER BY DA.posting_no;
+--------+------------------+------------+---------+
| emp_no | name             | posting_no | shift   |
+--------+------------------+------------+---------+
|      1 | Harry Potter     |        101 | Morning |
|      3 | Ron Weasley      |        101 | Night   |
|      2 | Hermione Granger |        102 | Evening |
+--------+------------------+------------+---------+
3 rows in set (0.00 sec)

mysql> SELECT E.emp_no, E.name, DA.posting_no, DA.shift
    -> FROM Employee E
    -> LEFT OUTER JOIN Duty_allocation DA
    ->     ON E.emp_no = DA.emp_no AND DA.day = 'Monday'
    -> ORDER BY E.emp_no;
+--------+------------------+------------+---------+
| emp_no | name             | posting_no | shift   |
+--------+------------------+------------+---------+
|      1 | Harry Potter     |        101 | Morning |
|      2 | Hermione Granger |        102 | Evening |
|      3 | Ron Weasley      |        101 | Night   |
|      4 | Nevin Longbottom |       NULL | NULL    |
|      5 | Cedric Diggory   |       NULL | NULL    |
+--------+------------------+------------+---------+
5 rows in set (0.00 sec)

mysql> SELECT P.posting_no, P.skill, E.name, DA.shift
    -> FROM Duty_allocation DA
    -> RIGHT OUTER JOIN Job_position P ON DA.posting_no = P.posting_no AND DA.day = 'Monday'
    -> LEFT  JOIN Employee E       ON DA.emp_no = E.emp_no
    -> ORDER BY P.posting_no;
+------------+--------+------------------+---------+
| posting_no | skill  | name             | shift   |
+------------+--------+------------------+---------+
|        101 | Python | Harry Potter     | Morning |
|        101 | Python | Ron Weasley      | Night   |
|        102 | Java   | Hermione Granger | Evening |
|        103 | C++    | NULL             | NULL    |
+------------+--------+------------------+---------+
4 rows in set (0.00 sec)

mysql> -- LEFT part: all employees, NULL where no Monday duty
mysql> SELECT E.name, DA.posting_no, DA.shift
    -> FROM Employee E
    -> LEFT JOIN Duty_allocation DA
    ->     ON E.emp_no = DA.emp_no AND DA.day = 'Monday'
    ->  
    -> UNION
    ->  
    -> -- RIGHT part: all duty slots, NULL where no employee match
    -> SELECT E.name, DA.posting_no, DA.shift
    -> FROM Employee E
    -> RIGHT JOIN Duty_allocation DA
    ->     ON E.emp_no = DA.emp_no AND DA.day = 'Monday';
+------------------+------------+---------+
| name             | posting_no | shift   |
+------------------+------------+---------+
| Harry Potter     |        101 | Morning |
| Hermione Granger |        102 | Evening |
| Ron Weasley      |        101 | Night   |
| Nevin Longbottom |       NULL | NULL    |
| Cedric Diggory   |       NULL | NULL    |
| NULL             |        101 | Evening |
| NULL             |        102 | Morning |
+------------------+------------+---------+
7 rows in set (0.01 sec)

mysql> SELECT E1.name AS Employee_1,
    ->        E2.name AS Employee_2,
    ->        E1.skill AS Shared_Skill
    -> FROM Employee E1
    -> INNER JOIN Employee E2
    ->     ON E1.skill = E2.skill
    ->     AND E1.emp_no < E2.emp_no
    -> ORDER BY E1.skill, E1.emp_no;
+------------------+----------------+--------------+
| Employee_1       | Employee_2     | Shared_Skill |
+------------------+----------------+--------------+
| Hermione Granger | Cedric Diggory | Java         |
| Harry Potter     | Ron Weasley    | Python       |
+------------------+----------------+--------------+
2 rows in set (0.00 sec)

mysql> 


```

DB 2:
```sql
mahek@mahek-ZenBook-UX325EA-UX325EA:~$ sudo mysql
[sudo] password for mahek: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 14
Server version: 8.0.45-0ubuntu0.22.04.1 (Ubuntu)

Copyright (c) 2000, 2026, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> CREATE DATABASE mailorder_db;
Query OK, 1 row affected (0.01 sec)

mysql> USE mailorder_db;
Database changed
mysql> CREATE TABLE zipcode (
    ->     zip  VARCHAR(10) PRIMARY KEY,
    ->     city VARCHAR(50) NOT NULL
    -> );
Query OK, 0 rows affected (0.02 sec)

mysql>  
mysql> CREATE TABLE emp (
    ->     eno   INT         PRIMARY KEY,
    ->     ename VARCHAR(50) NOT NULL,
    ->     zip   VARCHAR(10),
    ->     hdate DATE,
    ->     FOREIGN KEY (zip) REFERENCES zipcode(zip)
    -> );
Query OK, 0 rows affected (0.01 sec)

mysql>  
mysql> CREATE TABLE customer (
    ->     cno    INT         PRIMARY KEY,
    ->     cname  VARCHAR(60) NOT NULL,
    ->     street VARCHAR(100),
    ->     zip    VARCHAR(10),
    ->     phone  VARCHAR(15),
    ->     FOREIGN KEY (zip) REFERENCES zipcode(zip)
    -> );
Query OK, 0 rows affected (0.01 sec)

mysql>  
mysql> CREATE TABLE parts (
    ->     pno         INT          PRIMARY KEY,
    ->     pname       VARCHAR(60)  NOT NULL,
    ->     qty_on_hand INT          NOT NULL,
    ->     price       DECIMAL(8,2) NOT NULL
    -> );
Query OK, 0 rows affected (0.01 sec)

mysql>  
mysql> CREATE TABLE orders (
    ->     ono          INT  PRIMARY KEY,
    ->     cno          INT  NOT NULL,
    ->     receivedate  DATE,
    ->     shippeddate  DATE,
    ->     FOREIGN KEY (cno) REFERENCES customer(cno)
    -> );
Query OK, 0 rows affected (0.01 sec)

mysql>  
mysql> CREATE TABLE odetails (
    ->     ono INT NOT NULL,
    ->     pno INT NOT NULL,
    ->     qty INT NOT NULL,
    ->     PRIMARY KEY (ono, pno),
    ->     FOREIGN KEY (ono) REFERENCES orders(ono),
    ->     FOREIGN KEY (pno) REFERENCES parts(pno)
    -> );
Query OK, 0 rows affected (0.01 sec)

mysql> INSERT INTO zipcode VALUES
    ->     ('411001','Narnia'), ('400001','Ahtohalan'),
    ->     ('560001','Zootopia'), ('110001','Genovia');
Query OK, 4 rows affected (0.01 sec)
Records: 4  Duplicates: 0  Warnings: 0

mysql>  
mysql> 
mysql> 
mysql> INSERT INTO emp VALUES
    ->     (1, 'Godric Gryffindor',   '411001', '2020-06-01'),
    ->     (2, 'Olaf','400001', '2019-03-15'),
    ->     (3, 'Judy Hopps',     '560001', '2021-08-20');
Query OK, 3 rows affected (0.00 sec)
Records: 3  Duplicates: 0  Warnings: 0

mysql>  
mysql> INSERT INTO customer VALUES
    ->     (1, 'Helga Hufflepuff',    'Owl Wood',     '411001', '9876543210'),
    ->     (2, 'Elsa',  'Arendelle',     '400001', '9988776655'),
    ->     (3, 'Rowena Ravenclaw',    'Diagon ALley',       '411001', '9123456789'),
    ->     (4, 'Nick Wilde',   'Rainforest District', '560001', '9001234567'),
    ->     (5, 'Mia Thermopolis',  'Castle','110001', '9811223344');
Query OK, 5 rows affected (0.00 sec)
Records: 5  Duplicates: 0  Warnings: 0

mysql>  
mysql> INSERT INTO parts VALUES
    ->     (10, 'Keyboard',  50, 800.00),
    ->     (11, 'Mouse',     30, 400.00),
    ->     (12, 'Monitor',    5, 12000.00),
    ->     (13, 'USB Hub',   20, 650.00),
    ->     (14, 'Webcam',    10, 2500.00);
Query OK, 5 rows affected (0.00 sec)
Records: 5  Duplicates: 0  Warnings: 0

mysql>  
mysql> INSERT INTO orders VALUES
    ->     (1001, 1, '2025-01-10', '2025-01-15'),
    ->     (1002, 2, '2025-02-05', NULL),
    ->     (1003, 3, '2025-03-01', '2025-03-05'),
    ->     (1004, 4, '2025-03-20', '2025-03-22');
Query OK, 4 rows affected (0.00 sec)
Records: 4  Duplicates: 0  Warnings: 0

mysql>  
mysql> INSERT INTO odetails VALUES
    ->     (1001, 10, 60),
    ->     (1001, 11, 10),
    ->     (1002, 12,  3),
    ->     (1003, 11, 35),
    ->     (1004, 13, 15),
    ->     (1004, 14,  8);
Query OK, 6 rows affected (0.00 sec)
Records: 6  Duplicates: 0  Warnings: 0

mysql> SELECT * FROM customer;
+-----+------------------+---------------------+--------+------------+
| cno | cname            | street              | zip    | phone      |
+-----+------------------+---------------------+--------+------------+
|   1 | Helga Hufflepuff | Owl Wood            | 411001 | 9876543210 |
|   2 | Elsa             | Arendelle           | 400001 | 9988776655 |
|   3 | Rowena Ravenclaw | Diagon ALley        | 411001 | 9123456789 |
|   4 | Nick Wilde       | Rainforest District | 560001 | 9001234567 |
|   5 | Mia Thermopolis  | Castle              | 110001 | 9811223344 |
+-----+------------------+---------------------+--------+------------+
5 rows in set (0.00 sec)

mysql> SELECT * FROM parts;
+-----+----------+-------------+----------+
| pno | pname    | qty_on_hand | price    |
+-----+----------+-------------+----------+
|  10 | Keyboard |          50 |   800.00 |
|  11 | Mouse    |          30 |   400.00 |
|  12 | Monitor  |           5 | 12000.00 |
|  13 | USB Hub  |          20 |   650.00 |
|  14 | Webcam   |          10 |  2500.00 |
+-----+----------+-------------+----------+
5 rows in set (0.00 sec)

mysql> SELECT * FROM odetails;
+------+-----+-----+
| ono  | pno | qty |
+------+-----+-----+
| 1001 |  10 |  60 |
| 1001 |  11 |  10 |
| 1002 |  12 |   3 |
| 1003 |  11 |  35 |
| 1004 |  13 |  15 |
| 1004 |  14 |   8 |
+------+-----+-----+
6 rows in set (0.00 sec)

mysql> SELECT O.ono, C.cname, P.pname,
    ->        OD.qty AS qty_ordered,
    ->        P.qty_on_hand
    -> FROM odetails OD
    -> INNER JOIN orders  O ON OD.ono = O.ono
    -> INNER JOIN customer C ON O.cno = C.cno
    -> INNER JOIN parts    P ON OD.pno = P.pno
    -> WHERE OD.qty > P.qty_on_hand
    -> ORDER BY O.ono;
+------+------------------+----------+-------------+-------------+
| ono  | cname            | pname    | qty_ordered | qty_on_hand |
+------+------------------+----------+-------------+-------------+
| 1001 | Helga Hufflepuff | Keyboard |          60 |          50 |
| 1003 | Rowena Ravenclaw | Mouse    |          35 |          30 |
+------+------------------+----------+-------------+-------------+
2 rows in set (0.00 sec)

mysql> SELECT C.cno, C.cname, O.ono, O.receivedate
    -> FROM customer C
    -> LEFT OUTER JOIN orders O ON C.cno = O.cno
    -> ORDER BY C.cno;
+-----+------------------+------+-------------+
| cno | cname            | ono  | receivedate |
+-----+------------------+------+-------------+
|   1 | Helga Hufflepuff | 1001 | 2025-01-10  |
|   2 | Elsa             | 1002 | 2025-02-05  |
|   3 | Rowena Ravenclaw | 1003 | 2025-03-01  |
|   4 | Nick Wilde       | 1004 | 2025-03-20  |
|   5 | Mia Thermopolis  | NULL | NULL        |
+-----+------------------+------+-------------+
5 rows in set (0.00 sec)

mysql> SELECT P.pno, P.pname, OD.ono, O.receivedate
    -> FROM odetails OD
    -> RIGHT OUTER JOIN parts P ON OD.pno = P.pno
    -> LEFT  JOIN orders O      ON OD.ono  = O.ono
    -> ORDER BY P.pno;
+-----+----------+------+-------------+
| pno | pname    | ono  | receivedate |
+-----+----------+------+-------------+
|  10 | Keyboard | 1001 | 2025-01-10  |
|  11 | Mouse    | 1001 | 2025-01-10  |
|  11 | Mouse    | 1003 | 2025-03-01  |
|  12 | Monitor  | 1002 | 2025-02-05  |
|  13 | USB Hub  | 1004 | 2025-03-20  |
|  14 | Webcam   | 1004 | 2025-03-20  |
+-----+----------+------+-------------+
6 rows in set (0.00 sec)

mysql> SELECT O.ono, OD.pno, OD.qty
    -> FROM orders O
    -> LEFT JOIN odetails OD ON O.ono = OD.ono
    ->  
    -> UNION
    ->  
    -> SELECT O.ono, OD.pno, OD.qty
    -> FROM orders O
    -> RIGHT JOIN odetails OD ON O.ono = OD.ono
    -> ORDER BY ono, pno;
+------+------+------+
| ono  | pno  | qty  |
+------+------+------+
| 1001 |   10 |   60 |
| 1001 |   11 |   10 |
| 1002 |   12 |    3 |
| 1003 |   11 |   35 |
| 1004 |   13 |   15 |
| 1004 |   14 |    8 |
+------+------+------+
6 rows in set (0.00 sec)

mysql> SELECT C1.cname AS Customer_1,
    ->        C2.cname AS Customer_2,
    ->        Z.city   AS Shared_City
    -> FROM customer C1
    -> INNER JOIN customer C2 ON C1.zip = C2.zip AND C1.cno < C2.cno
    -> INNER JOIN zipcode  Z  ON C1.zip = Z.zip
    -> ORDER BY Z.city;
+------------------+------------------+-------------+
| Customer_1       | Customer_2       | Shared_City |
+------------------+------------------+-------------+
| Helga Hufflepuff | Rowena Ravenclaw | Narnia      |
+------------------+------------------+-------------+
1 row in set (0.00 sec)

mysql> 


```





























## FAQs

**Q1. How do you handle ambiguous column names when joining tables?**

Use table name (or alias) prefix to qualify the column:
```sql
SELECT e.name, d.dname
FROM employee e
INNER JOIN department d ON e.dept_id = d.dept_id;
```

**Q2. What are the performance considerations for different JOINs?**
- `INNER JOIN` is generally the fastest because it only returns matching rows.
- `OUTER JOINs` are slower since they must check both sides and handle NULLs.
- `CROSS JOIN` can produce very large result sets and should be used carefully.
- Proper indexing on JOIN columns significantly improves performance.

**Q3. What is the difference between ON and USING in JOINs?**
- `ON` allows specifying any join condition, including columns with different names:
  ```sql
  INNER JOIN dept ON emp.dept_id = dept.id
  ```
- `USING` is a shorthand when both tables have a column with the **same name**:
  ```sql
  INNER JOIN dept USING (dept_id)
  ```
  With `USING`, the joined column appears only once in the result.
