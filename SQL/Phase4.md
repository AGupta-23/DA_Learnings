# SQL String Functions Cheat Sheet (README)

String functions are used to manipulate, format, search, and transform text data stored in SQL databases.

---

# Common SQL String Functions

| Function      | Purpose                             | Example                                | Output        |
| ------------- | ----------------------------------- | -------------------------------------- | ------------- |
| UPPER()       | Converts text to uppercase          | `UPPER('hello')`                       | HELLO         |
| LOWER()       | Converts text to lowercase          | `LOWER('HELLO')`                       | hello         |
| LENGTH()      | Returns number of characters        | `LENGTH('Hello')`                      | 5             |
| CHAR_LENGTH() | Returns character count             | `CHAR_LENGTH('Hello')`                 | 5             |
| CONCAT()      | Combines strings                    | `CONCAT('John',' ','Doe')`             | John Doe      |
| TRIM()        | Removes leading and trailing spaces | `TRIM(' Hello ')`                      | Hello         |
| LTRIM()       | Removes leading spaces              | `LTRIM(' Hello')`                      | Hello         |
| RTRIM()       | Removes trailing spaces             | `RTRIM('Hello ')`                      | Hello         |
| SUBSTRING()   | Extracts part of a string           | `SUBSTRING('Database',1,4)`            | Data          |
| LEFT()        | Returns leftmost characters         | `LEFT('Database',4)`                   | Data          |
| RIGHT()       | Returns rightmost characters        | `RIGHT('Database',4)`                  | base          |
| REPLACE()     | Replaces text                       | `REPLACE('I like tea','tea','coffee')` | I like coffee |
| POSITION()    | Finds character position            | `POSITION('a' IN 'Database')`          | 2             |
| REVERSE()     | Reverses a string                   | `REVERSE('SQL')`                       | LQS           |
| REPEAT()      | Repeats a string                    | `REPEAT('Hi',3)`                       | HiHiHi        |
| LPAD()        | Pads left side                      | `LPAD('7',3,'0')`                      | 007           |
| RPAD()        | Pads right side                     | `RPAD('7',3,'0')`                      | 700           |

---

# UPPER()

Converts text to uppercase.

```sql
SELECT UPPER(name)
FROM employees;
```

### Use Cases

* Standardizing names
* Case-insensitive comparisons

---

# LOWER()

Converts text to lowercase.

```sql
SELECT LOWER(email)
FROM employees;
```

### Use Cases

* Email normalization
* Search operations

---

# LENGTH()

Returns the number of characters.

```sql
SELECT LENGTH(name)
FROM employees;
```

### Example

```text
LENGTH('Rahul') → 5
```

### Use Cases

* Password validation
* Data quality checks

---

# CONCAT()

Combines multiple strings.

```sql
SELECT CONCAT(first_name,' ',last_name)
FROM employees;
```

### Example

```text
John + Doe → John Doe
```

### Use Cases

* Full names
* Building messages

---

# TRIM()

Removes spaces from both ends.

```sql
SELECT TRIM('  SQL  ');
```

### Output

```text
SQL
```

### Use Cases

* Cleaning imported data
* Removing accidental spaces

---

# LTRIM() and RTRIM()

Remove spaces from one side only.

```sql
SELECT LTRIM(' SQL');
SELECT RTRIM('SQL ');
```

---

# SUBSTRING()

Extracts a portion of text.

## Syntax

```sql
SUBSTRING(string, start, length)
```

## Example

```sql
SELECT SUBSTRING('PostgreSQL',1,6);
```

### Output

```text
Postgr
```

### Use Cases

* Extracting codes
* Parsing identifiers

---

# LEFT()

Returns characters from the left.

```sql
SELECT LEFT('Database',4);
```

### Output

```text
Data
```

---

# RIGHT()

Returns characters from the right.

```sql
SELECT RIGHT('Database',4);
```

### Output

```text
base
```

### Use Cases

* File extensions
* Last digits of IDs

---

# REPLACE()

Replaces part of a string.

```sql
SELECT REPLACE('I love Java','Java','SQL');
```

### Output

```text
I love SQL
```

### Use Cases

* Data correction
* Standardization

---

# POSITION()

Returns the position of a character or substring.

```sql
SELECT POSITION('a' IN 'Database');
```

### Output

```text
2
```

### Use Cases

* Finding delimiters
* Parsing strings

---

# REVERSE()

Reverses text.

```sql
SELECT REVERSE('SQL');
```

### Output

```text
LQS
```

### Use Cases

* String manipulation exercises
* Special formatting

---

# REPEAT()

Repeats text multiple times.

```sql
SELECT REPEAT('*',5);
```

### Output

```text
*****
```

---

# LPAD()

Pads characters on the left.

```sql
SELECT LPAD('7',3,'0');
```

### Output

```text
007
```

### Use Cases

* Formatting invoice numbers
* Standardized IDs

---

# RPAD()

Pads characters on the right.

```sql
SELECT RPAD('7',3,'0');
```

### Output

```text
700
```

### Use Cases

* Fixed-width formatting

---

# Practical Examples

## Full Name

```sql
SELECT CONCAT(first_name,' ',last_name)
FROM employees;
```

---

## Convert Emails to Lowercase

```sql
SELECT LOWER(email)
FROM employees;
```

---

## Find Employees Whose Name Starts With A

```sql
SELECT *
FROM employees
WHERE name LIKE 'A%';
```

---

## Remove Extra Spaces

```sql
SELECT TRIM(name)
FROM employees;
```

---

## Generate Employee Codes

```sql
SELECT CONCAT('EMP', LPAD(employee_id,4,'0'))
FROM employees;
```

### Example Output

```text
EMP0001
EMP0002
EMP0003
```

---

# Quick Revision

```text
UPPER()      → Uppercase
LOWER()      → Lowercase
LENGTH()     → Character Count
CONCAT()     → Join Strings
TRIM()       → Remove Spaces
SUBSTRING()  → Extract Text
LEFT()       → Left Characters
RIGHT()      → Right Characters
REPLACE()    → Replace Text
POSITION()   → Find Position
REVERSE()    → Reverse String
REPEAT()     → Repeat Text
LPAD()       → Left Padding
RPAD()       → Right Padding
```

## Memory Trick

```text
Clean  → TRIM
Join   → CONCAT
Count  → LENGTH
Extract → SUBSTRING
Change → REPLACE
Format → LPAD / RPAD
Case   → UPPER / LOWER
```
