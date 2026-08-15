# Oracle Database – README

## 1. Basics of Databases

### Database

A **database** is an organized collection of information that is used by applications.

### DBMS

A **Database Management System (DBMS)** controls how data is:

* Stored
* Organized
* Retrieved

---

## 2. Early DBMS Types

### 1. Hierarchical Model

* Uses a **tree-like structure**.
* Data is organized in **parent → child** relationships.

### 2. Network Model

* Supports **many-to-many relationships** between data items.

---

## 3. Relational Model

The **relational model** stores data in the form of tables.

* **Tables (Relations)** → Store data.
* **Rows (Tuples)** → Represent individual records.
* **Columns (Attributes)** → Represent properties of the data.
* **Data structures** → Tables and database objects.
* **Operations** → Actions performed on data.
* **Integrity rules** → Rules that help keep data correct and consistent.

---

## 4. RDBMS

**RDBMS (Relational Database Management System)** stores and retrieves data using logical and physical operations.

### Logical Operations

* Describe **what data is needed**.
* Focus on the data required by the user.

### Physical Operations

* Describe **how the data is fetched**.
* Focus on the way data is physically accessed.

### Oracle Database

**Oracle Database** is an RDBMS that is extended with additional **object-relational features**.

---

# 5. History of Oracle

* **1977** → Oracle was founded by **Larry Ellison, Bob Miner, and Ed Oates**.
* **1979** → Oracle V2 was released and became the first commercial SQL-based RDBMS.
* **1983** → Oracle Version 3 was introduced and became portable across platforms.

### Important Features Added in Later Oracle Versions

* **Oracle 6** → PL/SQL language.
* **Oracle 7** → Stored procedures and triggers.
* **Oracle 8** → Objects and partitioning.
* **Oracle 8i** → Internet computing and Java support.
* **Oracle 9i** → Oracle RAC (Real Application Clusters).
* **Oracle 10g** → Grid computing.
* **Oracle 11g** → Automation and manageability.

---

# 6. Schema Objects

### Schema

A **schema** is a logical collection of database objects owned by a user.

### Main Schema Objects

#### Tables

* Store data in **rows and columns**.
* Can have rules called **constraints** to maintain data integrity.

#### Indexes

* Help **speed up data retrieval**.
* Improve the performance of queries.

---

# 7. Data Access

### SQL

**SQL (Structured Query Language)** is used to query and manage data in a database.

Common SQL tasks include:

* Querying data
* Inserting data
* Updating data
* Deleting data
* Creating database objects
* Controlling access

### PL/SQL

**PL/SQL** is Oracle's procedural extension of SQL.

* Allows procedural programming with SQL.
* Logic can be stored and executed inside the Oracle database.
* Used for procedures, functions, triggers, and other program units.

### Java

Java can also be **stored and executed inside an Oracle database** to support database applications.

---

# 8. Transaction Management

### Transaction

A **transaction** is a group of SQL statements that either **succeed together or fail together**.

Example:

```sql
UPDATE accounts
SET balance = balance - 1000
WHERE account_id = 101;

UPDATE accounts
SET balance = balance + 1000
WHERE account_id = 102;

COMMIT;
```

Both operations belong to the same transaction.

### Locks

Locks help **prevent conflicts when multiple users access the same data**.

They ensure that simultaneous database operations do not incorrectly modify the same data.

### Consistency

Transaction management helps ensure that each user sees **consistent and committed data**.

---

## Quick Revision

| Topic              | Key Point                                     |
| ------------------ | --------------------------------------------- |
| Database           | Organized collection of information           |
| DBMS               | Manages storage and retrieval of data         |
| Hierarchical Model | Parent → Child / Tree structure               |
| Network Model      | Many-to-many relationships                    |
| Relational Model   | Data stored in tables                         |
| Row                | Tuple / Record                                |
| Column             | Attribute                                     |
| RDBMS              | Relational database management system         |
| Schema             | Logical collection of objects owned by a user |
| Table              | Stores data in rows and columns               |
| Index              | Speeds up data retrieval                      |
| SQL                | Queries and manages data                      |
| PL/SQL             | Procedural extension of SQL                   |
| Transaction        | Group of SQL statements treated as one unit   |
| Locks              | Prevent conflicts during concurrent access    |
| Consistency        | Ensures users see valid, committed data       |
