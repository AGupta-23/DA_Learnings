# ALTER TABLE & CASE – Quick README

## ALTER TABLE

Used to **modify the structure of an existing table**.

### Common ALTER Commands

| Operation        | Syntax                                                          |
| ---------------- | --------------------------------------------------------------- |
| Add Column       | `ALTER TABLE employees ADD email VARCHAR(100);`                 |
| Drop Column      | `ALTER TABLE employees DROP COLUMN email;`                      |
| Rename Column    | `ALTER TABLE employees RENAME COLUMN name TO employee_name;`    |
| Change Data Type | `ALTER TABLE employees ALTER COLUMN salary TYPE NUMERIC(12,2);` |
| Rename Table     | `ALTER TABLE employees RENAME TO employee_details;`             |

### Memory Trick

```text
ALTER = Change the Table Structure
```

---

# CASE Statement

Used for **conditional logic (IF-ELSE) in SQL**.

## Syntax

```sql
SELECT column_name,
       CASE
           WHEN condition1 THEN result1
           WHEN condition2 THEN result2
           ELSE result
       END
FROM table_name;
```

---

## Example

Categorize employees based on salary:

```sql
SELECT name,
       salary,
       CASE
           WHEN salary >= 80000 THEN 'High'
           WHEN salary >= 50000 THEN 'Medium'
           ELSE 'Low'
       END AS salary_category
FROM employees;
```

### Example Output

| Name  | Salary | Salary_Category |
| ----- | ------ | --------------- |
| Rahul | 90000  | High            |
| Priya | 65000  | Medium          |
| Aman  | 40000  | Low             |

### Common Use Cases

* Grade classification
* Salary categorization
* Status mapping
* Custom labels in reports

### Memory Trick

```text
CASE = SQL's IF → ELSE
```
