# SQL & Database Quick Reference

## Database

A **Database** is an organized collection of data that allows storage,
retrieval, and management of information efficiently.

## RDBMS

**Relational Database Management System (RDBMS)** stores data in tables
(rows and columns) and uses relationships between tables. Examples:
MySQL, PostgreSQL, SQL Server, Oracle.

## SQL

**Structured Query Language (SQL)** is the standard language used to
interact with relational databases.

It is used to: - Store data - Retrieve data - Update data - Delete
data - Manage databases

### SQL Working

1.  Input -- User writes a SQL query.
2.  Parsing -- Syntax is checked.
3.  Optimization -- Fastest execution plan is selected.
4.  Execution -- Query runs.
5.  Output -- Results are returned.

## SQL Components

-   Database -- Collection of related data.
-   Tables -- Store data in rows and columns.
-   Indexes -- Improve search speed.
-   Views -- Virtual tables from queries.
-   Stored Procedures -- Saved SQL programs.
-   Transactions -- Ensure all operations succeed or fail together.
-   Security & Permissions -- Control access.
-   Joins -- Combine data from multiple tables.

### Comments

Single-line: `-- comment`

Multi-line: `/* comment */`

## NoSQL

**NoSQL (Not Only SQL)** databases store non-relational data and are
designed for flexibility and scalability.

Types: - Document Databases - Key-Value Stores - Column Stores - Graph
Databases

Examples: MongoDB, Cassandra, Redis, Neo4j.

### SQL vs NoSQL

-   SQL → Structured data, tables, fixed schema.
-   NoSQL → Flexible schema, unstructured/semi-structured data.
-   SQL → Strong relationships and ACID compliance.
-   NoSQL → High scalability and distributed systems.

## PostgreSQL

**PostgreSQL** is an open-source object-relational database management
system (ORDBMS).

Features: - Uses SQL - Highly reliable and secure - Supports advanced
queries and transactions - ACID compliant - Handles both simple and
complex applications

## Benefits of SQL

-   Efficient
-   Standardized
-   Scalable
-   Flexible

## Limitations of SQL

-   Advanced optimization can be complex.
-   Less suitable for massive unstructured data.
-   Features vary across database systems.
-   Traditional SQL is not designed for real-time analytics.

# ACID Properties in DBMS

**ACID** is a set of properties that ensures database transactions are processed reliably and data remains consistent.

## A – Atomicity
**"All or Nothing"**

A transaction either completes fully or does not happen at all.

**Example:**  
If ₹500 is transferred from Account A to Account B:
- ₹500 deducted from A
- ₹500 added to B

If the second step fails, the deduction is reversed.

---

## C – Consistency
**"Data remains valid"**

A transaction takes the database from one valid state to another while following all rules and constraints.

**Example:**  
If a bank account cannot have a negative balance, the transaction will be rejected if it violates that rule.

---

## I – Isolation
**"Transactions don't interfere"**

Multiple transactions running at the same time behave as if they are executed one after another.

**Example:**  
Two users booking the same movie seat simultaneously cannot both get the same seat.

---

## D – Durability
**"Changes are permanent"**

Once a transaction is committed, the changes are permanently saved, even if there is a power failure or system crash.

**Example:**  
After an online payment is successful, the transaction record remains stored even if the server restarts.

---

## One-Line Memory Trick

> **ACID = All or Nothing, Correct Data, Independent Transactions, Permanent Storage.**

## Summary Table

| Property | Meaning | Simple Keyword |
|----------|-----------|----------------|
| A | Atomicity | All or Nothing |
| C | Consistency | Valid Data |
| I | Isolation | No Interference |
| D | Durability | Permanent Changes |

**Examples of ACID-compliant databases:** PostgreSQL, MySQL (InnoDB), Oracle, and SQL Server.