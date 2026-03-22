# Assignment 3 — DML + SELECT Operators

## Title
SQL DML (INSERT, UPDATE, DELETE) and SELECT with WHERE Clause and Operators

## Aim
Write DML and SELECT commands to manipulate and retrieve data.

## Objective
Study INSERT, UPDATE, DELETE and logical operators: IN, Negation, NULL, Comparison, BETWEEN, EXISTS, ALL, LIKE.

## Database
**COMPANY Database** — EMP and DEPT tables (classic Oracle practice dataset)

```sql
-- EMP table columns: EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO
-- DEPT table columns: DEPTNO, DNAME, LOC
```

---

## Theory

### DML Commands

```sql
-- INSERT (with column list)
INSERT INTO table_name (col1, col2, ...) VALUES (val1, val2, ...);

-- INSERT (without column list)
INSERT INTO table_name VALUES (val1, val2, ...);

-- INSERT NULL explicitly
INSERT INTO table_name (col1, col2) VALUES (val1, NULL);

-- UPDATE
UPDATE table_name SET column = value WHERE condition;

-- DELETE
DELETE FROM table_name WHERE condition;
```

### SELECT Clause Order
```sql
SELECT   -- mandatory
FROM     -- mandatory
WHERE    -- optional: filter rows before grouping
GROUP BY -- optional: group rows
HAVING   -- optional: filter groups
ORDER BY -- optional: sort results
```

### Arithmetic Operators in SELECT
```sql
SELECT ENAME, SAL, SAL * 12 AS ANNUAL_SAL FROM EMP;
```

### String Functions
| Function | Usage |
|----------|-------|
| `UPPER(str)` | Convert to uppercase |
| `LOWER(str)` | Convert to lowercase |
| `CONCAT(s1, s2)` | Concatenate strings |
| `SUBSTR(str, pos, len)` | Extract substring |
| `LENGTH(str)` | String length |
| `TRIM(str)` | Remove leading/trailing spaces |
| `REPLACE(str, old, new)` | Replace occurrences |
| `LPAD(str, len, pad)` | Left-pad string |
| `LOCATE(substr, str)` | Find substring position |

### Date Functions
| Function | Usage |
|----------|-------|
| `CURDATE()` | Current date |
| `NOW()` | Current date and time |
| `DATEDIFF(d1, d2)` | Days between two dates |
| `DATE_ADD(date, INTERVAL n UNIT)` | Add interval to date |
| `DAYNAME(date)` | Day name (e.g., Monday) |
| `DATE_FORMAT(date, format)` | Format a date |

### Operators
```sql
-- NULL handling
WHERE column IS NULL
WHERE column IS NOT NULL

-- BETWEEN
WHERE SAL BETWEEN 1000 AND 3000;

-- LIKE (% = any chars, _ = one char)
WHERE ENAME LIKE 'S%';       -- starts with S
WHERE ENAME LIKE '%SON';     -- ends with SON
WHERE ENAME LIKE '_A%';      -- second char is A

-- IN / NOT IN
WHERE DEPTNO IN (10, 20, 30);
WHERE JOB NOT IN ('MANAGER', 'PRESIDENT');

-- EXISTS
WHERE EXISTS (SELECT 1 FROM DEPT WHERE DEPT.DEPTNO = EMP.DEPTNO);

-- ALL
WHERE SAL > ALL (SELECT SAL FROM EMP WHERE JOB = 'CLERK');

-- DISTINCT
SELECT DISTINCT JOB FROM EMP;
```

---

## Exercises

### Set 3 (Batch B)
- Unique designations
- Delete employees by hire year
- Salary updates
- NOT IN, LIKE patterns, IS NULL
- COUNT, DATE_FORMAT, BETWEEN

### Set 4 (Batch A)
- Employees with >2 years of service
- ORDER BY salary
- Subquery comparing salary to average
- UPDATE by employee number
- BETWEEN for salary range
- IS NULL for commission
- Salary increment by job type
- GROUP BY job for aggregate functions
- DELETE employees whose name starts with 'P'
- Clerks with commission > 500
- IN (20, 30, 40) for department filter

### Set 5 (Batch C)
- Annual salary job-wise
- Delete by name pattern
- Salary increment
- Aggregate by hire date
- LIKE for last letter
- Subquery (less than)
- BETWEEN, IN (30, 40, 10)
- WHERE with multiple conditions

### Set 6 (Batch D)
- ALTER ADD columns
- CREATE new table
- Duty allocation queries
- Subquery with ANY
- MIN pay rate
- JOIN-style queries
- GROUP BY shift
- UPDATE, CREATE VIEW

---

## FAQs

**Q1. What is the difference between TRUNCATE and DROP?**
- **TRUNCATE**: Removes all rows but keeps the table structure. It is a DDL command and cannot be rolled back (in most databases).
- **DROP**: Removes the entire table including its structure, indexes, and constraints. Data cannot be recovered.

**Q2. How is pattern matching done in SQL using LIKE?**
- `%` matches zero or more characters
- `_` matches exactly one character
- Example: `LIKE 'A%'` matches any string starting with A; `LIKE '_AT'` matches 3-character strings ending in AT.

**Q3. Compare EXISTS, ALL, and LIKE**
- **EXISTS**: Returns TRUE if the subquery returns at least one row.
- **ALL**: Compares a value to all values returned by a subquery (e.g., `> ALL` means greater than every value).
- **LIKE**: Used for pattern matching against string values using `%` and `_` wildcards.
