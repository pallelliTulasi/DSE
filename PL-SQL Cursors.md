# PL/SQL Cursors

## Overview

A **cursor** is a pointer to the **context area (memory area)** where Oracle stores the result of an SQL statement.

A cursor allows PL/SQL to process the rows returned by a **SELECT** statement **one row at a time**.

### Why Do We Need Cursors?

- Process query results row by row.
- Retrieve multiple records from a SELECT statement.
- Perform row-by-row operations.
- Improve control over data retrieval.

---

# Types of PL/SQL Cursors

There are two types of cursors in PL/SQL.

| Cursor Type | Description |
|-------------|-------------|
| Implicit Cursor | Automatically created by Oracle for DML statements and SELECT INTO. |
| Explicit Cursor | Created by the programmer to process multiple rows returned by a SELECT statement. |

---

# 1. Implicit Cursor

## Overview

An **Implicit Cursor** is automatically created by Oracle whenever an SQL statement is executed.

The programmer does not need to declare, open, fetch, or close it.

### Used With

- INSERT
- UPDATE
- DELETE
- SELECT ... INTO

---

## Implicit Cursor Attributes

| Attribute | Description |
|-----------|-------------|
| SQL%FOUND | Returns TRUE if one or more rows are affected. |
| SQL%NOTFOUND | Returns TRUE if no rows are affected. |
| SQL%ROWCOUNT | Returns the number of rows affected. |
| SQL%ISOPEN | Always returns FALSE because Oracle automatically closes the cursor. |

---

## Example 1: UPDATE Statement

```sql
DECLARE
    total NUMBER;
BEGIN
    UPDATE emp
    SET sal = sal + 1000;

    total := SQL%ROWCOUNT;

    DBMS_OUTPUT.PUT_LINE(total || ' Rows Updated');
END;
/
```

### Output

```
14 Rows Updated
```

*(Output depends on the number of rows in the table.)*

---

## Example 2: INSERT Statement

```sql
BEGIN
    INSERT INTO emp(empno, ename, sal)
    VALUES (9999, 'RAJU', 25000);

    DBMS_OUTPUT.PUT_LINE(SQL%ROWCOUNT || ' Row Inserted');
END;
/
```

---

## Example 3: DELETE Statement

```sql
BEGIN
    DELETE FROM emp
    WHERE deptno = 30;

    DBMS_OUTPUT.PUT_LINE(SQL%ROWCOUNT || ' Rows Deleted');
END;
/
```

---

# 2. Explicit Cursor

## Overview

An **Explicit Cursor** is created by the programmer to retrieve multiple rows returned by a SELECT statement.

Unlike implicit cursors, explicit cursors must be managed manually.

---

# Syntax

```sql
CURSOR cursor_name IS
SELECT column_list
FROM table_name;
```

---

# Life Cycle of an Explicit Cursor

An explicit cursor follows four steps.

```
Declare
   ↓
Open
   ↓
Fetch
   ↓
Close
```

| Step | Purpose |
|------|---------|
| Declare | Defines the cursor and SQL query. |
| Open | Executes the query and creates the result set. |
| Fetch | Retrieves one row at a time. |
| Close | Releases the memory occupied by the cursor. |

---

# Step 1: Declare Cursor

## Syntax

```sql
CURSOR cursor_name IS
SELECT column_list
FROM table_name;
```

### Example

```sql
CURSOR c_emp IS
SELECT empno, ename
FROM emp;
```

---

# Step 2: Open Cursor

## Syntax

```sql
OPEN cursor_name;
```

### Example

```sql
OPEN c_emp;
```

---

# Step 3: Fetch Rows

## Syntax

```sql
FETCH cursor_name
INTO variable1, variable2;
```

### Example

```sql
FETCH c_emp
INTO v_empno, v_name;
```

---

# Step 4: Close Cursor

## Syntax

```sql
CLOSE cursor_name;
```

### Example

```sql
CLOSE c_emp;
```

---

# Complete Example

