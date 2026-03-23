# Assignment 6 — PyMySQL Connectivity

## Title
DDL and DML for Educational Institute using Python Frontend + MySQL Backend

## Aim
Connect Python to MySQL using PyMySQL and perform all DDL and DML operations programmatically.

## Database (Batch A)
**Educational Institute** — Tables: `students`, `courses`, `faculty`

---
students(student_id, name, dept, year, email)
courses (course_id, course_name, credits, dept)
faculty (fac_id, fac_name, dept, salary)
 
### STEP 1    INSTALL PyMySQL
```
pip install PyMySQL
Collecting PyMySQL
  Downloading PyMySQL-1.1.0-py3-none-any.whl (44 kB)
Installing collected packages: PyMySQL
Successfully installed PyMySQL-1.1.0
 ```
### STEP 2    MySQL BACKEND — CREATE DATABASE & TABLES
```sql
CREATE DATABASE edu_db;
USE edu_db;
 
CREATE TABLE students (
    student_id INT          PRIMARY KEY AUTO_INCREMENT,
    name       VARCHAR(100) NOT NULL,
    dept       VARCHAR(50),
    year       INT          CHECK (year BETWEEN 1 AND 4),
    email      VARCHAR(100) UNIQUE
);
 
CREATE TABLE courses (
    course_id   INT          PRIMARY KEY AUTO_INCREMENT,
    course_name VARCHAR(100) NOT NULL,
    credits     INT,
    dept        VARCHAR(50)
);
 
CREATE TABLE faculty (
    fac_id   INT          PRIMARY KEY AUTO_INCREMENT,
    fac_name VARCHAR(100) NOT NULL,
    dept     VARCHAR(50),
    salary   DECIMAL(10,2)
);
Database changed
Query OK, 0 rows affected  -- students
Query OK, 0 rows affected  -- courses
Query OK, 0 rows affected  -- faculty
``` 
### STEP 3    PYTHON SCRIPT — CONNECTION + ALL DDL + DML OPERATIONS
```
connect_edu.py — Full Script
import pymysql
```
# ── Step 3: Create connection object ──────────────────────────────────
```
conn = pymysql.connect(
    host     = 'localhost',
    user     = 'root',
    password = 'your_password',   # <-- change to your MySQL password
    database = 'edu_db'
)
print('Connection established successfully!')
``` 
# ── Step 4: Create cursor object ──────────────────────────────────────
```
cursor = conn.cursor()
 ```

# DDL: CREATE TABLE (Python-side, alternative approach)
```
cursor.execute('''
    CREATE TABLE IF NOT EXISTS enrollments (
        enroll_id  INT PRIMARY KEY AUTO_INCREMENT,
        student_id INT,
        course_id  INT,
        enroll_date DATE
    )
''')
print('DDL: Table enrollments created')
 ```
# 🏰 Disney Database Demo (DML + DDL using Python & MySQL)

# ───────────────────────────────────────────────────────────────
# DML: INSERT — add characters (students → Disney characters)
# ───────────────────────────────────────────────────────────────
characters_data = [
    ('Mickey Mouse',     'Magic Kingdom', 2, 'mickey@disney.com'),
    ('Elsa',             'Frozen',        3, 'elsa@disney.com'),
    ('Simba',            'Lion King',     1, 'simba@disney.com'),
    ('Moana',            'Ocean',         4, 'moana@disney.com'),
    ('Buzz Lightyear',   'Toy Story',     2, 'buzz@disney.com'),
]

cursor.executemany(
    'INSERT INTO characters (name, world, level, email) VALUES (%s, %s, %s, %s)',
    characters_data
)
conn.commit()
print(f'DML INSERT: {cursor.rowcount} characters inserted')


