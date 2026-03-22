# Assignment 2 — DDL + DCL Commands

## Title
SQL DDL (CREATE, DESC) and DCL (GRANT, REVOKE)

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
### Batch C
- **Schema 1**: Project, Employee, Assigned-To
- **Schema 2**: Employee, Position, Duty_allocation

## LIST OF ALL COMMANDS USED
#### Database & Table
```sql
CREATE DATABASE Bank;
USE Bank;
CREATE TABLE Customers(id INT, name VARCHAR(10));
DESC Customers;
SHOW DATABASES;
SHOW TABLES;
```
#### Data Operations
```sql
INSERT INTO Customers VALUES (...);
SELECT * FROM Customers;
```
#### User Management
```sql
CREATE USER 'BatchE1'@'localhost' IDENTIFIED BY 'e@123';
SHOW GRANTS FOR BatchE1@localhost;
```

#### Privilege Control
```sql
GRANT INSERT ON Bank.Customers TO BatchE1@localhost;
GRANT SELECT, UPDATE, DELETE ON Bank.Customers TO BatchE1@localhost;
REVOKE INSERT, SELECT, UPDATE, DELETE ON Bank.Customers FROM BatchE1@localhost;
```  
  #### Access Test Commands
```sql
SELECT user FROM mysql.user; ❌ (Denied for BatchE1)
SELECT * FROM Customers; ✅ / ❌ (based on grants)
```
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



### IMPLEMENTATION

