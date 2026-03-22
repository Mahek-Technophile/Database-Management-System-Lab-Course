# Assignment 2 — DDL + DCL Commands

## Title
SQL DDL (CREATE, ALTER, DROP, TRUNCATE, RENAME, DESC) and DCL (GRANT, REVOKE)

## Aim
Execute SQL DDL statements for different databases.

## Objective
Study DDL (Data Definition Language) and DCL (Data Control Language) commands.

## Theory

### DDL Commands

| Command | Description |
|---------|-------------|
| `CREATE TABLE` | Creates a new table |
| `ALTER TABLE` | Modifies an existing table structure |
| `DROP TABLE` | Permanently deletes a table and its data |
| `TRUNCATE` | Removes all rows from a table (faster than DELETE) |
| `RENAME` | Renames a table |
| `DESC` | Displays the structure of a table |

#### ALTER TABLE Operations
```sql
ALTER TABLE table_name ADD column_name datatype;
ALTER TABLE table_name DROP COLUMN column_name;
ALTER TABLE table_name MODIFY column_name new_datatype;
ALTER TABLE table_name RENAME TO new_table_name;
ALTER TABLE table_name RENAME COLUMN old_name TO new_name;
```

### Constraints
| Constraint | Description |
|------------|-------------|
| `NOT NULL` | Column cannot have NULL values |
| `UNIQUE` | All values in the column must be distinct |
| `PRIMARY KEY` | Uniquely identifies each row; combines NOT NULL + UNIQUE |
| `FOREIGN KEY` | Links two tables; enforces referential integrity |
| `CHECK` | Ensures values in a column satisfy a condition |
| `DEFAULT` | Sets a default value when none is provided |

### DCL Commands

```sql
-- Grant privileges
GRANT SELECT, INSERT, UPDATE, DELETE ON table_name TO 'username'@'host';
GRANT ALL PRIVILEGES ON table_name TO 'username'@'host';

-- Revoke privileges
REVOKE SELECT, INSERT ON table_name FROM 'username'@'host';
```

---

## Exercises

### Batch A
- **Schema 1**: Suppliers, Parts, Projects, Shipments (SPJ)
- **Schema 2**: Employee, Works, Company, Manages

### Batch B
- **Schema 1**: Hotel, Room, Booking, Guest
- **Schema 2**: Mail Order (emp, parts, customer, order, odetails, zipcode)

### Batch C
- **Schema 1**: Project, Employee, Assigned-To
- **Schema 2**: Employee, Position, Duty_allocation

### Batch D
- **Schema 1**: Bank (Account, Branch, Customer, Depositor, Loan, Borrower)
- **Schema 2**: Emp, Dept

### Full Task List (per batch)
1. Create all tables with appropriate constraints
2. TRUNCATE a table
3. DROP a table
4. ALTER — add column
5. ALTER — drop column
6. ALTER — add constraint
7. ALTER — drop constraint
8. ALTER — modify datatype
9. ALTER — rename table
10. ALTER — rename column
11. CREATE USER
12. GRANT privileges to user
13. REVOKE privileges from user

---

## FAQs

**Q1. How do you drop a column from a table?**
```sql
ALTER TABLE table_name DROP COLUMN column_name;
```

**Q2. How do you add a primary key to an existing table?**
```sql
ALTER TABLE table_name ADD PRIMARY KEY (column_name);
```

**Q3. How do you create a new user in MySQL?**
```sql
CREATE USER 'username'@'host' IDENTIFIED BY 'password';
```
After creating, grant privileges using the `GRANT` command.
