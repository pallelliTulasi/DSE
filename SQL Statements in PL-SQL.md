# SQL Statements in PL/SQL

## Overview

PL/SQL allows SQL statements to be executed within procedural blocks. However, not all SQL statements are executed in the same way.

- **DML** and **TCL** statements can be written directly inside a PL/SQL block.
- **DDL** and **DCL** statements cannot be written directly; they must be executed using **Dynamic SQL** with the `EXECUTE IMMEDIATE` statement.

---

# Types of SQL Statements in PL/SQL

| SQL Statement Type | Can Be Used Directly in PL/SQL? | Example Commands |
|--------------------|---------------------------------|------------------|
| DML (Data Manipulation Language) | ✅ Yes | INSERT, UPDATE, DELETE, SELECT, MERGE |
| DDL (Data Definition Language) | ❌ No (Use Dynamic SQL) | CREATE, ALTER, DROP, TRUNCATE |
| TCL (Transaction Control Language) | ✅ Yes | COMMIT, ROLLBACK, SAVEPOINT |
| DCL (Data Control Language) | ❌ No (Use Dynamic SQL) | GRANT, REVOKE |

---

# 1. DML (Data Manipulation Language)

## Purpose

DML statements are used to insert, update, delete, retrieve, and merge data in database tables.

These statements can be written **directly inside a PL/SQL block**.

---

## DML Commands

- INSERT
- SELECT
- UPDATE
- DELETE
- MERGE

---

## Example 1: UPDATE Statement

```sql
BEGIN
    UPDATE emp
    SET sal = sal + 1000
    WHERE eid = 101;
END;
/
```

### Result

The salary of employee **101** is increased by **1000**.

---

## Example 2: INSERT Statement

```sql
BEGIN
    INSERT INTO emp(eid, ename, sal)
    VALUES (105, 'Rahul', 30000);
END;
/
```

### Result

A new employee record is inserted.

---

## Example 3: DELETE Statement

```sql
BEGIN
    DELETE FROM emp
    WHERE eid = 105;
END;
/
```

### Result

The employee record with **EID = 105** is deleted.

---

## Example 4: SELECT INTO Statement

```sql
DECLARE
    v_name emp.ename%TYPE;
BEGIN
    SELECT ename
    INTO v_name
    FROM emp
    WHERE eid = 101;

    DBMS_OUTPUT.PUT_LINE(v_name);
END;
/
```

### Result

Displays the employee name.

---

# 2. DDL (Data Definition Language)

## Purpose

DDL statements are used to create and modify database objects such as tables, indexes, and views.

DDL statements **cannot be written directly** inside PL/SQL blocks.

They must be executed using **Dynamic SQL**.

---

## DDL Commands

- CREATE
- ALTER
- DROP
- TRUNCATE

---

## Dynamic SQL

Dynamic SQL allows SQL statements to be executed at runtime using the `EXECUTE IMMEDIATE` statement.

### Syntax

```sql
EXECUTE IMMEDIATE 'SQL Statement';
```

---

## Example 1: CREATE TABLE

```sql
BEGIN
    EXECUTE IMMEDIATE
    'CREATE TABLE test (
        id NUMBER
    )';
END;
/
```

### Result

Creates a table named **test**.

---

## Example 2: ALTER TABLE

```sql
BEGIN
    EXECUTE IMMEDIATE
    'ALTER TABLE test ADD name VARCHAR2(30)';
END;
/
```

### Result

Adds a new column **name**.

---

## Example 3: DROP TABLE

```sql
BEGIN
    EXECUTE IMMEDIATE
    'DROP TABLE test';
END;
/
```

### Result

Deletes the **test** table.

---

## Example 4: TRUNCATE TABLE

```sql
BEGIN
    EXECUTE IMMEDIATE
    'TRUNCATE TABLE emp';
END;
/
```

### Result

Deletes all records from the **emp** table.

---

# 3. TCL (Transaction Control Language)

## Purpose

TCL statements manage database transactions.

These statements can be written **directly inside PL/SQL blocks**.

---

## TCL Commands

- COMMIT
- ROLLBACK
- SAVEPOINT

---

## Example 1: COMMIT

```sql
BEGIN
    UPDATE emp
    SET sal = sal + 500;

    COMMIT;
END;
/
```

### Result

The salary update is permanently saved.

---

## Example 2: ROLLBACK

```sql
BEGIN
    UPDATE emp
    SET sal = sal + 500;

    ROLLBACK;
END;
/
```

### Result

The salary update is cancelled.

---

## Example 3: SAVEPOINT

```sql
BEGIN
    SAVEPOINT before_update;

    UPDATE emp
    SET sal = sal + 500;

    ROLLBACK TO before_update;
END;
/
```

### Result

The transaction rolls back to the specified savepoint.

---

# 4. DCL (Data Control Language)

## Purpose

DCL statements are used to control user access and permissions.

Like DDL statements, DCL statements **cannot be written directly** inside PL/SQL blocks.

They must be executed using **Dynamic SQL**.

---

## DCL Commands

- GRANT
- REVOKE

---

## Example 1: GRANT

```sql
BEGIN
    EXECUTE IMMEDIATE
    'GRANT SELECT ON emp TO scott';
END;
/
```

### Result

Grants **SELECT** permission on the **emp** table to user **SCOTT**.

---

## Example 2: REVOKE

```sql
BEGIN
    EXECUTE IMMEDIATE
    'REVOKE SELECT ON emp FROM scott';
END;
/
```

### Result

Removes the **SELECT** permission from user **SCOTT**.

---

# Dynamic SQL Using EXECUTE IMMEDIATE

## Purpose

`EXECUTE IMMEDIATE` executes SQL statements dynamically during program execution.

### Syntax

```sql
EXECUTE IMMEDIATE 'SQL Statement';
```

### Used For

- CREATE
- ALTER
- DROP
- TRUNCATE
- GRANT
- REVOKE

---