## LAB Implementation
```sql

Last login: Tue Jan 27 10:50:20 on ttys000
(base) vyaas124@VY030-36 ~ % mysql -u root
ERROR 1045 (28000): Access denied for user 'root'@'localhost' (using password: NO)
(base) vyaas124@VY030-36 ~ % mysql -u root -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 15
Server version: 9.3.0-commercial MySQL Enterprise Server - Commercial
Copyright (c) 2000, 2018, Oracle and/or its affiliates. All rights reserved.
Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.
Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
mysql> create database Bank;
Query OK, 1 row affected (0.01 sec)
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| 21Jan              |
| author             |
| Bank               |
| College            |
| dbms               |
| f12                |
| information_schema |
| mysql              |
| performance_schema |
| sys                |
| table1             |
+--------------------+
11 rows in set (0.00 sec)
mysql> use Bank;
Database changed
mysql> create table Customers(id int, name varchar(10));
Query OK, 0 rows affected (0.01 sec)
mysql> show tables;
+----------------+
| Tables_in_bank |
+----------------+
| Customers      |
+----------------+
1 row in set (0.00 sec)
mysql> desc Customers;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| id    | int         | YES  |     | NULL    |       |
| name  | varchar(10) | YES  |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
2 rows in set (0.00 sec)
mysql> insert into Customers values(100, "Tom"),(101, "Jerry");
Query OK, 2 rows affected (0.00 sec)
Records: 2  Duplicates: 0  Warnings: 0
mysql> desc Customers;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| id    | int         | YES  |     | NULL    |       |
| name  | varchar(10) | YES  |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
2 rows in set (0.00 sec)
mysql> show tables;
+----------------+
| Tables_in_bank |
+----------------+
| Customers      |
+----------------+
1 row in set (0.00 sec)
mysql> select * from Customers;
+------+-------+
| id   | name  |
+------+-------+
|  100 | Tom   |
|  101 | Jerry |
+------+-------+
2 rows in set (0.00 sec)
mysql> select user from mysql.user
    -> ^C
mysql> select user from mysql.user;
+------------------+
| user             |
+------------------+
| baraksis         |
| mysql.infoschema |
| mysql.session    |
| mysql.sys        |
| root             |
+------------------+
5 rows in set (0.00 sec)
mysql> selectuser();
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'selectuser()' at line 1
mysql> select user();
+----------------+
| user()         |
+----------------+
| root@localhost |
+----------------+
1 row in set (0.01 sec)
mysql> create user 'PanelE'@'localhost'
    -> ^C
mysql> create user 'PanelE'@'localhost';
Query OK, 0 rows affected (0.04 sec)
mysql> create user 'PanelE'@'localhost' identified by 'panelE@123';
ERROR 1396 (HY000): Operation CREATE USER failed for 'PanelE'@'localhost'
mysql> delete user 'PanelE'@'localhost';
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near ''PanelE'@'localhost'' at line 1
mysql> select user();^C
mysql> create user 'BatchE1'@'localhost' identified by 'e@123';
Query OK, 0 rows affected (0.01 sec)
mysql> select user from mysql.user
    -> ^C
mysql> select user from mysql.user;
+------------------+
| user             |
+------------------+
| baraksis         |
| BatchE1          |
| PanelE           |
| mysql.infoschema |
| mysql.session    |
| mysql.sys        |
| root             |
+------------------+
7 rows in set (0.01 sec)
mysql> select user;
ERROR 1054 (42S22): Unknown column 'user' in 'field list'
mysql> select user();
+----------------+
| user()         |
+----------------+
| root@localhost |
+----------------+
1 row in set (0.00 sec)
mysql> show grants for BatchE1;
ERROR 1141 (42000): There is no such grant defined for user 'BatchE1' on host '%'
mysql> show grants for BatchE1@localhost;
+---------------------------------------------+
| Grants for BatchE1@localhost                |
+---------------------------------------------+
| GRANT USAGE ON *.* TO `BatchE1`@`localhost` |
+---------------------------------------------+
1 row in set (0.00 sec)
mysql> grant insert on Bank.Customers to BatchE1@localhost;
Query OK, 0 rows affected (0.01 sec)
mysql> grant all on Banks.Customers to BatchE1@localhost;
Query OK, 0 rows affected (0.00 sec)
mysql> exit root
    -> ^C
mysql> exit root;
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'exit root' at line 1
mysql> exit;
Bye
(base) vyaas124@VY030-36 ~ % mysql -u BatchE1 -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 16
Server version: 9.3.0-commercial MySQL Enterprise Server - Commercial
Copyright (c) 2000, 2018, Oracle and/or its affiliates. All rights reserved.
Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.
Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| Bank               |
| information_schema |
| performance_schema |
+--------------------+
3 rows in set (0.02 sec)
mysql> use Bank;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A
Database changed
mysql> show tables;
+----------------+
| Tables_in_bank |
+----------------+
| Customers      |
+----------------+
1 row in set (0.00 sec)
mysql> desc Customers;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| id    | int         | YES  |     | NULL    |       |
| name  | varchar(10) | YES  |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
2 rows in set (0.01 sec)
mysql> insert into Customers valus(102, "Chip");
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'valus(102, "Chip")' at line 1
mysql> insert into Customers values(102, "Chip");
Query OK, 1 row affected (0.01 sec)
mysql> show tables;
+----------------+
| Tables_in_bank |
+----------------+
| Customers      |
+----------------+
1 row in set (0.01 sec)
mysql> select * from Customers;
ERROR 1142 (42000): SELECT command denied to user 'BatchE1'@'localhost' for table 'customers'
mysql> exit
Bye
(base) vyaas124@VY030-36 ~ % mysql -u root -p  
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 17
Server version: 9.3.0-commercial MySQL Enterprise Server - Commercial
Copyright (c) 2000, 2018, Oracle and/or its affiliates. All rights reserved.
Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.
Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
mysql> select * from Customers;
ERROR 1046 (3D000): No database selected
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| 21Jan              |
| author             |
| Bank               |
| College            |
| dbms               |
| f12                |
| information_schema |
| mysql              |
| performance_schema |
| sys                |
| table1             |
+--------------------+
11 rows in set (0.00 sec)
mysql> use Bank;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A
Database changed
mysql> select * from Customers;
+------+-------+
| id   | name  |
+------+-------+
|  100 | Tom   |
|  101 | Jerry |
|  102 | Chip  |
+------+-------+
3 rows in set (0.00 sec)
mysql> grant select,update,delete on Bank.Customers to BatchE1@localhost;
Query OK, 0 rows affected (0.01 sec)
mysql> exit
Bye
(base) vyaas124@VY030-36 ~ % mysql -u BatchE1 -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 18
Server version: 9.3.0-commercial MySQL Enterprise Server - Commercial
Copyright (c) 2000, 2018, Oracle and/or its affiliates. All rights reserved.
Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.
Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| Bank               |
| information_schema |
| performance_schema |
+--------------------+
3 rows in set (0.01 sec)
mysql> use Bank;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A
Database changed
mysql> select * from Customers;
+------+-------+
| id   | name  |
+------+-------+
|  100 | Tom   |
|  101 | Jerry |
|  102 | Chip  |
+------+-------+
3 rows in set (0.00 sec)
mysql> select user from mysql.user
    -> ^C
mysql> select user from mysql.user;
ERROR 1142 (42000): SELECT command denied to user 'BatchE1'@'localhost' for table 'user'
mysql> exit
Bye
(base) vyaas124@VY030-36 ~ % mysql -u root -p   
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 19
Server version: 9.3.0-commercial MySQL Enterprise Server - Commercial
Copyright (c) 2000, 2018, Oracle and/or its affiliates. All rights reserved.
Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.
Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| 21Jan              |
| author             |
| Bank               |
| College            |
| dbms               |
| f12                |
| information_schema |
| mysql              |
| performance_schema |
| sys                |
| table1             |
+--------------------+
11 rows in set (0.00 sec)
mysql> use mysql;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A
Database changed
mysql> show tables;
+------------------------------------------------------+
| Tables_in_mysql                                      |
+------------------------------------------------------+
| columns_priv                                         |
| component                                            |
| db                                                   |
| default_roles                                        |
| engine_cost                                          |
| func                                                 |
| general_log                                          |
| global_grants                                        |
| gtid_executed                                        |
| help_category                                        |
| help_keyword                                         |
| help_relation                                        |
| help_topic                                           |
| innodb_index_stats                                   |
| innodb_table_stats                                   |
| ndb_binlog_index                                     |
| password_history                                     |
| plugin                                               |
| procs_priv                                           |
| proxies_priv                                         |
| replication_asynchronous_connection_failover         |
| replication_asynchronous_connection_failover_managed |
| replication_group_configuration_version              |
| replication_group_member_actions                     |
| role_edges                                           |
| server_cost                                          |
| servers                                              |
| slave_master_info                                    |
| slave_relay_log_info                                 |
| slave_worker_info                                    |
| slow_log                                             |
| tables_priv                                          |
| time_zone                                            |
| time_zone_leap_second                                |
| time_zone_name                                       |
| time_zone_transition                                 |
| time_zone_transition_type                            |
| user                                                 |
+------------------------------------------------------+
38 rows in set (0.00 sec)
mysql> select * from user;
+-----------+------------------+-------------+-------------+-------------+-------------+-------------+-----------+-------------+---------------+--------------+-----------+------------+-----------------+------------+------------+--------------+------------+-----------------------+------------------+--------------+-----------------+------------------+------------------+----------------+---------------------+--------------------+------------------+------------+--------------+------------------------+----------+------------+-------------+--------------+---------------+-------------+-----------------+----------------------+-----------------------+------------------------------------------------------------------------+------------------+-----------------------+-------------------+----------------+------------------+----------------+------------------------+---------------------+--------------------------+-----------------+
| Host      | User             | Select_priv | Insert_priv | Update_priv | Delete_priv | Create_priv | Drop_priv | Reload_priv | Shutdown_priv | Process_priv | File_priv | Grant_priv | References_priv | Index_priv | Alter_priv | Show_db_priv | Super_priv | Create_tmp_table_priv | Lock_tables_priv | Execute_priv | Repl_slave_priv | Repl_client_priv | Create_view_priv | Show_view_priv | Create_routine_priv | Alter_routine_priv | Create_user_priv | Event_priv | Trigger_priv | Create_tablespace_priv | ssl_type | ssl_cipher | x509_issuer | x509_subject | max_questions | max_updates | max_connections | max_user_connections | plugin                | authentication_string                                                  | password_expired | password_last_changed | password_lifetime | account_locked | Create_role_priv | Drop_role_priv | Password_reuse_history | Password_reuse_time | Password_require_current | User_attributes |
+-----------+------------------+-------------+-------------+-------------+-------------+-------------+-----------+-------------+---------------+--------------+-----------+------------+-----------------+------------+------------+--------------+------------+-----------------------+------------------+--------------+-----------------+------------------+------------------+----------------+---------------------+--------------------+------------------+------------+--------------+------------------------+----------+------------+-------------+--------------+---------------+-------------+-----------------+----------------------+-----------------------+------------------------------------------------------------------------+------------------+-----------------------+-------------------+----------------+------------------+----------------+------------------------+---------------------+--------------------------+-----------------+
| %         | baraksis         | N           | N           | N           | N           | N           | N         | N           | N             | N            | N         | N          | N               | N          | N          | N            | N          | N                     | N                | N            | N               | N                | N                | N              | N                   | N                  | N                | N          | N            | N                      |          |            |             |              |             0 |           0 |               0 |                    0 | caching_sha2_password | $A$005$`^E8t^tN_:C
<iMvYOEml1cU4qBpVHNhDm0BoGdRrVnFowXzWa7fwm7k5 | N                | 2026-01-27 10:06:39   |              NULL | N              | N                | N              |                   NULL |                NULL | NULL                     | NULL            |
| localhost | BatchE1          | N           | N           | N           | N           | N           | N         | N           | N             | N            | N         | N          | N               | N          | N          | N            | N          | N                     | N                | N            | N               | N                | N                | N              | N                   | N                  | N                | N          | N            | N                      |          |            |             |              |             0 |           0 |               0 |                    0 | caching_sha2_password | $A$005$@Mg)vyN6lb
                                                                                                                                                                                                                              7tcWaGUeEz5mC2w/nctf1pd0.w/KBaBdZXrHnu0HmYgMW7 | N                | 2026-01-27 11:30:52   |              NULL | N              | N                | N              |                   NULL |                NULL | NULL                     | NULL            |
