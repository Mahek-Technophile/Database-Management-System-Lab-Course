# Assignment 7 — PL/SQL Stored Procedures & Functions

## Title
Stored Procedures and Functions in PL/SQL

## Aim
Write PL/SQL Procedures and Functions for given problem statements.

## Database (Batch A)
- `Employees` table
- `EmployeeSalaryLog` table

---
### LAB WORK:
```sql
-- Create and use database
CREATE DATABASE students;
USE students;

-- Create table
CREATE TABLE stud (
    prn   INT,
    name  VARCHAR(10),
    sub   VARCHAR(10),
    atnd  INT,
    marks INT
);

-- Insert records
INSERT INTO stud VALUES (123, 'Elsa',  'dbms', 90, 79);
INSERT INTO stud VALUES (234, 'Moana', 'daa',  93, 85);
INSERT INTO stud VALUES (345, 'Simba', 'ps',   85, 90);
INSERT INTO stud VALUES (456, 'Ariel', 'oop',  88, 89);

-- Verify data
SELECT * FROM stud;

-- ───────
-- P1: Display all records
-- ────────
DELIMITER $
CREATE PROCEDURE p1()
BEGIN
    SELECT * FROM stud;
END $
DELIMITER ;

CALL p1();

-- ──────
-- P2: Get attendance by PRN (IN parameter)
-- ────────
DELIMITER $
CREATE PROCEDURE p2(IN var1 INT)
BEGIN
    SELECT atnd FROM stud WHERE prn = var1;
END $
DELIMITER ;

CALL p2(234);

-- ────────
-- P3: Get attendance & marks by PRN
-- ────────
DELIMITER $
CREATE PROCEDURE p3(IN var1 INT)
BEGIN
    SELECT atnd, marks FROM stud WHERE prn = var1;
END $
DELIMITER ;

CALL p3(123);

-- ──────
-- P4: Update marks by PRN (IN parameter)
-- ───
DELIMITER $
CREATE PROCEDURE p4(IN var1 INT)
BEGIN
    UPDATE stud SET marks = var1 WHERE prn = 456;
END $
DELIMITER ;

CALL p4(89);
SELECT * FROM stud;

-- ──────────
-- P5: Get max attendance (OUT parameter)
-- ─────────
DELIMITER $
CREATE PROCEDURE p5(OUT v2 INT)
BEGIN
    SELECT MAX(atnd) INTO v2 FROM stud;
END $
DELIMITER ;

CALL p5(@a);
SELECT @a;

-- ──────
-- P6: Get max, min, avg attendance (OUT parameters)
-- ───────
DELIMITER $
CREATE PROCEDURE p6(OUT v1 INT, OUT v2 INT, OUT v3 INT)
BEGIN
    SELECT MAX(atnd) INTO v1 FROM stud;
    SELECT MIN(atnd) INTO v2 FROM stud;
    SELECT AVG(atnd) INTO v3 FROM stud;
END $
DELIMITER ;

CALL p6(@a, @b, @c);
SELECT @a AS max_atnd, @b AS min_atnd, @c AS avg_atnd;

-- ────────
-- F1: Function to update marks and return new marks
-- ────────
DELIMITER $
CREATE FUNCTION f1(v1 INT, m1 INT)
RETURNS INT
DETERMINISTIC
BEGIN
    UPDATE stud SET marks = m1 WHERE prn = v1;
    RETURN m1;
END $
DELIMITER ;

SELECT f1(123, 95);
SELECT * FROM stud;
```
### Implementation Queries :

#### Creation of DB & INsertion of values:
<img width="821" height="880" alt="image" src="https://github.com/user-attachments/assets/6f0afdaa-48e9-4906-9cb5-779783a6010a" />

### Q.1) Design a procedure named UpdateEmployeeSalary that takes two input parameters: employee_id (an integer) and new_salary (a decimal).
<img width="802" height="771" alt="image" src="https://github.com/user-attachments/assets/d7a7a967-3d73-47b8-b2a5-ed1189c9e0ff" />

