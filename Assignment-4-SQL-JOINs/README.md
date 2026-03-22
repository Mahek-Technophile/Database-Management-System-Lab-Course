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

### Implementation:


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