| localhost | PanelE           | N           | N           | N           | N           | N           | N         | N           | N             | N            | N         | N          | N               | N          | N          | N            | N          | N                     | N                | N            | N               | N                | N                | N              | N                   | N                  | N                | N          | N            | N                      |          |            |             |              |             0 |           0 |               0 |                    0 | caching_sha2_password |                                                                        | N                | 2026-01-27 11:27:23   |              NULL | N              | N                | N              |                   NULL |                NULL | NULL                     | NULL            |
| localhost | mysql.infoschema | Y           | N           | N           | N           | N           | N         | N           | N             | N            | N         | N          | N               | N          | N          | N            | N          | N                     | N                | N            | N               | N                | N                | N              | N                   | N                  | N                | N          | N            | N                      |          |            |             |              |             0 |           0 |               0 |                    0 | caching_sha2_password | $A$005$THISISACOMBINATIONOFINVALIDSALTANDPASSWORDTHATMUSTNEVERBRBEUSED | N                | 2026-01-21 14:05:38   |              NULL | Y              | N                | N              |                   NULL |                NULL | NULL                     | NULL            |
| localhost | mysql.session    | N           | N           | N           | N           | N           | N         | N           | Y             | N            | N         | N          | N               | N          | N          | N            | Y          | N                     | N                | N            | N               | N                | N                | N              | N                   | N                  | N                | N          | N            | N                      |          |            |             |              |             0 |           0 |               0 |                    0 | caching_sha2_password | $A$005$THISISACOMBINATIONOFINVALIDSALTANDPASSWORDTHATMUSTNEVERBRBEUSED | N                | 2026-01-21 14:05:38   |              NULL | Y              | N                | N              |                   NULL |                NULL | NULL                     | NULL            |
| localhost | mysql.sys        | N           | N           | N           | N           | N           | N         | N           | N             | N            | N         | N          | N               | N          | N          | N            | N          | N                     | N                | N            | N               | N                | N                | N              | N                   | N                  | N                | N          | N            | N                      |          |            |             |              |             0 |           0 |               0 |                    0 | caching_sha2_password | $A$005$THISISACOMBINATIONOFINVALIDSALTANDPASSWORDTHATMUSTNEVERBRBEUSED | N                | 2026-01-21 14:05:38   |              NULL | Y              | N                | N              |                   NULL |                NULL | NULL                     | NULL            |
| localhost | root             | Y           | Y           | Y           | Y           | Y           | Y         | Y           | Y             | Y            | Y         | Y          | Y               | Y          | Y          | Y            | Y          | Y                     | Y                | Y            | Y               | Y                | Y                | Y              | Y                   | Y                  | Y                | Y          | Y            | Y                      |          |            |             |              |             0 |           0 |               0 |                    0 | caching_sha2_password | $A$005$'E
                                                                                                                                                                                                                      Vk"w/ER
                                                                                                                                                                                                                             tGX8a9/BqIizy29calbHpRSW2rt1LOeNYfTQYP/nmR9ANAf4 | N                | 2026-01-21 14:05:41   |              NULL | N              | Y                | Y              |                   NULL |                NULL | NULL                     | NULL            |