1. Check if an employee with the provided employee_id exists in the Employees table.
 <img width="804" height="500" alt="image" src="https://github.com/user-attachments/assets/ff04532c-7c88-4574-87ac-5aac26100f60" />
  
2. If the employee exists, update their salary to the new_salary.
<img width="776" height="496" alt="image" src="https://github.com/user-attachments/assets/1a59f526-e145-4012-b6b9-4056e0470aef" />

3.  If the employee does not exist, do nothing.
<img width="570" height="184" alt="image" src="https://github.com/user-attachments/assets/fa8694dd-9d9d-4c39-9a1b-7a73e38a9c96" />

4. After the update (if it occurred), insert a record into an EmployeeSalaryLog table. This log table should capture the employee_id, the
old_salary, the new_salary, and the timestamp of the update. The old_salary should be the value before the update took place.
<img width="688" height="747" alt="image" src="https://github.com/user-attachments/assets/d91298df-4d0e-4c55-a414-499d4681e132" />

### Q.2) Create a function named CalculateAnnualBonus that accepts one input parameter: employee_id (an integer). The function should return a decimal value representing the employee's annual bonus. The bonus is calculated based on their current salary and their years of service.
<img width="679" height="780" alt="image" src="https://github.com/user-attachments/assets/2a08bbe8-6346-41ab-b20f-e1b6c0ae2536" />

The calculation rules are as follows:
● If an employee has less than 5 years of service, their bonus is 10% of their current salary.
● If an employee has 5 or more years of service but less than 10, their bonus is 15% of their current salary.
● If an employee has 10 or more years of service, their bonus is 20% of their current salary.
The function should retrieve the employee's salary from the Employees table and their years of service by calculating the difference
between the current date and their hire_date. If the employee_id does not exist, the function should return NULL.
   <img width="1061" height="775" alt="image" src="https://github.com/user-attachments/assets/844f8876-e65d-4010-aa1e-8b730c12fff1" />

