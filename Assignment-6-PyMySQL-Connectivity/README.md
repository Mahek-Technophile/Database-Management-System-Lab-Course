# Assignment 6 — PyMySQL Connectivity

## Title
DDL and DML for Educational Institute using Python Frontend + MySQL Backend

## Aim
Connect Python to MySQL using PyMySQL and perform all DDL and DML operations programmatically.

## Database (Batch A)
**Educational Institute** — Tables: `students`, `courses`, `faculty`

---

## Theory

### Setup

**Step 1: Install PyMySQL**
```bash
pip install PyMySQL
```

**Step 2: Import the module**
```python
import pymysql
```

**Step 3: Create a connection**
```python
conn = pymysql.connect(
    host='localhost',
    user='root',
    password='your_password',
    database='educational_institute'
)
```

**Step 4: Create a cursor**
```python
cursor = conn.cursor()
```

**Step 5: Execute queries**
```python
cursor.execute(sql, params)
```

**Step 6: Close the connection**
```python
conn.close()
```

---

## Key Concepts

| Concept | Description |
|---------|-------------|
| `%s` placeholders | Parameterized queries that prevent SQL injection |
| `conn.commit()` | Must be called after every DML statement to make changes permanent |
| `cursor` | The execution engine — sends queries to MySQL and retrieves results |
| `fetchall()` | Retrieves all rows from the last executed SELECT |
| `fetchone()` | Retrieves the next single row from the result set |
| `executemany()` | Inserts/updates multiple rows in one call |

---

## Operations to Demonstrate

### DDL from Python

```python
# CREATE TABLE
cursor.execute("""
    CREATE TABLE IF NOT EXISTS students (
        student_id INT AUTO_INCREMENT PRIMARY KEY,
        name VARCHAR(100) NOT NULL,
        email VARCHAR(150) UNIQUE,
        course_id INT,
        enrollment_date DATE
    )
""")
conn.commit()

# ALTER TABLE
cursor.execute("ALTER TABLE students ADD COLUMN phone VARCHAR(15)")
conn.commit()

# DROP TABLE
cursor.execute("DROP TABLE IF EXISTS students")
conn.commit()
```

### DML from Python

```python
# INSERT (single row)
cursor.execute(
    "INSERT INTO students (name, email, course_id) VALUES (%s, %s, %s)",
    ('Alice', 'alice@example.com', 1)
)
conn.commit()

# INSERT (multiple rows)
students = [
    ('Bob', 'bob@example.com', 2),
    ('Carol', 'carol@example.com', 1),
]
cursor.executemany(
    "INSERT INTO students (name, email, course_id) VALUES (%s, %s, %s)",
    students
)
conn.commit()

# SELECT (all rows)
cursor.execute("SELECT * FROM students")
rows = cursor.fetchall()
for row in rows:
    print(row)

# SELECT (single row with parameterized WHERE)
cursor.execute("SELECT * FROM students WHERE student_id = %s", (1,))
row = cursor.fetchone()
print(row)

# UPDATE
cursor.execute(
    "UPDATE students SET email = %s WHERE student_id = %s",
    ('newemail@example.com', 1)
)
conn.commit()

# DELETE
cursor.execute("DELETE FROM students WHERE student_id = %s", (1,))
conn.commit()
```

---

## FAQs

**Q1. Why use `%s` placeholders instead of string formatting?**
Using `%s` with parameterized queries prevents **SQL injection** — a common security vulnerability. Never use f-strings or `.format()` to build SQL queries with user input.

**Q2. Why must `conn.commit()` be called after DML operations?**
PyMySQL does not auto-commit by default. Without `conn.commit()`, INSERT, UPDATE, and DELETE changes are held in a transaction and not written to the database permanently. They will be lost when the connection closes.

**Q3. What is the role of the cursor?**
The cursor is the execution engine that:
- Sends SQL statements to the MySQL server
- Holds the result set after a SELECT
- Provides methods like `execute()`, `executemany()`, `fetchall()`, `fetchone()`
A single connection can have multiple cursors open simultaneously.
