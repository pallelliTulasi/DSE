# PL/SQL Introduction and Data Types

## Overview

**PL/SQL (Procedural Language/Structured Query Language)** is Oracle's procedural extension to SQL. It combines the power of SQL with procedural programming features such as variables, loops, conditions, cursors, and exception handling.

PL/SQL is a **portable**, **high-performance**, and **transaction-processing** language used to build reliable and efficient database applications.

### Features of PL/SQL

- Block-structured programming language.
- Integrates SQL with procedural statements.
- Supports variables, loops, conditions, and exceptions.
- Improves performance by reducing network traffic.
- Supports modular programming using procedures, functions, and packages.
- Provides robust error handling through exceptions.

---

# Structure of a PL/SQL Block

A PL/SQL program is organized into logical blocks. Every PL/SQL block contains up to three sections.

```
DECLARE        ← Optional
BEGIN          ← Mandatory
EXCEPTION      ← Optional
END;
```

| Section | Description | Mandatory |
|----------|-------------|-----------|
| DECLARE | Declares variables, constants, cursors, and subprograms | No |
| BEGIN | Contains executable SQL and PL/SQL statements | Yes |
| EXCEPTION | Handles runtime errors | No |
| END | Marks the end of the PL/SQL block | Yes |

---

# 1. Declaration Section

## Purpose

The declaration section begins with the **DECLARE** keyword.

It is used to declare:

- Variables
- Constants
- Cursors
- Records
- Collections
- Subprograms

### Syntax

```sql
DECLARE
   variable_name datatype;
```

### Example

```sql
DECLARE
   message VARCHAR2(20);
BEGIN
   message := 'Hello';
   DBMS_OUTPUT.PUT_LINE(message);
END;
/
```

---

# 2. Executable Section

## Purpose

The executable section starts with the **BEGIN** keyword and ends with **END**.

It contains all executable statements.

This section is **mandatory**.

### Syntax

```sql
BEGIN
   executable_statements;
END;
```

### Example

```sql
BEGIN
   DBMS_OUTPUT.PUT_LINE('Welcome to PL/SQL');
END;
/
```

---

# 3. Exception Section

## Purpose

The exception section begins with the **EXCEPTION** keyword.

It handles runtime errors that occur during execution.

This section is optional.

### Syntax

```sql
BEGIN
   executable_statements;

EXCEPTION
   WHEN exception_name THEN
      statements;
END;
```

### Example

```sql
DECLARE
   num NUMBER;
BEGIN
   num := 10/0;

EXCEPTION
   WHEN ZERO_DIVIDE THEN
      DBMS_OUTPUT.PUT_LINE('Cannot divide by zero');
END;
/
```

---

# Basic PL/SQL Syntax

```sql
DECLARE
   -- Declaration Section

BEGIN
   -- Executable Statements

EXCEPTION
   -- Exception Handling

END;
/
```

---

# Example Program

```sql
DECLARE
   message VARCHAR2(20) := 'Hello, World!';
BEGIN
   DBMS_OUTPUT.PUT_LINE(message);
END;
/
```

### Output

```
Hello, World!
```

---

# PL/SQL Data Types

## Overview

A **data type** specifies the type of value that a variable can store.

PL/SQL data types are classified into four categories.

| Category | Description | Examples |
|----------|-------------|----------|
| Scalar | Stores a single value | NUMBER, DATE, BOOLEAN |
| LOB | Stores large objects | BLOB, CLOB |
| Composite | Stores multiple values | Records, Collections |
| Reference | Stores references to other objects | REF CURSOR |

---

# 1. Scalar Data Types

Scalar data types store only one value.

---

## Numeric Data Types

### Purpose

Used to store numeric values.

### Common Numeric Data Types

- NUMBER
- PLS_INTEGER
- BINARY_INTEGER
- FLOAT
- REAL
- DOUBLE PRECISION
- DECIMAL

### Example

```sql
DECLARE
   num1 INTEGER;
   num2 NUMBER(5,2);
   num3 REAL;
BEGIN
   num1 := 100;
   num2 := 99.99;
   num3 := 25.6;

   DBMS_OUTPUT.PUT_LINE(num1);
   DBMS_OUTPUT.PUT_LINE(num2);
   DBMS_OUTPUT.PUT_LINE(num3);
END;
/
```

---

## Character Data Types

### Purpose

Used to store characters and strings.

### Common Character Data Types

- CHAR
- VARCHAR2
- NCHAR
- NVARCHAR2
- RAW
- LONG
- LONG RAW
- ROWID
- UROWID

### Example

```sql
DECLARE
   name VARCHAR2(20);
BEGIN
   name := 'Tulasi';

   DBMS_OUTPUT.PUT_LINE(name);
END;
/
```

---

## Boolean Data Type

### Purpose

Stores logical values.

### Values

- TRUE
- FALSE
- NULL

### Example

```sql
DECLARE
   flag BOOLEAN;
BEGIN
   flag := TRUE;

   IF flag THEN
      DBMS_OUTPUT.PUT_LINE('Success');
   END IF;
END;
/
```

---

## Date and Time Data Type

### Purpose

Stores date and time values.

### Data Type

- DATE

The DATE datatype stores:

- Year
- Month
- Day
- Hour
- Minute
- Second

### Example

```sql
DECLARE
   today DATE;
BEGIN
   today := SYSDATE;

   DBMS_OUTPUT.PUT_LINE(today);
END;
/
```

---

# 2. Large Object (LOB) Data Types

LOB data types are used to store very large amounts of data.

| Data Type | Description |
|-----------|-------------|
| BLOB | Stores binary data such as images, videos, audio |
| CLOB | Stores large text data |
| NCLOB | Stores large Unicode text |
| BFILE | Stores external binary files outside the database |

---

## Example

```sql
CREATE TABLE documents (
   doc_id NUMBER,
   document CLOB
);
```

---

# 3. Composite Data Types

Composite data types store multiple values together.

Examples include:

- Records
- Collections
- Associative Arrays
- Nested Tables
- VARRAYs

### Example

```sql
DECLARE
   TYPE student_record IS RECORD(
      id NUMBER,
      name VARCHAR2(20)
   );

   student student_record;

BEGIN
   student.id := 101;
   student.name := 'Tulasi';

   DBMS_OUTPUT.PUT_LINE(student.name);
END;
/
```

---

# 4. Reference Data Types

Reference data types store references to database objects.

Example:

- REF CURSOR

### Example

```sql
DECLARE
   TYPE emp_cursor IS REF CURSOR;
   c emp_cursor;
BEGIN
   NULL;
END;
/
```

---

# User-Defined Subtypes

## Purpose

A subtype is a user-defined subset of an existing data type.

It helps improve readability and consistency.

### Syntax

```sql
SUBTYPE subtype_name IS datatype;
```

### Example

```sql
DECLARE
   SUBTYPE student_name IS CHAR(20);

   name student_name;

BEGIN
   name := 'Tulasi';

   DBMS_OUTPUT.PUT_LINE(name);
END;
/
```

---

# NULL in PL/SQL

## Purpose

NULL represents an **unknown**, **missing**, or **undefined** value.

### Important Points

- NULL is not zero (0).
- NULL is not an empty string ('').
- NULL is not the character '\0'.

### Example

```sql
DECLARE
   age NUMBER;
BEGIN
   age := NULL;
END;
/
```