### Implementation:
```sql
mahek@mahek-ZenBook-UX325EA-UX325EA:~$ sudo mysql
[sudo] password for mahek: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 10
Server version: 8.0.45-0ubuntu0.22.04.1 (Ubuntu)

Copyright (c) 2000, 2026, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> CREATE DATABASE employee_db;
Query OK, 1 row affected (0.01 sec)

mysql> USE employee_db;
Database changed
mysql> CREATE TABLE Employees (
    ->     employee_id INT          PRIMARY KEY,
    ->     first_name  VARCHAR(50)  NOT NULL,
    ->     last_name   VARCHAR(50)  NOT NULL,
    ->     salary      DECIMAL(10,2) NOT NULL,
    ->     hire_date   DATE          NOT NULL,
    ->     department  VARCHAR(50)   NOT NULL
    -> );
Query OK, 0 rows affected (0.01 sec)

mysql>  
mysql> CREATE TABLE EmployeeSalaryLog (
    ->     log_id      INT           PRIMARY KEY AUTO_INCREMENT,
    ->     employee_id INT           NOT NULL,
    ->     old_salary  DECIMAL(10,2) NOT NULL,
    ->     new_salary  DECIMAL(10,2) NOT NULL,
    ->     updated_at  TIMESTAMP     DEFAULT CURRENT_TIMESTAMP
    -> );
Query OK, 0 rows affected (0.00 sec)

mysql> INSERT INTO Employees VALUES
    ->     (1, 'Mickey',  'Mouse',   45000.00, '1928-11-18', 'Entertainment'),
    ->     (2, 'Elsa',    'Arendelle', 60000.00, '2013-11-27', 'Royalty'),
    ->     (3, 'Simba',   'Lion',    72000.00, '1994-06-24', 'Leadership'),
    ->     (4, 'Moana',   'Voyager', 30000.00, '2016-11-23', 'Exploration'),
    ->     (5, 'Aladdin', 'Streetrat', 85000.00, '1992-11-25', 'Adventure'),
    ->     (6, 'Buzz',    'Lightyear', 55000.00, '1995-11-22', 'Space');
Query OK, 6 rows affected (0.01 sec)
Records: 6  Duplicates: 0  Warnings: 0

mysql> DELIMITER $$
mysql>  
mysql> CREATE PROCEDURE UpdateEmployeeSalary(
    ->     IN p_emp_id     INT,
    ->     IN p_new_salary DECIMAL(10,2)
    -> )
    -> BEGIN
    ->     DECLARE v_old_salary DECIMAL(10,2);
    ->     DECLARE emp_count    INT DEFAULT 0;
    ->  
    ->     -- Check if the employee exists
    ->     SELECT COUNT(*) INTO emp_count
    ->     FROM Employees
    ->     WHERE employee_id = p_emp_id;
    ->  
    ->     IF emp_count > 0 THEN
    ->  
    ->         -- Capture old salary before update
    ->         SELECT salary INTO v_old_salary
    ->         FROM Employees
    ->         WHERE employee_id = p_emp_id;
    ->  
    ->         -- Update salary in Employees table
    ->         UPDATE Employees
    ->         SET salary = p_new_salary
    ->         WHERE employee_id = p_emp_id;
    ->  
    ->         -- Log the salary change
    ->         INSERT INTO EmployeeSalaryLog (employee_id, old_salary, new_salary)
    ->         VALUES (p_emp_id, v_old_salary, p_new_salary);
    ->  
    ->         SELECT CONCAT('Salary updated for employee ID: ', p_emp_id) AS message;
    ->  
    ->     ELSE
    ->         SELECT 'ERROR: Employee not found.' AS message;
    ->     END IF;
    ->  
    -> END$$
Query OK, 0 rows affected (0.01 sec)

mysql>  
mysql> DELIMITER ;
mysql> CALL UpdateEmployeeSalary(1, 52000.00);
+-----------------------------------+
| message                           |
+-----------------------------------+
| Salary updated for employee ID: 1 |
+-----------------------------------+
1 row in set (0.01 sec)

Query OK, 0 rows affected (0.01 sec)

mysql> -- Verify the update in Employees
mysql> SELECT employee_id, first_name, salary FROM Employees WHERE employee_id = 1;
+-------------+------------+----------+
| employee_id | first_name | salary   |
+-------------+------------+----------+
|           1 | Mickey     | 52000.00 |
+-------------+------------+----------+
1 row in set (0.00 sec)

mysql> -- Verify the log entry
mysql> SELECT * FROM EmployeeSalaryLog;
+--------+-------------+------------+------------+---------------------+
| log_id | employee_id | old_salary | new_salary | updated_at          |
+--------+-------------+------------+------------+---------------------+
|      1 |           1 |   45000.00 |   52000.00 | 2026-03-23 22:33:17 |
+--------+-------------+------------+------------+---------------------+
1 row in set (0.00 sec)

mysql> CALL UpdateEmployeeSalary(99, 70000.00);
+----------------------------+
| message                    |
+----------------------------+
| ERROR: Employee not found. |
+----------------------------+
1 row in set (0.00 sec)

Query OK, 0 rows affected (0.01 sec)

mysql> CALL UpdateEmployeeSalary(2, 65000.00);
+-----------------------------------+
| message                           |
+-----------------------------------+
| Salary updated for employee ID: 2 |
+-----------------------------------+
1 row in set (0.01 sec)

Query OK, 0 rows affected (0.01 sec)

mysql> CALL UpdateEmployeeSalary(3, 80000.00);
+-----------------------------------+
| message                           |
+-----------------------------------+
| Salary updated for employee ID: 3 |
+-----------------------------------+
1 row in set (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

mysql> CALL UpdateEmployeeSalary(5, 90000.00);
+-----------------------------------+
| message                           |
+-----------------------------------+
| Salary updated for employee ID: 5 |
+-----------------------------------+
1 row in set (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

mysql>  
mysql> SELECT * FROM EmployeeSalaryLog ORDER BY log_id;
+--------+-------------+------------+------------+---------------------+
| log_id | employee_id | old_salary | new_salary | updated_at          |
+--------+-------------+------------+------------+---------------------+
|      1 |           1 |   45000.00 |   52000.00 | 2026-03-23 22:33:17 |
|      2 |           2 |   60000.00 |   65000.00 | 2026-03-23 22:34:04 |
|      3 |           3 |   72000.00 |   80000.00 | 2026-03-23 22:34:04 |
|      4 |           5 |   85000.00 |   90000.00 | 2026-03-23 22:34:04 |
+--------+-------------+------------+------------+---------------------+
4 rows in set (0.00 sec)

mysql> SHOW PROCEDURE STATUS WHERE Name = 'UpdateEmployeeSalary'\G
*************************** 1. row ***************************
                  Db: employee_db
                Name: UpdateEmployeeSalary
                Type: PROCEDURE
             Definer: root@localhost
            Modified: 2026-03-23 22:33:04
             Created: 2026-03-23 22:33:04
       Security_type: DEFINER
             Comment: 
character_set_client: utf8mb4
collation_connection: utf8mb4_0900_ai_ci
  Database Collation: utf8mb4_0900_ai_ci
1 row in set (0.00 sec)

mysql> -- DROP PROCEDURE UpdateEmployeeSalary;
mysql> DELIMITER $$
mysql>  
mysql> CREATE FUNCTION CalculateAnnualBonus(p_emp_id INT)
    -> RETURNS DECIMAL(10,2)
    -> DETERMINISTIC
    -> BEGIN
    ->     DECLARE v_salary    DECIMAL(10,2);
    ->     DECLARE v_years     INT;
    ->     DECLARE v_bonus_pct DECIMAL(5,2);
    ->     DECLARE emp_count   INT DEFAULT 0;
    ->  
    ->     -- Check employee exists
    ->     SELECT COUNT(*) INTO emp_count
    ->     FROM Employees WHERE employee_id = p_emp_id;
    ->  
    ->     IF emp_count = 0 THEN
    ->         RETURN NULL;
    ->     END IF;
    ->  
    ->     -- Get salary and years of service
    ->     SELECT salary,
    ->            TIMESTAMPDIFF(YEAR, hire_date, CURDATE())
    ->     INTO   v_salary, v_years
    ->     FROM   Employees WHERE employee_id = p_emp_id;
    ->  
    ->     -- Apply bonus tier
    ->     IF v_years < 5 THEN
    ->         SET v_bonus_pct = 0.10;
    ->     ELSEIF v_years < 10 THEN
    ->         SET v_bonus_pct = 0.15;
    ->     ELSE
    ->         SET v_bonus_pct = 0.20;
    ->     END IF;
    ->  
    ->     RETURN ROUND(v_salary * v_bonus_pct, 2);
    ->  
    -> END$$
Query OK, 0 rows affected (0.01 sec)

mysql>  
mysql> DELIMITER ;
mysql> 
mysql> SELECT
    ->     employee_id,
    ->     CONCAT(first_name,' ',last_name)         AS name,
    ->     salary                                   AS current_salary,
    ->     hire_date,
    ->     TIMESTAMPDIFF(YEAR, hire_date, CURDATE()) AS years_of_service,
    ->     CASE
    ->         WHEN TIMESTAMPDIFF(YEAR,hire_date,CURDATE()) < 5  THEN '10%'
    ->         WHEN TIMESTAMPDIFF(YEAR,hire_date,CURDATE()) < 10 THEN '15%'
    ->         ELSE '20%'
    ->     END                                      AS bonus_tier,
    ->     CalculateAnnualBonus(employee_id)         AS annual_bonus
    -> FROM Employees
    -> ORDER BY employee_id;
+-------------+-------------------+----------------+------------+------------------+------------+--------------+
| employee_id | name              | current_salary | hire_date  | years_of_service | bonus_tier | annual_bonus |
+-------------+-------------------+----------------+------------+------------------+------------+--------------+
|           1 | Mickey Mouse      |       52000.00 | 1928-11-18 |               97 | 20%        |     10400.00 |
|           2 | Elsa Arendelle    |       65000.00 | 2013-11-27 |               12 | 20%        |     13000.00 |
|           3 | Simba Lion        |       80000.00 | 1994-06-24 |               31 | 20%        |     16000.00 |
|           4 | Moana Voyager     |       30000.00 | 2016-11-23 |                9 | 15%        |      4500.00 |
|           5 | Aladdin Streetrat |       90000.00 | 1992-11-25 |               33 | 20%        |     18000.00 |
|           6 | Buzz Lightyear    |       55000.00 | 1995-11-22 |               30 | 20%        |     11000.00 |
+-------------+-------------------+----------------+------------+------------------+------------+--------------+
6 rows in set (0.00 sec)

mysql> SELECT CalculateAnnualBonus(4) AS bonus_for_emp4;
+----------------+
| bonus_for_emp4 |
+----------------+
|        4500.00 |
+----------------+
1 row in set (0.00 sec)

mysql> -- Neha Gupta: 30000 x 10% = 3000.00
mysql> SELECT CalculateAnnualBonus(999) AS result;
+--------+
| result |
+--------+
|   NULL |
+--------+
1 row in set (0.00 sec)

mysql> -- emp_id 999 does not exist -> returns NULL
mysql> SHOW FUNCTION STATUS WHERE Name = 'CalculateAnnualBonus'\G
*************************** 1. row ***************************
                  Db: employee_db
                Name: CalculateAnnualBonus
                Type: FUNCTION
             Definer: root@localhost
            Modified: 2026-03-23 22:35:03
             Created: 2026-03-23 22:35:03
       Security_type: DEFINER
             Comment: 
character_set_client: utf8mb4
collation_connection: utf8mb4_0900_ai_ci
  Database Collation: utf8mb4_0900_ai_ci
1 row in set (0.00 sec)

mysql> -- DROP FUNCTION CalculateAnnualBonus;
mysql> ^C
mysql> 


```