+-----------+------------------+-------------+-------------+-------------+-------------+-------------+-----------+-------------+---------------+--------------+-----------+------------+-----------------+------------+------------+--------------+------------+-----------------------+------------------+--------------+-----------------+------------------+------------------+----------------+---------------------+--------------------+------------------+------------+--------------+------------------------+----------+------------+-------------+--------------+---------------+-------------+-----------------+----------------------+-----------------------+------------------------------------------------------------------------+------------------+-----------------------+-------------------+----------------+------------------+----------------+------------------------+---------------------+--------------------------+-----------------+
7 rows in set (0.00 sec)
mysql> 
]



$ mysql -u root
ERROR 1045 (28000): Access denied for user 'root'@'localhost' (using password: NO)

$ mysql -u root -p
Enter password:
Welcome to the MySQL monitor.

— —- —- —- —- —- —- —- —- —- —- —- —- —- —-


mysql> CREATE DATABASE Bank;
Query OK, 1 row affected

mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| Bank               |
| mysql              |
| information_schema |
| performance_schema |
| sys                |
+--------------------+

mysql> USE Bank;
Database changed

mysql> CREATE TABLE Customers (
    id INT,
    name VARCHAR(10)
);
Query OK

mysql> DESC Customers;
+-------+-------------+
| Field | Type        |
+-------+-------------+
| id    | int         |
| name  | varchar(10) |
+-------+-------------+

