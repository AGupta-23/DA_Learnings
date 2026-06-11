# SQL Complete Cheat Sheet (README)

A compact reference covering **SQL Commands, Syntax, Clauses, Operators, Aggregate Functions, and Common Query Patterns**.

---

# SQL Command Categories

| Category | Full Form                    | Purpose                    | Common Commands                       |
| -------- | ---------------------------- | -------------------------- | ------------------------------------- |
| DDL      | Data Definition Language     | Defines database structure | CREATE, ALTER, DROP, TRUNCATE, RENAME |
| DQL      | Data Query Language          | Retrieves data             | SELECT                                |
| DML      | Data Manipulation Language   | Modifies data              | INSERT, UPDATE, DELETE                |
| DCL      | Data Control Language        | Controls permissions       | GRANT, REVOKE                         |
| TCL      | Transaction Control Language | Manages transactions       | COMMIT, ROLLBACK, SAVEPOINT           |

---

# SQL Query Execution Order

```text
FROM
↓
WHERE
↓
GROUP BY
↓
HAVING
↓
SELECT
↓
ORDER BY
↓
LIMIT
```

---

# DDL – Data Definition Language

## Create Database

```sql
CREATE DATABASE company_db;
```

## Create Table

```sql
CREATE TABLE employees (
    employee_id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    department VARCHAR(50),
    salary NUMERIC(10,2)
);
```

## Alter Table

```sql
ALTER TABLE employees
ADD email VARCHAR(100);
```

## Rename Table

```sql
ALTER TABLE employees
RENAME TO employee_details;
```

## Truncate Table

```sql
TRUNCATE TABLE employees;
```

Deletes all rows but keeps the structure.

## Drop Table

```sql
DROP TABLE employees;
```

Deletes the table permanently.

---

# DML – Data Manipulation Language

## INSERT

```sql
INSERT INTO employees(name, department, salary)
VALUES ('Rahul', 'IT', 65000);
```

Adds new records.

---

## UPDATE

```sql
UPDATE employees
SET salary = 70000
WHERE employee_id = 1;
```

Updates existing records.

⚠️ Without `WHERE`, all rows are updated.

---

## DELETE

```sql
DELETE FROM employees
WHERE employee_id = 1;
```

Deletes records.

⚠️ Without `WHERE`, all rows are deleted.

---

# DQL – Data Query Language

## View All Data

```sql
SELECT *
FROM employees;
```

---

## View Specific Columns

```sql
SELECT name, salary
FROM employees;
```

---

# CRUD Operations

| Operation | SQL Command |
| --------- | ----------- |
| Create    | INSERT      |
| Read      | SELECT      |
| Update    | UPDATE      |
| Delete    | DELETE      |

---

# SQL Clauses

## SELECT

**What to display**

```sql
SELECT name, salary
FROM employees;
```

---

## FROM

**Where the data comes from**

```sql
SELECT *
FROM employees;
```

---

## WHERE

**Filters rows before grouping**

```sql
SELECT *
FROM employees
WHERE department = 'HR';
```

---

## DISTINCT

**Removes duplicates**

```sql
SELECT DISTINCT department
FROM employees;
```

---

## ORDER BY

**Sorts results**

Ascending:

```sql
SELECT *
FROM employees
ORDER BY salary ASC;
```

Descending:

```sql
SELECT *
FROM employees
ORDER BY salary DESC;
```

---

## LIMIT

**Restricts output**

```sql
SELECT *
FROM employees
LIMIT 5;
```

---

## GROUP BY

**Creates groups for aggregation**

```sql
SELECT department,
       COUNT(*)
FROM employees
GROUP BY department;
```

### Example Output

| Department | Employees |
| ---------- | --------- |
| HR         | 5         |
| IT         | 8         |
| Sales      | 4         |

### Memory Trick

```text
GROUP BY = Make Groups → Then Aggregate
```

---

## HAVING

**Filters grouped data**

```sql
SELECT department,
       COUNT(*)
FROM employees
GROUP BY department
HAVING COUNT(*) > 5;
```

---

## WHERE vs HAVING

| WHERE           | HAVING          |
| --------------- | --------------- |
| Filters rows    | Filters groups  |
| Before GROUP BY | After GROUP BY  |
| No aggregates   | Uses aggregates |

---

# SQL Operators

## Comparison Operators

| Operator | Meaning               |
| -------- | --------------------- |
| =        | Equal to              |
| <>       | Not equal to          |
| !=       | Not equal to          |
| >        | Greater than          |
| <        | Less than             |
| >=       | Greater than or equal |
| <=       | Less than or equal    |

### Example