## Q1 — Procedure: UpdateEmployeeSalary

**Parameters:**
- `IN p_emp_id INT`
- `IN p_new_salary DECIMAL`

**Logic:**
1. Check if the employee exists
2. If found: get old salary → UPDATE salary → INSERT into EmployeeSalaryLog → print success
3. If not found: print "Employee not found"

```sql
DELIMITER $$

CREATE PROCEDURE UpdateEmployeeSalary(
    IN p_emp_id INT,
    IN p_new_salary DECIMAL(10, 2)
)
BEGIN
    DECLARE v_old_salary DECIMAL(10, 2);
    DECLARE v_count INT;

    SELECT COUNT(*) INTO v_count FROM Employees WHERE emp_id = p_emp_id;

    IF v_count > 0 THEN
        SELECT salary INTO v_old_salary FROM Employees WHERE emp_id = p_emp_id;

        UPDATE Employees SET salary = p_new_salary WHERE emp_id = p_emp_id;

        INSERT INTO EmployeeSalaryLog (emp_id, old_salary, new_salary, change_date)
        VALUES (p_emp_id, v_old_salary, p_new_salary, CURDATE());

        SELECT CONCAT('Salary updated successfully for employee ID: ', p_emp_id) AS message;
    ELSE
        SELECT 'Employee not found' AS message;
    END IF;
END$$

DELIMITER ;

-- Call the procedure
CALL UpdateEmployeeSalary(101, 75000.00);
```