mysql> INSERT INTO Customers VALUES
(100,'Tom'),
(101,'Jerry');
Query OK, 2 rows affected

mysql> SELECT * FROM Customers;
+------+-------+
| id   | name  |
+------+-------+
| 100  | Tom   |
| 101  | Jerry |
+------+-------+

mysql> CREATE USER 'BatchE1'@'localhost' IDENTIFIED BY 'e@123';
Query OK

mysql> SHOW GRANTS FOR BatchE1@localhost;
GRANT USAGE ON *.* TO `BatchE1`@`localhost`;

mysql> GRANT INSERT ON Bank.Customers TO BatchE1@localhost;
Query OK

mysql> EXIT;
Bye

— —- —- —- —- —- —- —- —- —- —- —- —- —- —-
$ mysql -u BatchE1 -p
Enter password:
Welcome to the MySQL monitor.

mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| Bank               |
| information_schema |
| performance_schema |
+--------------------+

mysql> USE Bank;
Database changed

mysql> INSERT INTO Customers VALUES (102,'Chip');
Query OK, 1 row affected

mysql> SELECT * FROM Customers;
ERROR 1142 (42000): SELECT command denied to user 'BatchE1'@'localhost'
— —- —- —- —- —- —- —- —- —- —- —- —- —- —-
$ mysql -u root -p

mysql> USE Bank;
Database changed

mysql> SELECT * FROM Customers;
+------+-------+
| id   | name  |
+------+-------+
| 100  | Tom   |
| 101  | Jerry |
| 102  | Chip  |
+------+-------+

mysql> GRANT SELECT, UPDATE, DELETE
ON Bank.Customers TO BatchE1@localhost;
Query OK

mysql> EXIT;
Bye



— —- —- —- —- —- —- —- —- —- —- —- —- —- —-
$ mysql -u BatchE1 -p

mysql> USE Bank;
Database changed

mysql> SELECT * FROM Customers;
+------+-------+
| id   | name  |
+------+-------+
| 100  | Tom   |
| 101  | Jerry |
| 102  | Chip  |
+------+-------+

mysql> SELECT user FROM mysql.user;
ERROR 1142 (42000): SELECT command denied to user 'BatchE1'@'localhost'