# INSERT movies   (courses → Disney movies)
```
movies_data = [
    ('Frozen',        4, 'Fantasy'),
    ('Toy Story',     3, 'Adventure'),
    ('Lion King',     4, 'Drama'),
    ('Moana',         3, 'Adventure'),
]

cursor.executemany(
    'INSERT INTO movies (movie_name, rating, genre) VALUES (%s, %s, %s)',
    movies_data
)
conn.commit()
print(f'DML INSERT: {cursor.rowcount} movies inserted')

```
# INSERT creators  (faculty → Disney creators)
```
cursor.execute(
    'INSERT INTO creators (creator_name, specialty, salary) VALUES (%s, %s, %s)',
    ('Walt Disney', 'Animation', 100000.00)
)
conn.commit()
print('DML INSERT: 1 creator inserted')

```
# DML: SELECT — fetch and display all characters
```
cursor.execute('SELECT * FROM characters ORDER BY character_id')
rows = cursor.fetchall()

print('\nDML SELECT: All Characters')
print('-' * 60)
for row in rows:
    print(row)


# Fetch one row at a time
cursor.execute('SELECT * FROM movies')
print('\nDML SELECT (fetchone): First Movie')
one = cursor.fetchone()
print(one)


# SELECT with WHERE + parameterized query
cursor.execute('SELECT * FROM characters WHERE world = %s', ('Magic Kingdom',))
magic_chars = cursor.fetchall()

print(f'\nMagic Kingdom characters: {len(magic_chars)}')
for c in magic_chars:
    print(c)
```

# DML: UPDATE — level up a character
```
cursor.execute(
    'UPDATE characters SET level = level + 1 WHERE name = %s',
    ('Mickey Mouse',)
)
conn.commit()
print(f'\nDML UPDATE: {cursor.rowcount} row(s) updated')


# Verify update
cursor.execute(
    'SELECT character_id, name, level FROM characters WHERE name = %s',
    ('Mickey Mouse',)
)
print('After update:', cursor.fetchone())

```
# DML: DELETE — remove a character
```
cursor.execute('DELETE FROM characters WHERE name = %s', ('Simba',))
conn.commit()
print(f'\nDML DELETE: {cursor.rowcount} row(s) deleted')


# Verify remaining count
cursor.execute('SELECT COUNT(*) FROM characters')
count = cursor.fetchone()[0]
print(f'Remaining characters: {count}')
```

# DDL: ALTER TABLE — add a new column
```
cursor.execute('ALTER TABLE characters ADD COLUMN magic_power VARCHAR(50)')
print('\nDDL ALTER: column magic_power added to characters')
```

# DDL: DROP TABLE (cleanup)
```
cursor.execute('DROP TABLE IF EXISTS adventures')
print('DDL DROP: adventures table dropped')

```
# Close connection
```
cursor.close()
conn.close()
print('\nConnection closed successfully.')

```
# Output


"""
Connection established successfully!
DDL: Table adventures created
DML INSERT: 5 characters inserted
DML INSERT: 4 movies inserted
DML INSERT: 1 creator inserted

DML SELECT: All Characters
------------------------------------------------------------
(1, 'Mickey Mouse', 'Magic Kingdom', 2, 'mickey@disney.com')
(2, 'Elsa', 'Frozen', 3, 'elsa@disney.com')
(3, 'Simba', 'Lion King', 1, 'simba@disney.com')
(4, 'Moana', 'Ocean', 4, 'moana@disney.com')
(5, 'Buzz Lightyear', 'Toy Story', 2, 'buzz@disney.com')

DML SELECT (fetchone): First Movie
(1, 'Frozen', 4, 'Fantasy')

Magic Kingdom characters: 1
(1, 'Mickey Mouse', 'Magic Kingdom', 2, 'mickey@disney.com')

DML UPDATE: 1 row(s) updated
After update: (1, 'Mickey Mouse', 3)

DML DELETE: 1 row(s) deleted
Remaining characters: 4

DDL ALTER: column magic_power added to characters
DDL DROP: adventures table dropped

Connection closed successfully.
"""

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