---

## Q2 — Function: CalculateAnnualBonus

**Parameter:** `IN p_emp_id INT`  
**Returns:** `DECIMAL`

**Logic:**
1. Check if employee exists (return NULL if not found)
2. Get salary and years of experience using `TIMESTAMPDIFF`
3. Calculate bonus percentage:
   - < 5 years → 10%
   - 5–9 years → 15%
   - ≥ 10 years → 20%
4. Return `salary × bonus_percentage`

```sql
DELIMITER $$

CREATE FUNCTION CalculateAnnualBonus(p_emp_id INT)
RETURNS DECIMAL(10, 2)
DETERMINISTIC
BEGIN
    DECLARE v_salary DECIMAL(10, 2);
    DECLARE v_hire_date DATE;
    DECLARE v_years INT;
    DECLARE v_bonus_pct DECIMAL(5, 2);
    DECLARE v_count INT;

    SELECT COUNT(*) INTO v_count FROM Employees WHERE emp_id = p_emp_id;

    IF v_count = 0 THEN
        RETURN NULL;
    END IF;

    SELECT salary, hire_date INTO v_salary, v_hire_date
    FROM Employees WHERE emp_id = p_emp_id;

    SET v_years = TIMESTAMPDIFF(YEAR, v_hire_date, CURDATE());

    IF v_years < 5 THEN
        SET v_bonus_pct = 0.10;
    ELSEIF v_years < 10 THEN
        SET v_bonus_pct = 0.15;
    ELSE
        SET v_bonus_pct = 0.20;
    END IF;

    RETURN v_salary * v_bonus_pct;
END$$

DELIMITER ;

-- Use the function in a SELECT
SELECT emp_id, name, salary, CalculateAnnualBonus(emp_id) AS annual_bonus
FROM Employees;
```

