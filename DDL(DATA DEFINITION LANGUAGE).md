# Data Definition Language (DDL) Operations in SQL

## Overview

Data Definition Language (DDL) is a set of SQL commands used to create, modify, rename, truncate, and delete database objects such as tables, views, indexes, and sequences.

DDL commands change the **structure (schema)** of the database rather than the data stored in it.

### Characteristics
- Defines and manages database objects.
- Changes the database schema.
- Automatically commits changes (Auto Commit).
- Cannot be rolled back in most databases.

---

# Types of DDL Operations

| Operation | Purpose |
|-----------|---------|
| CREATE | Creates new database objects. |
| ALTER | Modifies the structure of existing objects. |
| RENAME | Changes the name of a database object. |
| TRUNCATE | Removes all rows from a table without deleting the table structure. |
| DROP | Permanently deletes a database object. |

---

# 1. CREATE

## Purpose
Creates a new database object such as a table, view, index, or sequence.

### Syntax

```sql
CREATE TABLE table_name (
    column_name datatype constraints,
    ...
);
```

### Example

```sql
CREATE TABLE employee (
    emp_id NUMBER PRIMARY KEY,
    emp_name VARCHAR2(30),
    salary NUMBER(8,2),
    department VARCHAR2(20)
);
```

### Result
A new table named **employee** is created.

---

# 2. ALTER

## Purpose
Modifies the structure of an existing table.

## Types of ALTER Operations

### A. Add a New Column

```sql
ALTER TABLE employee
ADD email VARCHAR2(50);
```

**Result**

Adds a new column named **email**.

---

### B. Modify a Column

```sql
ALTER TABLE employee
MODIFY salary NUMBER(10,2);
```

**Result**

Changes the datatype/size of the **salary** column.

---

### C. Rename a Column

```sql
ALTER TABLE employee
RENAME COLUMN emp_name TO employee_name;
```

**Result**

Changes the column name.

---

### D. Drop a Column

```sql
ALTER TABLE employee
DROP COLUMN department;
```

**Result**

Deletes the **department** column permanently.

---

# 3. RENAME

## Purpose
Changes the name of an existing database object.

### Syntax

```sql
RENAME old_table_name TO new_table_name;
```

### Example

```sql
RENAME employee TO employees;
```

**Result**

Table name changes from **employee** to **employees**.

---

# 4. TRUNCATE

## Purpose
Deletes all records from a table while keeping its structure.

### Syntax

```sql
TRUNCATE TABLE employees;
```

### Example

Before:

| EMP_ID | NAME |
|--------|------|
|101|John|
|102|David|

Command:

```sql
TRUNCATE TABLE employees;
```

After:

Table exists but contains **0 rows**.

---

# 5. DROP

## Purpose
Deletes the entire database object permanently.

### Syntax

```sql
DROP TABLE employees;
```

### Example

```sql
DROP TABLE employees;
```

**Result**

- Table is deleted.
- All data is deleted.
- Table structure is removed.

---

# Complete Example

## Step 1: Create Table

```sql
CREATE TABLE student (
    sid NUMBER PRIMARY KEY,
    sname VARCHAR2(30),
    age NUMBER(2)
);
```

---

## Step 2: Add Email Column

```sql
ALTER TABLE student
ADD email VARCHAR2(50);
```

---

## Step 3: Modify Name Size

```sql
ALTER TABLE student
MODIFY sname VARCHAR2(50);
```

---

## Step 4: Rename Column

```sql
ALTER TABLE student
RENAME COLUMN sname TO student_name;
```

---

## Step 5: Drop Age Column

```sql
ALTER TABLE student
DROP COLUMN age;
```

---

## Step 6: Rename Table

```sql
RENAME student TO students;
```

---

## Step 7: Remove All Data

```sql
TRUNCATE TABLE students;
```

---

## Step 8: Delete Table

```sql
DROP TABLE students;
```

---

# Difference Between DDL Operations

| Operation | Structure | Data | Can Use WHERE | Rollback |
|-----------|-----------|------|---------------|----------|
| CREATE | Creates | No Data | No | No |
| ALTER | Modified | Existing Data Remains | No | No |
| RENAME | Name Changed | Data Remains | No | No |
| TRUNCATE | Unchanged | All Data Removed | No | No |
| DROP | Removed | Removed | No | No |