— —- —- —- —- —- —- —- —- —- —- —- —- —- —-

$ mysql -u root -p

mysql> REVOKE INSERT, SELECT, UPDATE, DELETE
ON Bank.Customers FROM BatchE1@localhost;
Query OK

— —- —- —- —- —- —- —- —- —- —- —- —- —- —-
$ mysql -u BatchE1 -p

mysql> SELECT * FROM Customers;
ERROR 1142 (42000): SELECT command denied to user 'BatchE1'@'localhost'

```


## Create a database which consist of the following tables with appropriate constraints like primary key, foreign key, check constrains, not null etc.
* Project(project_id,proj_name,chief_arch) project_id is primary key
* Employee(Emp_id,Emp_name) Emp_id is primary key
* Assigned-To(Project_id,Emp_id)


```sql
mysql> create database proj;

--Query OK, 1 row affected (0.01 sec)

 mysql> show databases;
+--------------------+
| Database                |
+--------------------+
| ProjectDB               |
| information_schema      |
| mysql                   |
| performance_schema      |
| proj                    |
| sys                     |
+--------------------+
--6 rows in set (0.00 sec)


mysql> use ProjectDB;
Database changed

mysql> create table Project(proj_id int primary key, proj_name varchar(100) Not null, domain varchar(10) not null default 'FinTech');

--Query OK, 0 rows affected (0.02 sec)

mysql> show tables;
+---------------------+
| Tables_in_ProjectDB |
+---------------------+
| Project             |
+---------------------+
--1 row in set (0.00 sec)

mysql> desc Project;
+-----------+--------------+------+-----+---------+-------+
| Field     | Type         | Null | Key | Default | Extra |
+-----------+--------------+------+-----+---------+-------+
| proj_id   | int          | NO   | PRI | NULL    |       |
| proj_name | varchar(100) | NO   |     | NULL    |       |
| domain    | varchar(10)  | NO   |     | FinTech |       |
+-----------+--------------+------+-----+---------+-------+
--3 rows in set (0.00 sec)

mysql> create table employee(emp_id int primary key, emp_name varchar(100) not null);
--Query OK, 0 rows affected (0.02 sec)

mysql> create table Assigned_to ( proj_id int, emp_id int, primary key(proj_id, emp_id), foreign key(proj_id) references Project(proj_id) on delete cascade on update cascade, foreign key (emp_id) references employee(emp_id) on delete cascade on update cascade);
--Query OK, 0 rows affected (0.02 sec)

mysql> show tables;
+---------------------+
| Tables_in_ProjectDB |
+---------------------+
| Assigned_to         |
| Project             |
| employee            |
+---------------------+
--3 rows in set (0.00 sec)

mysql> desc Project;
+-----------+--------------+------+-----+---------+-------+
| Field     | Type         | Null | Key | Default | Extra |
+-----------+--------------+------+-----+---------+-------+
| proj_id   | int          | NO   | PRI | NULL    |       |
| proj_name | varchar(100) | NO   |     | NULL    |       |
| domain    | varchar(10)  | NO   |     | FinTech |       |
+-----------+--------------+------+-----+---------+-------+
--3 rows in set (0.00 sec)

mysql> desc employee;
+----------+--------------+------+-----+---------+-------+
| Field    | Type         | Null | Key | Default | Extra |
+----------+--------------+------+-----+---------+-------+
| emp_id   | int          | NO   | PRI | NULL    |       |
| emp_name | varchar(100) | NO   |     | NULL    |       |
+----------+--------------+------+-----+---------+-------+
--2 rows in set (0.00 sec)

mysql> desc Assigned_to;
+---------+------+------+-----+---------+-------+
| Field   | Type | Null | Key | Default | Extra |
+---------+------+------+-----+---------+-------+
| proj_id | int  | NO   | PRI | NULL    |       |
| emp_id  | int  | NO   | PRI | NULL    |       |
+---------+------+------+-----+---------+-------+
--2 rows in set (0.00 sec)



 mysql> INSERT INTO Project (proj_id, proj_name, domain)
     -> VALUES (1, 'Inventory Management System', 'HealthTech');
--Query OK, 1 row affected (0.01 sec)

mysql> INSERT INTO Project (proj_id, proj_name)
 -> VALUES (2, 'Online Banking System');
