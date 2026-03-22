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
