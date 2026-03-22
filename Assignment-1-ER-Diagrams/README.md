# Assignment 1 — ER Diagrams

## Title
Design an ER Diagram for Different Case Studies

## Aim
Create Entity-Relationship diagrams for given problem statements.

## Objective
Understand ER diagram components and design methodology.

## Theory

### ER Diagram Components
| Symbol | Represents |
|--------|-----------|
| Rectangle | Entity |
| Ellipse | Attribute |
| Diamond | Relationship |
| Line | Link between entity and attribute/relationship |
| Double Ellipse | Multi-valued attribute |

### Design Steps
1. Identify entities in the problem statement
2. Decide relationships and cardinality between entities
3. Draw entities with their attributes
4. Connect entities through relationships

### Cardinality Types
- **1:1** — One-to-One
- **1:N** — One-to-Many
- **M:N** — Many-to-Many

---

## Exercises

### Batch A
1. **NHL Database** — Entities: Teams, Players, Games, Injuries
2. **Educational Institute** — Entities: Departments, Students, Professors, Courses, Projects

### Batch B
1. **Banking System** — Entities: Branches, Customers, Accounts (Savings/Checking), Loans, Employees, Dependents

### Batch C
1. **Company Database** — Entities: Departments, Projects, Employees, Dependents
2. **Car Insurance** — Entities: Customers, Cars, Accidents

### Batch D
1. **University Registrar** — Entities: Courses, Offerings, Students, Instructors, Enrollment
2. **Salesperson/Order System** — Entities: Salespersons, Orders, Products

---

## FAQs

**Q1. What are the types of attributes in an ER diagram?**
- **Simple**: Cannot be divided further (e.g., Age)
- **Composite**: Can be divided into sub-parts (e.g., Name → First Name, Last Name)
- **Multi-valued**: Can have multiple values (e.g., Phone Numbers) — shown with double ellipse
- **Derived**: Derived from another attribute (e.g., Age from Date of Birth) — shown with dashed ellipse

**Q2. What is the difference between a Primary Key and a Foreign Key?**
- **Primary Key**: Uniquely identifies each record in a table; cannot be NULL.
- **Foreign Key**: A field in one table that references the Primary Key of another table; establishes referential integrity.

**Q3. What is a weak entity?**
A weak entity is an entity that cannot be uniquely identified by its own attributes alone. It depends on a strong (owner) entity for its existence and is identified by a combination of its partial key and the primary key of the owner entity. Represented by a double rectangle in ER diagrams.
