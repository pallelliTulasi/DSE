# PL/SQL Bind Variables

## Overview

A **Bind Variable** is a variable that is created in **SQL\*Plus** (or other Oracle client tools) and can be referenced inside SQL statements and PL/SQL blocks.

Bind variables are stored in memory and can be reused multiple times without being declared again inside every PL/SQL block.

> **Note:** Whenever a bind variable is used in SQL or PL/SQL, it must be prefixed with a **colon (`:`)**.

Example:

```sql
:v_bind1
```

---

# Why Use Bind Variables?

- Improve performance by reusing SQL statements.
- Reduce memory usage.
- Avoid repeated parsing of SQL statements.
- Allow communication between SQL*Plus and PL/SQL blocks.
- Useful for passing values between multiple PL/SQL blocks.

---

# Life Cycle of a Bind Variable

A bind variable is generally used in the following steps:

```
Declare
   ↓
Initialize
   ↓
Use
   ↓
Display
```

---

# 1. Declaring a Bind Variable

## Purpose

A bind variable is declared using the **VARIABLE** command in SQL*Plus.

### Syntax

```sql
VARIABLE variable_name datatype;
```

### Example

```sql
VARIABLE v_bind1 VARCHAR2(10);
```

### Result

A bind variable named **v_bind1** is created.

---

# 2. Initializing a Bind Variable

A bind variable can be assigned a value in two different ways.

---

## Method 1: Using EXEC

### Syntax

```sql
EXEC :variable_name := value;
```

### Example

```sql
EXEC :v_bind1 := 'Tulasi';
```

### Result

The value **Tulasi** is assigned to the bind variable.

---

## Method 2: Using a PL/SQL Block

### Example

```sql
BEGIN
    :v_bind1 := 'Tulasi';
END;
/
```

### Result

The bind variable is initialized inside the PL/SQL block.

---

# 3. Using Bind Variables

Bind variables can be used inside SQL statements and PL/SQL blocks.

### Example

```sql
VARIABLE v_salary NUMBER;

BEGIN
    :v_salary := 50000;

    DBMS_OUTPUT.PUT_LINE(:v_salary);
END;
/
```

---

# Displaying Bind Variables

There are three common ways to display the value of a bind variable.

---

# Method 1: Using DBMS_OUTPUT

### Example

```sql
BEGIN
    :v_bind1 := 'Tulasi';

    DBMS_OUTPUT.PUT_LINE(:v_bind1);
END;
/
```

### Output

```
Tulasi
```

---

# Method 2: Using PRINT

The **PRINT** command displays the current value of a bind variable.

### Syntax

```sql
PRINT :variable_name;
```

### Example

```sql
PRINT :v_bind1;
```

### Output

```
V_BIND1
-------
Tulasi
```

---

# Method 3: Using AUTOPRINT

The **AUTOPRINT** option automatically displays bind variable values after successful execution of a PL/SQL block.

### Enable AUTOPRINT

```sql
SET AUTOPRINT ON;
```

### Example

```sql
VARIABLE v_name VARCHAR2(20);

BEGIN
    :v_name := 'Tulasi';
END;
/
```

### Output

```
V_NAME
------
Tulasi
```

---

# Complete Example

```sql
VARIABLE v_name VARCHAR2(20);

BEGIN

    :v_name := 'Tulasi';

    DBMS_OUTPUT.PUT_LINE(:v_name);

END;
/

PRINT :v_name;
```

### Output

```
Tulasi

V_NAME
------
Tulasi
```

---

# Bind Variable vs PL/SQL Variable

| Feature | Bind Variable | PL/SQL Variable |
|----------|---------------|-----------------|
| Declared Using | `VARIABLE` command | `DECLARE` section |
| Scope | SQL*Plus Session | Current PL/SQL Block |
| Prefix | `:` (Colon) | No Prefix |
| Accessible Outside Block | Yes | No |
| Lifetime | Until SQL*Plus session ends | Until block execution ends |

