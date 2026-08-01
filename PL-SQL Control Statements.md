# PL/SQL Control Statements

## Overview

Control statements in PL/SQL are used to control the flow of program execution based on conditions.

They allow a program to make decisions and execute different blocks of code depending on whether a condition is **TRUE** or **FALSE**.

The primary control statements in PL/SQL are:

- IF Statement
- IF-THEN-ELSE Statement
- IF-THEN-ELSIF Statement
- CASE Expression

---

# Types of Control Statements

| Control Statement | Purpose |
|-------------------|---------|
| IF | Executes a block if a condition is TRUE |
| IF-THEN-ELSE | Executes one block if TRUE and another if FALSE |
| IF-THEN-ELSIF | Checks multiple conditions sequentially |
| CASE | Selects one of several alternatives based on a value |

---

# 1. IF Statement

## Purpose

The **IF** statement executes a block of code only when the specified condition evaluates to **TRUE**.

If the condition is FALSE, the statements inside the IF block are skipped.

---

## Syntax

```sql
IF condition THEN
    statement1;
    statement2;
    ...
END IF;
```

---

## Example

```sql
SET SERVEROUTPUT ON;

DECLARE
    v_num NUMBER := 9;
BEGIN

    IF v_num < 10 THEN
        DBMS_OUTPUT.PUT_LINE('Inside IF');
    END IF;

    DBMS_OUTPUT.PUT_LINE('Outside IF');

END;
/
```

### Output

```
Inside IF
Outside IF
```

---

# 2. IF-THEN-ELSE Statement

## Purpose

The **IF-THEN-ELSE** statement executes one block when the condition is TRUE and another block when the condition is FALSE.

---

## Syntax

```sql
IF condition THEN
    statements;

ELSE
    statements;

END IF;
```

---

## Example

```sql
SET SERVEROUTPUT ON;

DECLARE
    v_num NUMBER := 2;
BEGIN

    IF MOD(v_num,2)=0 THEN
        DBMS_OUTPUT.PUT_LINE(v_num || ' IS Even');
    ELSE
        DBMS_OUTPUT.PUT_LINE(v_num || ' IS Odd');
    END IF;

    DBMS_OUTPUT.PUT_LINE('Executed');

END;
/
```

### Output

```
2 IS Even
Executed
```

---

# 3. IF-THEN-ELSIF Statement

## Purpose

The **IF-THEN-ELSIF** statement is used when multiple conditions need to be evaluated one after another.

The first condition that evaluates to TRUE is executed.

If none of the conditions are TRUE, the ELSE block is executed.

---

## Syntax

```sql
IF condition1 THEN
    statements;

ELSIF condition2 THEN
    statements;

ELSIF condition3 THEN
    statements;

ELSE
    statements;

END IF;
```

---

## Example

```sql
SET SERVEROUTPUT ON;

DECLARE
    v_marks NUMBER := 82;
BEGIN

    IF v_marks >= 90 THEN
        DBMS_OUTPUT.PUT_LINE('Grade A');

    ELSIF v_marks >= 75 THEN
        DBMS_OUTPUT.PUT_LINE('Grade B');

    ELSIF v_marks >= 60 THEN
        DBMS_OUTPUT.PUT_LINE('Grade C');

    ELSE
        DBMS_OUTPUT.PUT_LINE('Fail');

    END IF;

END;
/
```

### Output

```
Grade B
```

---

# 4. CASE Expression

## Purpose

A **CASE** expression evaluates a single variable or expression against multiple possible values and returns the corresponding result for the first matching condition.

CASE expressions improve readability when multiple values are compared.

---

## Syntax

```sql
CASE expression

    WHEN value1 THEN result1

    WHEN value2 THEN result2

    ...

    ELSE default_result

END;
```

---

## Example

```sql
SET VERIFY OFF;

DECLARE

    v_grade CHAR(1) := UPPER('&grade');

    v_appraisal VARCHAR2(20);

BEGIN

    v_appraisal := CASE v_grade

        WHEN 'A' THEN 'Excellent'

        WHEN 'B' THEN 'Very Good'

        WHEN 'C' THEN 'Good'

        ELSE 'No Such Grade'

    END;

    DBMS_OUTPUT.PUT_LINE('Grade : ' || v_grade);

    DBMS_OUTPUT.PUT_LINE('Appraisal : ' || v_appraisal);

END;
/
```

### Sample Output

```
Enter value for grade: A

Grade : A
Appraisal : Excellent
```

---

# CASE vs IF-ELSIF

| IF-ELSIF | CASE |
|-----------|------|
| Used for complex conditions | Used for comparing one expression with multiple values |
| Can use multiple logical conditions | Compares only one expression |
| More flexible | Easier to read |

---

# Complete Example

```sql
SET SERVEROUTPUT ON;

DECLARE

    v_marks NUMBER := 95;

BEGIN

    IF v_marks >= 90 THEN

        DBMS_OUTPUT.PUT_LINE('Excellent');

    ELSIF v_marks >= 75 THEN

        DBMS_OUTPUT.PUT_LINE('Very Good');

    ELSIF v_marks >= 60 THEN

        DBMS_OUTPUT.PUT_LINE('Good');

    ELSE

        DBMS_OUTPUT.PUT_LINE('Need Improvement');

    END IF;

END;
/
```