--Query OK, 1 row affected (0.01 sec)

mysql> INSERT INTO Project (proj_id, proj_name)
 -> VALUES (3, 'E-Commerce Platform');
--Query OK, 1 row affected (0.00 sec)

mysql> select * from Project;
+---------+-----------------------------+------------+
| proj_id | proj_name                   | domain     |
+---------+-----------------------------+------------+
|       1 | Inventory Management System | HealthTech |
|       2 | Online Banking System       | FinTech    |
|       3 | E-Commerce Platform         | FinTech    |
+---------+-----------------------------+------------+
--3 rows in set (0.00 sec)


mysql> INSERT INTO employee (emp_id, emp_name) VALUES (101, 'tom'),(102, ' jerry'), (103,'luca');
--Query OK, 3 rows affected (0.01 sec)
--Records: 3  Duplicates: 0  Warnings: 0

 mysql> select * from employee;
+--------+----------+
| emp_id | emp_name |
+--------+----------+
|    101 | tom      |
|    102 |  jerry   |
|    103 | luca     |
+--------+----------+
--3 rows in set (0.00 sec)

mysql> INSERT INTO Assigned_to (proj_id, emp_id) VALUES (1, 101);
-- Query OK, 1 row affected (0.00 sec)

mysql> INSERT INTO Assigned_to (proj_id, emp_id) VALUES (2, 103);
--Query OK, 1 row affected (0.01 sec)

mysql> INSERT INTO Assigned_to (proj_id, emp_id) VALUES (3, 102);
--Query OK, 1 row affected (0.01 sec)

mysql> INSERT INTO Assigned_to (proj_id, emp_id) VALUES (3, 101);
--Query OK, 1 row affected (0.01 sec)

mysql> INSERT INTO Assigned_to (proj_id, emp_id) VALUES (2, 101);
--Query OK, 1 row affected (0.01 sec)

mysql> SELECT * FROM Project;
+---------+-----------------------------+------------+
| proj_id | proj_name                   | domain     |
+---------+-----------------------------+------------+
|       1 | Inventory Management System | HealthTech |
|       2 | Online Banking System       | FinTech    |
|       3 | E-Commerce Platform         | FinTech    |
+---------+-----------------------------+------------+
--3 rows in set (0.00 sec)


mysql> SELECT * FROM employee;
+--------+----------+
| emp_id | emp_name |
+--------+----------+
|    101 | tom      |
|    102 |  jerry   |
|    103 | luca     |
+--------+----------+
--3 rows in set (0.00 sec)

mysql> SELECT * FROM Assigned_to;
+---------+--------+
| proj_id | emp_id |
+---------+--------+
|       1 |    101 |
|       2 |    101 |
|       3 |    101 |
|       3 |    102 |
|       2 |    103 |
+---------+--------+
--5 rows in set (0.00 sec)

```
---
## Create a database which consist of the following tables with appropriate constraints like primary key, foreign key, check constrains, not null etc.

* Employee(emp_no,name,skill,pay-rate) eno primary key
* Position(posting_no,skill) posting_no primary key
* Duty_allocation(posting_no,emp_no,day,shift)

```sql
mahek@mahek-ZenBook-UX325EA-UX325EA:~$ sudo mysql
[sudo] password for mahek: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 16
Server version: 8.0.45-0ubuntu0.22.04.1 (Ubuntu)

Copyright (c) 2000, 2026, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> CREATE DATABASE DisneyDB; USE DisneyDB;
Query OK, 1 row affected (0.01 sec)

Database changed
mysql> CREATE TABLE CastMember (cast_id INT PRIMARY KEY, cast_name VARCHAR(100) NOT NULL, talent VARCHAR(50) NOT NULL, pay_rate DECIMAL(8,2) NOT NULL, CHECK (pay_rate > 0));
Query OK, 0 rows affected (0.02 sec)

mysql> 
mysql> INSERT INTO CastMember VALUES (1,'Mickey Mouse','Security',1500),(2,'Minnie Mouse','Management',1800),(3,'Donald Duck','IT',2000);
Query OK, 3 rows affected (0.01 sec)
Records: 3  Duplicates: 0  Warnings: 0

