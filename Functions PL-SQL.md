
# PL/SQL Functions

- a function is a named PL/SQL block that performs a specific task and always returns exactly one value using RETURN statement.

**Syntax:**
```sql
CREATE OR REPLACE FUNCTION fn_name
(parameter_name datatype)
RETURN datatype
IS
BEGIN
    -- statements
    RETURN value;
END;
/
```

**Example!**
```sql
Create or replace function findmax(x NUMBER, y NUMBER)
RETURN NUMBER
IS
BEGIN
    if x > y THEN
        RETURN x;
    ELSE
        RETURN y;
    END if;
END;
/
```

**Calling fn**
```sql
DECLARE
    result NUMBER;
BEGIN
    result := findmax(10, 20);
    DBMS_OUTPUT.PUT_LINE(result);
END;
/
```

**Parameters of functions**
`IN`, `OUT`, `IN OUT`
Eg: `FUNCTION calSal(empid IN NUMBER) RETURN NUMBER.`

**Recursion function (calls itself).**
eg: (fact, fibonacci)
```sql
FUNCTION fact(n NUMBER)
RETURN NUMBER
```

---

# Procedures and Functions:-

## Anonymous Blocks vs Subprograms:

| Anonymous Blocks | Subprograms |
| :--- | :--- |
| -> These are unnamed executable PL/SQL blocks. | -> procedures & fns are named PL/SQL blocks. |
| -> They can neither be reused nor stored for later use. | -> They can be declared at the schema level or within any other PL/SQL block. |
| -> compile every time | -> compile at once. |
| -> Not stored in database | -> stored in database. |
| -> can't be invoked by other applications. | -> Named & can be invoked by other applications. |
| -> do not return values. | -> if functions, must return value. |
| -> can't take parameters. | -> can take parameters. |

## Subprogram block structure:
Similar to anonymous block.
* **Declarative section (optional):** declarative section follows the `IS` or `AS` keyword in subprogram declaration.
* **Executable section (mandatory):** contains implementation of bussiness logic. easily determine business functionality of subprogram, use key words `BEGIN` & `END`.
* **Exception section (optional):** Included to handle exceptions.

---

* -> declarative section of procedure starts immediately after the procedure declaration.
* -> does not begin with the `DECLARE` keyword.
* -> The procedure uses the Implicit cursor attribute or the `SQL%ROWCOUNT` SQL attribute to verify that the row was successfully inserted. (1 returned in this case).

# Subprograms!
- Subprogram is a program unit or module designed to perform a specific task.
- These can be combined to create large programs, which is a practice known as modular design.
- A calling program is a program or subprogram that invokes another subprogram.

## Two types:
1. **Functions:** used primarily to compute and return a single value.
2. **Procedures:** don't return a value directly and are mainly used to perform a specific action.

## Where subprograms are created:
1. **Schema level:** `CREATE PROCEDURE` (or) `CREATE FUNCTION`. `DROP PROCEDURE` or `DROP FUNCTION`.
2. **Inside a package:** `DROP PACKAGE`.
3. **Inside a PL/SQL block:**
