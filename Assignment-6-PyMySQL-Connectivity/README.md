
# Assignment 6 — PyMySQL Connectivity

## Title
DDL and DML for Educational Institute using Python Frontend + MySQL Backend

## Aim
Connect Python to MySQL using PyMySQL and perform all DDL and DML operations programmatically.

---

## Database (Batch A)

**Educational Institute** — Tables: `students`, `courses`, `faculty`

### Schema
- `students(student_id, name, dept, year, email)`
- `courses(course_id, course_name, credits, dept)`
- `faculty(fac_id, fac_name, dept, salary)`

---

## Implementation of Lab PS
<img width="1129" height="762" alt="image" src="https://github.com/user-attachments/assets/38c29a56-69c5-4d59-bb4e-b6c50b4f3993" />
<img width="1125" height="616" alt="image" src="https://github.com/user-attachments/assets/5a0c6805-7a23-4f44-aab5-29f93dcb3c0b" />
<img width="1116" height="369" alt="image" src="https://github.com/user-attachments/assets/ad1747dd-d117-4e1a-bf74-a7d6f9dc19ac" />

```python
! pip install PyMySQL
```
Collecting PyMySQL
  Downloading pymysql-1.1.2-py3-none-any.whl.metadata (4.3 kB)
Downloading pymysql-1.1.2-py3-none-any.whl (45 kB)
Installing collected packages: PyMySQL
Successfully installed PyMySQL-1.1.2

```python
#importing pymysql
import pymysql
#Creating a connection object
database = pymysql.connect(host = 'localhost',
user = 'root',
password = 'mysqlroot')
#Cursor to the db
cursor = database.cursor()
cursor.execute("CREATE DATABASE democonn7")
print("demconn7 data base is created")
```
demconn1 data base is created

### MySQL:
<img width="488" height="636" alt="image" src="https://github.com/user-attachments/assets/46a1feb5-3dea-415d-8fff-c1d8a73e3052" />


```python
#importing pymysql
import pymysql
#Creating a connection object
database = pymysql.connect(host = 'localhost',
user = 'root',
password = 'mysqlroot',
database = 'democonn7')
#Cursor to the db

cursor = database.cursor()
cursor.execute("CREATE TABLE studA(prn int, name VARCHAR(25), subject VARCHAR(25))")
cursor.close()
database.close()

print("table created")
```
table created

### MySQL:
<img width="902" height="253" alt="image" src="https://github.com/user-attachments/assets/b4ad5c2c-98ca-44bb-8cd8-edbd207dfdbe" />


```python
#importing pymysql
import pymysql
#Creating a connection object
database = pymysql.connect(host = 'localhost',
user = 'root',
password ='mysqlroot',
database = 'democonn7')
#Cursor to the db
#Creating a cursor object
cursor = database.cursor()
sql = "INSERT INTO studA(prn, name, subject) VALUES (%s,%s,%s)"
val=[("102", "ankush", "PBL3"), ("103", "yash", "DBMS"), ("104","shambhavee", "DAA")]
cursor.executemany(sql,val)
database.commit()
print(cursor.rowcount, "record inserted.")
```

3 record inserted.

### MYSQL
<img width="568" height="332" alt="image" src="https://github.com/user-attachments/assets/df93d3e6-819d-4d0b-9b31-04d27134c261" />

```python
#importing pymysql
import pymysql
#Creating a connection object
database = pymysql.connect(host = 'localhost',
user = 'root',
password = 'mysqlroot',
database = 'democonn7')
#Cursor to the db
#Creating a cursor object
cursor = database.cursor()
cursor.execute("DELETE FROM studA WHERE name = 'yash' ")
database.commit()
print(cursor.rowcount, "record inserted.")
```
1 record inserted.

## Mysql 
<img width="567" height="256" alt="image" src="https://github.com/user-attachments/assets/9a9616d3-dac6-4e87-a631-30adf733f3ea" />

```python
#importing pymysql
import pymysql
#Creating a connection object
database = pymysql.connect(host = 'localhost',
user = 'root',
password = 'mysqlroot',
database = 'democonn7')
#Cursor to the db
#Creating a cursor object
cursor = database.cursor()
cursor.execute(" SELECT * FROM studA")
#printing the results
results = cursor.fetchall()
for row in results:
    print (row)
```
(102, 'ankush', 'PBL3')
(104, 'shambhavee', 'DAA')


## implementation of batch A PS 

## STEP 1: Install PyMySQL

```bash
pip install PyMySQL
```

---
## Step 2: Use connect method With 3 parameters:
* Host 
* User name 
* Password


# Step 3: Create connection object
database = pymysql.connect(
    host="localhost",
    user="root",
    password="your_password",   # <-- change to your MySQL password
    database="edu_db"
)
print("Connection established successfully!")


## Theory

## Setup

### Step 1: Install PyMySQL
```bash
pip install PyMySQL
```

### Step 2: Import the module
```python
import pymysql
```

### Step 3: Create a connection
```python
conn = pymysql.connect(
    host="localhost",
    user="root",
    password="your_password",
    database="educational_institute"
)
```

### Step 4: Create a cursor
```python
cursor = conn.cursor()
```

### Step 5: Execute queries
```python
cursor.execute(sql, params)
```

### Step 6: Close the connection
```python
conn.close()
```

---

## extra info

| Concept | Description |
|--------|-------------|
| `%s` placeholders | Parameterized queries that prevent SQL injection |
| `conn.commit()` | Must be called after every DML statement to make changes permanent |
| `cursor` | Executes queries + stores result sets |
| `fetchall()` | Retrieves all rows from the last executed `SELECT` |
| `fetchone()` | Retrieves one row from the result set |
| `executemany()` | Inserts/updates multiple rows in one call |

---

## FAQs

### Q1. Why use `%s` placeholders instead of string formatting?
Using `%s` with parameterized queries prevents SQL injection. Avoid f-strings or `.format()` for queries containing user input.

### Q2. Why must `conn.commit()` be called after DML operations?
PyMySQL does not auto-commit by default. Without `commit()`, changes won’t be permanently saved.

### Q3. What is the role of the cursor?
The cursor:
- Sends SQL to MySQL
- Stores result sets for `SELECT`
- Provides `execute()`, `executemany()`, `fetchall()`, `fetchone()`
```
