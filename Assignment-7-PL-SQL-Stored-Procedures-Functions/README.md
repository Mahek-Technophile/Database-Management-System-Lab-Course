# Assignment 7 — PL/SQL Stored Procedures & Functions

## Title
Stored Procedures and Functions in PL/SQL

## Aim
Write PL/SQL Procedures and Functions for given problem statements.

## Database (Batch A)
- `Employees` table
- `EmployeeSalaryLog` table

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