---

## Theory

### Function vs Procedure

| Feature | Stored Function | Stored Procedure |
|---------|----------------|-----------------|
| Returns value | **Must** return a value | Optional (via OUT params) |
| Called via | `SELECT` statement | `CALL` statement |
| Parameters | IN only | IN, OUT, INOUT |
| Use in SQL | Yes (e.g., in `SELECT`, `WHERE`) | No |

### Parameter Modes
| Mode | Description |
|------|-------------|
| `IN` | Input parameter — value passed into the routine (default) |
| `OUT` | Output parameter — value passed back to the caller |
| `INOUT` | Both input and output — value passed in and modified value returned |

### DELIMITER
MySQL uses `;` as its default statement terminator. Since procedure bodies contain `;` inside them, the `DELIMITER` command changes the terminator temporarily so MySQL does not prematurely end the `CREATE PROCEDURE` statement.

```sql
DELIMITER $$

CREATE PROCEDURE my_proc()
BEGIN
    SELECT 'Hello';
END$$

DELIMITER ;
```

### DETERMINISTIC vs NOT DETERMINISTIC
- **DETERMINISTIC**: The function always returns the same result for the same input parameters.
- **NOT DETERMINISTIC**: The function may return different results for the same input (e.g., functions using `RAND()`, `NOW()`).

### Error Handling
```sql
DECLARE CONTINUE HANDLER FOR SQLEXCEPTION
BEGIN
    -- handle error
END;

DECLARE EXIT HANDLER FOR SQLEXCEPTION
BEGIN
    -- exit the block on error
END;
```

---
## FAQs

**Q1. What is the difference between a Function and a Procedure?**
- A **Function** must return a single value and can be used directly in SQL statements (e.g., in `SELECT` or `WHERE`). It is called like a built-in function.
- A **Procedure** does not need to return a value (though it can use OUT/INOUT parameters). It is invoked using the `CALL` statement and cannot be embedded in SQL expressions.

**Q2. What does DETERMINISTIC mean in stored functions?**
A function declared `DETERMINISTIC` guarantees that it always returns the same result for the same input values. This allows MySQL to optimize and cache results. If the function uses `RAND()`, `NOW()`, or other non-deterministic operations, it must be declared `NOT DETERMINISTIC`.

**Q3. Explain IN, OUT, and INOUT parameters in PL/SQL Procedures**
- **IN**: The caller passes a value into the procedure. The procedure cannot modify it in a way that affects the caller. This is the default mode.
- **OUT**: The procedure sets this parameter's value, which is then returned to the caller. The initial value passed in is ignored.
- **INOUT**: The caller passes a value in, the procedure can modify it, and the modified value is returned to the caller.
