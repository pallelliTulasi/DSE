# PL/SQL MERGE Statement

## Overview

The **MERGE** statement is a DML (Data Manipulation Language) command used to **insert new rows** or **update existing rows** in a target table based on matching conditions with a source table.

It combines the functionality of **INSERT** and **UPDATE** into a single statement, making data synchronization more efficient.

> **Key Point:** The `MERGE` statement depends on an **equi-join condition** to determine whether a row already exists in the target table.

---

# Purpose of MERGE

The MERGE statement is commonly used to:

- Synchronize data between two tables.
- Update existing records.
- Insert new records if they do not exist.
- Reduce the need for separate INSERT and UPDATE statements.

---

# Syntax

```sql
MERGE INTO target_table target_alias
USING source_table source_alias
ON (join_condition)

WHEN MATCHED THEN
    UPDATE SET
        column1 = value1,
        column2 = value2

WHEN NOT MATCHED THEN
    INSERT (column_list)
    VALUES (value_list);
```

---

# Components of MERGE Statement

| Clause | Description |
|---------|-------------|
| MERGE INTO | Specifies the target table to update or insert into. |
| USING | Specifies the source table or query. |
| ON | Defines the matching (equi-join) condition. |
| WHEN MATCHED | Executes UPDATE when matching rows exist. |
| WHEN NOT MATCHED | Executes INSERT when no matching row exists. |

---

# Sample Tables

## Source Table (employee)

| EMPLOYEE_ID | FIRST_NAME | LAST_NAME |
|-------------|------------|-----------|
|101|John|Smith|
|102|David|Miller|
|103|Alice|Brown|

---

## Target Table (copy_emp)

| EMPNO | FIRST_NAME | LAST_NAME |
|--------|------------|-----------|
|101|John|Taylor|
|104|James|Wilson|

---

# Example

```sql
BEGIN

    MERGE INTO copy_emp c
    USING employee e
    ON (e.employee_id = c.empno)

    WHEN MATCHED THEN
        UPDATE SET
            c.first_name = e.first_name,
            c.last_name  = e.last_name

    WHEN NOT MATCHED THEN
        INSERT (empno, first_name, last_name)
        VALUES (e.employee_id, e.first_name, e.last_name);

END;
/
```

---

# Explanation of the Example

### MERGE INTO

```sql
MERGE INTO copy_emp c
```

- Specifies **copy_emp** as the target table.
- `c` is the alias for the target table.

---

### USING

```sql
USING employee e
```

- Specifies **employee** as the source table.
- `e` is the alias for the source table.

---

### ON

```sql
ON (e.employee_id = c.empno)
```

- Defines the **matching condition**.
- Oracle compares `employee.employee_id` with `copy_emp.empno`.
- If they are equal, the row is considered a match.

---

### WHEN MATCHED THEN

```sql
WHEN MATCHED THEN
UPDATE SET
    c.first_name = e.first_name,
    c.last_name  = e.last_name
```

- Executes when matching records are found.
- Updates the existing row in the target table.

---

### WHEN NOT MATCHED THEN

```sql
WHEN NOT MATCHED THEN
INSERT (empno, first_name, last_name)
VALUES (e.employee_id, e.first_name, e.last_name);
```

- Executes when no matching record exists.
- Inserts a new row into the target table.

---

# Before MERGE

## employee (Source)

| EMPLOYEE_ID | FIRST_NAME | LAST_NAME |
|-------------|------------|-----------|
|101|John|Smith|
|102|David|Miller|
|103|Alice|Brown|

---

## copy_emp (Target)

| EMPNO | FIRST_NAME | LAST_NAME |
|--------|------------|-----------|
|101|John|Taylor|
|104|James|Wilson|

---

# After MERGE

| EMPNO | FIRST_NAME | LAST_NAME |
|--------|------------|-----------|
|101|John|Smith|
|102|David|Miller|
|103|Alice|Brown|
|104|James|Wilson|

### Explanation

- Employee **101** already existed, so it was **updated**.
- Employees **102** and **103** did not exist, so they were **inserted**.
- Employee **104** remained unchanged because it was not present in the source table.

---

# MERGE Flow

```
                Source Table
                     │
                     ▼
             Compare Using ON
                     │
         ┌───────────┴───────────┐
         │                       │
      Match Found           No Match
         │                       │
         ▼                       ▼
     UPDATE Row             INSERT Row
```