```sql
DECLARE

    v_empno emp.empno%TYPE;
    v_name  emp.ename%TYPE;

    CURSOR c_emp IS
        SELECT empno, ename
        FROM emp;

BEGIN

    OPEN c_emp;

    LOOP

        FETCH c_emp
        INTO v_empno, v_name;

        EXIT WHEN c_emp%NOTFOUND;

        DBMS_OUTPUT.PUT_LINE(v_empno || ' ' || v_name);

    END LOOP;

    CLOSE c_emp;

END;
/
```

### Sample Output

```
7369 SMITH
7499 ALLEN
7521 WARD
7566 JONES
7654 MARTIN
...
```

---

# Explicit Cursor Attributes

| Attribute | Description |
|-----------|-------------|
| cursor_name%FOUND | Returns TRUE if the last FETCH returned a row. |
| cursor_name%NOTFOUND | Returns TRUE when no more rows are available. |
| cursor_name%ROWCOUNT | Returns the number of rows fetched so far. |
| cursor_name%ISOPEN | Returns TRUE if the cursor is currently open. |

---

# Example Using Cursor Attributes

```sql
DECLARE

    CURSOR c_emp IS
    SELECT empno
    FROM emp;

    v_empno emp.empno%TYPE;

BEGIN

    OPEN c_emp;

    LOOP

        FETCH c_emp INTO v_empno;

        EXIT WHEN c_emp%NOTFOUND;

        DBMS_OUTPUT.PUT_LINE(v_empno);

    END LOOP;

    DBMS_OUTPUT.PUT_LINE('Rows Fetched : ' || c_emp%ROWCOUNT);

    CLOSE c_emp;

END;
/
```

---

# Difference Between Implicit and Explicit Cursor

| Feature | Implicit Cursor | Explicit Cursor |
|----------|-----------------|-----------------|
| Created By | Oracle | Programmer |
| Declaration Required | No | Yes |
| Open Required | No | Yes |
| Fetch Required | No | Yes |
| Close Required | No | Yes |
| Used For | Single-row operations and DML | Multiple-row SELECT statements |
| Managed By | Oracle | Programmer |

---

# Advantages of Cursors

- Retrieves data row by row.
- Provides better control over query processing.
- Useful for processing multiple records.
- Supports complex business logic.
- Improves readability in procedural programming.

---

# Disadvantages of Cursors

- Slower than set-based SQL operations.
- Consumes additional memory.
- Requires more code for explicit cursors.
- Can reduce performance when processing very large datasets.

---

# Interview Questions

### 1. What is a cursor in PL/SQL?

A cursor is a pointer to the context area that stores the result of an SQL statement. It is used to process query results one row at a time.

---

### 2. What are the two types of cursors?

- Implicit Cursor
- Explicit Cursor

---

### 3. Who creates an implicit cursor?

Oracle automatically creates an implicit cursor.

---

### 4. Who creates an explicit cursor?

The programmer creates an explicit cursor.

---

### 5. What are the four steps in the life cycle of an explicit cursor?

1. Declare
2. Open
3. Fetch
4. Close

---

### 6. Name the implicit cursor attributes.

- SQL%FOUND
- SQL%NOTFOUND
- SQL%ROWCOUNT
- SQL%ISOPEN

---

### 7. Name the explicit cursor attributes.

- cursor_name%FOUND
- cursor_name%NOTFOUND
- cursor_name%ROWCOUNT
- cursor_name%ISOPEN

---

### 8. What is the purpose of FETCH?

The **FETCH** statement retrieves one row at a time from an open cursor.

---

### 9. What does `%ROWCOUNT` return?

It returns the number of rows processed (implicit cursor) or fetched (explicit cursor).

---

### 10. When should explicit cursors be used?

Explicit cursors should be used when a **SELECT** statement returns **multiple rows** that need to be processed one by one.

---

# Conclusion

PL/SQL cursors are used to process the result of SQL queries efficiently.

- **Implicit cursors** are automatically managed by Oracle and are suitable for DML statements and single-row queries.
- **Explicit cursors** are programmer-defined and are ideal for processing multiple rows returned by a SELECT statement.

Understanding cursor types, lifecycle, and cursor attributes is essential for writing efficient and reliable PL/SQL programs.
