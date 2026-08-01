# Retrieving Data and Naming Conventions in PL/SQL

## Overview

In PL/SQL, data is retrieved from database tables using the **SELECT...INTO** statement.

Unlike standard SQL, a standalone `SELECT` statement is **not allowed** inside a PL/SQL block. The retrieved value must be stored in one or more variables using the **INTO** clause.

PL/SQL also follows **naming conventions** that make programs easier to read, understand, and maintain.

---

# 1. Retrieving Data in PL/SQL

## Purpose

The **SELECT...INTO** statement is used to retrieve data from a database table and store it into PL/SQL variables.

It is used when the query returns **exactly one row**.

---

# Syntax

```sql
SELECT column_name
INTO variable_name
FROM table_name
WHERE condition;
```

---

# Rules for SELECT...INTO

The following rules must be followed while using the `SELECT...INTO` statement:

- The number of selected columns must be equal to the number of variables in the `INTO` clause.
- The query must return **exactly one row**.
- If no rows are returned, Oracle raises the **NO_DATA_FOUND** exception.
- If more than one row is returned, Oracle raises the **TOO_MANY_ROWS** exception.

---

# Example 1: Retrieve a Single Value

```sql
DECLARE
    v_name emp.first_name%TYPE;
BEGIN
    SELECT first_name
    INTO v_name
    FROM emp
    WHERE eid = 101;

    DBMS_OUTPUT.PUT_LINE(v_name);
END;
/
```

### Result

The employee's **first name** is retrieved and stored in the variable `v_name`.

---

# Example 2: Retrieve Multiple Columns

```sql
DECLARE
    v_name emp.first_name%TYPE;
    v_salary emp.sal%TYPE;
BEGIN
    SELECT first_name, sal
    INTO v_name, v_salary
    FROM emp
    WHERE eid = 101;

    DBMS_OUTPUT.PUT_LINE(v_name || ' ' || v_salary);
END;
/
```

### Result

Both the employee's name and salary are stored in PL/SQL variables.

---

# Example 3: Using %TYPE

```sql
DECLARE
    v_salary emp.sal%TYPE;
BEGIN
    SELECT sal
    INTO v_salary
    FROM emp
    WHERE eid = 102;

    DBMS_OUTPUT.PUT_LINE(v_salary);
END;
/
```

### Result

The variable automatically uses the same datatype as the `sal` column.

---

# Exceptions in SELECT...INTO

## NO_DATA_FOUND

Occurs when the query returns **no rows**.

### Example

```sql
DECLARE
    v_name emp.first_name%TYPE;
BEGIN
    SELECT first_name
    INTO v_name
    FROM emp
    WHERE eid = 999;
END;
/
```

### Result

```
ORA-01403: no data found
```

---

## TOO_MANY_ROWS

Occurs when the query returns **more than one row**.

### Example

```sql
DECLARE
    v_name emp.first_name%TYPE;
BEGIN
    SELECT first_name
    INTO v_name
    FROM emp
    WHERE dept_id = 10;
END;
/
```

### Result

```
ORA-01422: exact fetch returns more than requested number of rows
```

---

# Naming Conventions in PL/SQL

## Overview

Naming conventions are standard prefixes used to identify the type and scope of PL/SQL objects.

Using meaningful names improves code readability and maintenance.

---

# Common Naming Prefixes

| Prefix | Meaning | Example |
|---------|---------|---------|
| `v_` | Local Variable | `v_name`, `v_salary` |
| `g_` | Global Variable | `g_total` |
| `p_` | Parameter | `p_empid` |
| `c_` | Constant | `c_pi` |

---

# Variable Prefix (`v_`)

Used for **local variables**.

### Example

```sql
DECLARE
    v_name VARCHAR2(30);
BEGIN
    v_name := 'Tulasi';

    DBMS_OUTPUT.PUT_LINE(v_name);
END;
/
```

---

# Global Variable Prefix (`g_`)

Used for **global variables**, usually declared in packages.

### Example

```sql
g_total NUMBER := 1000;
```

---

# Parameter Prefix (`p_`)

Used for parameters passed to procedures or functions.

### Example

```sql
CREATE OR REPLACE PROCEDURE display_emp
(
    p_empid NUMBER
)
IS
BEGIN
    DBMS_OUTPUT.PUT_LINE(p_empid);
END;
/
```

---

# Constant Prefix (`c_`)

Used for constants whose values never change.

### Example

```sql
DECLARE
    c_pi CONSTANT NUMBER := 3.14159;
BEGIN
    DBMS_OUTPUT.PUT_LINE(c_pi);
END;
/
```
