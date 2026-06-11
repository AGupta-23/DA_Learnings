## Database

A **Database** is a large container that stores related data in an organized manner.

* It contains tables, schemas, views, functions, indexes, and other database objects.
* In a PostgreSQL server, you can create **multiple databases**.
* Each database is **isolated**, meaning data from one database does not mix with another.

### Example

```
PostgreSQL Server
│
├── Company_DB
├── School_DB
└── ECommerce_DB
```

Each database is independent.

---

## Schema

A **Schema** is a logical container inside a database used to organize database objects.

Think of it as a **folder inside a database** that helps group related objects.

Schemas can contain:

* Tables
* Views
* Functions
* Procedures
* Sequences
* Indexes

### Important Notes

* Every PostgreSQL database has a default schema called **`public`**.
* Multiple schemas can exist within the same database.
* Schemas help avoid naming conflicts and improve organization.

### Example

```
Company_DB
│
├── public
│   ├── employees
│   └── departments
│
├── sales
│   ├── customers
│   └── orders
│
└── hr
    ├── payroll
    └── attendance
```

---

## Table

A **Table** stores actual data in the form of **rows and columns**.

* Columns define what type of information is stored.
* Rows represent individual records.

### Example

### Employees Table

| Employee_ID | Name  | Department | Salary |
| ----------- | ----- | ---------- | ------ |
| 101         | Rahul | HR         | 50000  |
| 102         | Priya | Sales      | 65000  |
| 103         | Aman  | IT         | 70000  |

### Key Points

* A database can contain multiple tables.
* Tables can be related to one another using keys.
* Tables are the primary storage units in an RDBMS.

---

## Database Hierarchy

```
PostgreSQL Server
    ↓
Database
    ↓
Schema
    ↓
Table
    ↓
Rows & Columns
```

---

# SQL Data Types

Each column in SQL must have a **data type**, which defines the kind of data it can store.

Choosing appropriate data types improves:

* Data integrity
* Query performance
* Storage efficiency
* Index efficiency

## Common SQL Data Types

| Data Type               | Description                   | Example Usage           |
| ----------------------- | ----------------------------- | ----------------------- |
| INTEGER / INT           | Whole numbers                 | Age, Quantity           |
| BIGINT                  | Large whole numbers           | Transaction IDs         |
| DECIMAL(p,s) / NUMERIC  | Exact decimal values          | Salary, Price           |
| REAL / DOUBLE PRECISION | Approximate decimal values    | Scientific calculations |
| VARCHAR(n)              | Variable-length text          | Names, Email            |
| CHAR(n)                 | Fixed-length text             | Country Codes           |
| TEXT                    | Large text values             | Comments, Descriptions  |
| BOOLEAN                 | True or False values          | Is_Active               |
| DATE                    | Stores dates                  | Birth Date              |
| TIME                    | Stores time                   | Meeting Time            |
| TIMESTAMP               | Date and time together        | Order Timestamp         |
| SERIAL                  | Auto-increment integer        | Primary Keys            |
| UUID                    | Universally unique identifier | Distributed Systems     |
| BYTEA                   | Binary data                   | Files, Images           |

---

# SQL Constraints

**Constraints** are rules applied to columns to maintain data accuracy and integrity.

## Common Constraints

| Constraint              | Purpose                                | Example                 |
| ----------------------- | -------------------------------------- | ----------------------- |
| NOT NULL                | Prevents empty values                  | Name cannot be blank    |
| UNIQUE                  | Prevents duplicate values              | Email must be unique    |
| PRIMARY KEY             | Uniquely identifies each row           | Employee_ID             |
| FOREIGN KEY             | Maintains relationships between tables | Department_ID reference |
| CHECK                   | Enforces custom conditions             | Salary > 0              |
| DEFAULT                 | Assigns default values                 | Status = 'Active'       |
| AUTO INCREMENT (SERIAL) | Generates values automatically         | ID column               |

---

## Constraint Example

```sql
CREATE TABLE employees (
    employee_id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE,
    salary NUMERIC(10,2) CHECK (salary > 0),
    status VARCHAR(20) DEFAULT 'Active',
    department_id INTEGER REFERENCES departments(department_id)
);
```