mysql> SELECT * FROM CastMember;
+---------+--------------+------------+----------+
| cast_id | cast_name    | talent     | pay_rate |
+---------+--------------+------------+----------+
|       1 | Mickey Mouse | Security   |  1500.00 |
|       2 | Minnie Mouse | Management |  1800.00 |
|       3 | Donald Duck  | IT         |  2000.00 |
+---------+--------------+------------+----------+
3 rows in set (0.00 sec)

mysql> CREATE TABLE Attractions (attraction_id INT PRIMARY KEY, talent VARCHAR(50) NOT NULL);
Query OK, 0 rows affected (0.03 sec)

mysql> DESC Attractions;
+---------------+-------------+------+-----+---------+-------+
| Field         | Type        | Null | Key | Default | Extra |
+---------------+-------------+------+-----+---------+-------+
| attraction_id | int         | NO   | PRI | NULL    |       |
| talent        | varchar(50) | NO   |     | NULL    |       |
+---------------+-------------+------+-----+---------+-------+
2 rows in set (0.00 sec)

mysql> DESC Attractions;
+---------------+-------------+------+-----+---------+-------+
| Field         | Type        | Null | Key | Default | Extra |
+---------------+-------------+------+-----+---------+-------+
| attraction_id | int         | NO   | PRI | NULL    |       |
| talent        | varchar(50) | NO   |     | NULL    |       |
+---------------+-------------+------+-----+---------+-------+
2 rows in set (0.00 sec)

mysql> INSERT INTO Attractions VALUES (101,'Security'),(102,'Management'),(103,'IT');
Query OK, 3 rows affected (0.01 sec)
Records: 3  Duplicates: 0  Warnings: 0

mysql> SELECT * FROM Attractions;
+---------------+------------+
| attraction_id | talent     |
+---------------+------------+
|           101 | Security   |
|           102 | Management |
|           103 | IT         |
+---------------+------------+
3 rows in set (0.00 sec)

mysql> CREATE TABLE Shift_Roster (attraction_id INT NOT NULL, cast_id INT NOT NULL, day VARCHAR(10) NOT NULL, shift VARCHAR(10) NOT NULL, PRIMARY KEY (attraction_id, cast_id, day), FOREIGN KEY (attraction_id) REFERENCES Attractions(attraction_id) ON DELETE CASCADE ON UPDATE CASCADE, FOREIGN KEY (cast_id) REFERENCES CastMember(cast_id) ON DELETE CASCADE ON UPDATE CASCADE, CHECK (shift IN ('Morning','Evening','Night')));
Query OK, 0 rows affected (0.02 sec)

mysql> DESC Shift_Roster;
+---------------+-------------+------+-----+---------+-------+
| Field         | Type        | Null | Key | Default | Extra |
+---------------+-------------+------+-----+---------+-------+
| attraction_id | int         | NO   | PRI | NULL    |       |
| cast_id       | int         | NO   | PRI | NULL    |       |
| day           | varchar(10) | NO   | PRI | NULL    |       |
| shift         | varchar(10) | NO   |     | NULL    |       |
+---------------+-------------+------+-----+---------+-------+
4 rows in set (0.01 sec)

mysql> INSERT INTO Shift_Roster VALUES (101,1,'Monday','Morning'),(102,2,'Monday','Evening'),(103,3,'Tuesday','Night'),(101,1,'Wednesday','Morning');
Query OK, 4 rows affected (0.01 sec)
Records: 4  Duplicates: 0  Warnings: 0

mysql> SELECT * FROM Shift_Roster;
+---------------+---------+-----------+---------+
| attraction_id | cast_id | day       | shift   |
+---------------+---------+-----------+---------+
|           101 |       1 | Monday    | Morning |
|           101 |       1 | Wednesday | Morning |
|           102 |       2 | Monday    | Evening |
|           103 |       3 | Tuesday   | Night   |
+---------------+---------+-----------+---------+
4 rows in set (0.00 sec)

mysql> SHOW TABLES;
+--------------------+
| Tables_in_DisneyDB |
+--------------------+
| Attractions        |
| CastMember         |
| Shift_Roster       |
+--------------------+
3 rows in set (0.00 sec)

mysql> 
```



