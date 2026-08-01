# PL/SQL Iterative Statements (Loops)

## Overview

Iterative statements (Loops) in PL/SQL are used to execute a block of code repeatedly until a specified condition is met.

Loops help reduce repetitive code and make programs more efficient.

The main types of loops in PL/SQL are:

- Simple LOOP
- WHILE LOOP
- FOR LOOP
  - Numeric FOR LOOP
  - Cursor FOR LOOP

---

# Types of Loops

| Loop | Purpose |
|------|---------|
| Simple LOOP | Repeats until an EXIT statement is encountered |
| WHILE LOOP | Repeats while a condition is TRUE |
| Numeric FOR LOOP | Executes for a fixed range of numbers |
| Cursor FOR LOOP | Iterates through rows returned by a cursor |

---

# 1. Simple LOOP

## Purpose

A **Simple LOOP** repeatedly executes a block of statements until an **EXIT** statement is encountered.

The condition is checked **inside** the loop.

---

## Syntax

```sql
LOOP

    statements;

    EXIT WHEN condition;

END LOOP;
```

or

```sql
LOOP

    statements;

    IF condition THEN
        EXIT;
    END IF;

END LOOP;
```

---

## Example

```sql
SET SERVEROUTPUT ON;

DECLARE

    v_counter NUMBER := 0;
    v_result NUMBER;

BEGIN

    LOOP

        v_counter := v_counter + 1;

        v_result := 19 * v_counter;

        DBMS_OUTPUT.PUT_LINE('19 x ' || v_counter || ' = ' || v_result);

        IF v_counter >= 10 THEN
            EXIT;
        END IF;

    END LOOP;

END;
/
```

### Output

```
19 x 1 = 19
19 x 2 = 38
...
19 x 10 = 190
```

---

## Using EXIT WHEN

The same example can be written using `EXIT WHEN`.

```sql
LOOP

    v_counter := v_counter + 1;

    DBMS_OUTPUT.PUT_LINE(v_counter);

    EXIT WHEN v_counter >= 10;

END LOOP;
```

---

# 2. WHILE LOOP

## Purpose

A **WHILE LOOP** executes a block of statements **as long as** the specified condition remains TRUE.

The condition is checked **before** each iteration.

---

## Syntax

```sql
WHILE condition LOOP

    statements;

END LOOP;
```

---

## Example

```sql
SET SERVEROUTPUT ON;

DECLARE

    v_counter NUMBER := 1;
    v_res NUMBER;

BEGIN

    WHILE v_counter <= 10 LOOP

        v_res := 19 * v_counter;

        DBMS_OUTPUT.PUT_LINE(v_res);

        v_counter := v_counter + 1;

    END LOOP;

    DBMS_OUTPUT.PUT_LINE('Outside Loop');

END;
/
```

### Output

```
19
38
57
...
190
Outside Loop
```

---

# Difference Between LOOP and WHILE LOOP

| Simple LOOP | WHILE LOOP |
|-------------|------------|
| Condition checked inside the loop | Condition checked before entering the loop |
| Executes at least once | May execute zero times |
| Requires EXIT statement | Stops automatically when condition becomes FALSE |

---

# 3. FOR LOOP

## Purpose

A **FOR LOOP** executes a block of statements a fixed number of times.

Oracle automatically:

- Declares the loop variable.
- Increments or decrements it.
- Ends the loop after the last iteration.

---

# Types of FOR LOOP

- Numeric FOR LOOP
- Cursor FOR LOOP

---

# A. Numeric FOR LOOP

## Purpose

Executes statements for a specified range of integer values.

---

## Syntax

```sql
FOR counter IN lower..upper LOOP

    statements;

END LOOP;
```

---

## Example

```sql
SET SERVEROUTPUT ON;

BEGIN

    FOR v_counter IN 1..10 LOOP

        DBMS_OUTPUT.PUT_LINE(v_counter);

    END LOOP;

END;
/
```

### Output

```
1
2
3
4
5
6
7
8
9
10
```

---

# Reverse Numeric FOR LOOP

The **REVERSE** keyword executes the loop in descending order.

---

## Syntax

```sql
FOR counter IN REVERSE lower..upper LOOP

    statements;

END LOOP;
```

---

## Example

```sql
SET SERVEROUTPUT ON;

BEGIN

    FOR v_counter IN REVERSE 1..10 LOOP

        DBMS_OUTPUT.PUT_LINE(v_counter);

    END LOOP;

END;
/
```

### Output

```
10
9
8
7
6
5
4
3
2
1
```

---

# B. Cursor FOR LOOP

## Purpose

A **Cursor FOR LOOP** automatically:

- Opens the cursor.
- Fetches each row.
- Stores each row into a record variable.
- Closes the cursor after processing.

No explicit OPEN, FETCH, or CLOSE statements are required.

---

## Syntax

```sql
DECLARE

    CURSOR cursor_name IS
        SELECT columns
        FROM table;

BEGIN

    FOR record IN cursor_name LOOP

        statements;

    END LOOP;

END;
/
```

---

## Example

```sql
SET SERVEROUTPUT ON;

DECLARE

    CURSOR cur_Tulasi IS
        SELECT first_name,
               last_name
        FROM employees
        WHERE employee_id > 200;

BEGIN

    FOR l_idx IN cur_Tulasi LOOP

        DBMS_OUTPUT.PUT_LINE
        (
            l_idx.first_name || ' ' ||
            l_idx.last_name
        );

    END LOOP;

END;
/
```

### Output

```
Steven King
Neena Kochhar
Lex De Haan
...
```
d.