```sql
SELECT *
FROM employees
WHERE salary > 50000;
```

---

## Logical Operators

### AND

```sql
SELECT *
FROM employees
WHERE department='IT'
AND salary > 50000;
```

---

### OR

```sql
SELECT *
FROM employees
WHERE department='HR'
OR department='IT';
```

---

### NOT

```sql
SELECT *
FROM employees
WHERE NOT department='HR';
```

---

## IN

Checks multiple values.

```sql
SELECT *
FROM employees
WHERE department IN ('HR','IT','Sales');
```

---

## BETWEEN

Checks ranges.

```sql
SELECT *
FROM employees
WHERE salary BETWEEN 30000 AND 60000;
```

---

## LIKE

Pattern matching.

Starts with A:

```sql
SELECT *
FROM employees
WHERE name LIKE 'A%';
```

Contains A:

```sql
SELECT *
FROM employees
WHERE name LIKE '%A%';
```

Ends with A:

```sql
SELECT *
FROM employees
WHERE name LIKE '%A';
```

### Wildcards

| Symbol | Meaning                  |
| ------ | ------------------------ |
| %      | Any number of characters |
| _      | Single character         |

---

## IS NULL

```sql
SELECT *
FROM employees
WHERE manager_id IS NULL;
```

---

## IS NOT NULL

```sql
SELECT *
FROM employees
WHERE manager_id IS NOT NULL;
```

---

# Aggregate Functions

Aggregate functions operate on multiple rows.

| Function | Purpose       | Example     |
| -------- | ------------- | ----------- |
| COUNT()  | Counts rows   | COUNT(*)    |
| SUM()    | Total value   | SUM(salary) |
| AVG()    | Average value | AVG(salary) |
| MAX()    | Highest value | MAX(salary) |
| MIN()    | Lowest value  | MIN(salary) |

---

## Examples

### Count Employees

```sql
SELECT COUNT(*)
FROM employees;
```

---

### Total Salary

```sql
SELECT SUM(salary)
FROM employees;
```

---

### Average Salary

```sql
SELECT AVG(salary)
FROM employees;
```

---

### Highest Salary

```sql
SELECT MAX(salary)
FROM employees;
```

---

### Lowest Salary

```sql
SELECT MIN(salary)
FROM employees;
```

---

# DCL – Data Control Language

## GRANT

```sql
GRANT SELECT ON employees TO user1;
```

Gives permissions.

---

## REVOKE

```sql
REVOKE SELECT ON employees FROM user1;
```

Removes permissions.

---

# TCL – Transaction Control Language

## COMMIT

```sql
COMMIT;
```

Saves changes permanently.

---

## ROLLBACK

```sql
ROLLBACK;
```

Undoes uncommitted changes.

---

## SAVEPOINT

```sql
SAVEPOINT sp1;
```

Creates a checkpoint.

---

## Rollback to Savepoint

```sql
ROLLBACK TO sp1;
```

Returns to a checkpoint.

---

# Common Query Patterns

## Find all employees

```sql
SELECT *
FROM employees;
```

---

## Employees from HR

```sql
SELECT *
FROM employees
WHERE department = 'HR';
```

---

## Top 5 Highest Salaries

```sql
SELECT *
FROM employees
ORDER BY salary DESC
LIMIT 5;
```

---

## Employees earning between 30k–60k

```sql
SELECT *
FROM employees
WHERE salary BETWEEN 30000 AND 60000;
```

---

## Employees whose names start with A

```sql
SELECT *
FROM employees
WHERE name LIKE 'A%';
```

---

## Employee Count by Department

```sql
SELECT department,
       COUNT(*)
FROM employees
GROUP BY department;
```

---

## Departments with More Than 5 Employees

```sql
SELECT department,
       COUNT(*)
FROM employees
GROUP BY department
HAVING COUNT(*) > 5;
```

---

# Final Revision

```text
DDL → Structure
DML → Modify Data
DQL → Retrieve Data
DCL → Permissions
TCL → Transactions

SELECT   → What
FROM     → From Where
WHERE    → Filter Rows
GROUP BY → Make Groups
HAVING   → Filter Groups
ORDER BY → Sort
LIMIT    → Restrict Output

COUNT → Count
SUM   → Total
AVG   → Average
MAX   → Highest
MIN   → Lowest
```

## SQL Learning Flow

```text
Create Database
↓
Create Table
↓
INSERT Data
↓
SELECT Data
↓
Filter (WHERE)
↓
Sort (ORDER BY)
↓
Aggregate (COUNT/SUM/AVG)
↓
GROUP BY
↓
HAVING
↓
UPDATE / DELETE
↓
COMMIT or ROLLBACK
```
