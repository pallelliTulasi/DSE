# PL/SQL Cursor Parameters, FOR UPDATE Clause & Cursor-Based Records

## Introduction

PL/SQL cursors can be made more flexible by using **Cursor Parameters**, can lock rows using the **FOR UPDATE** clause, and can simplify data handling using **Cursor-Based Records**.

These features help write efficient and reusable PL/SQL programs.

---

# 1. Cursor Parameters

A **Parameterized Cursor** is a cursor that accepts one or more parameters.

The parameter values are supplied when the cursor is opened, allowing the same cursor to execute with different conditions.

---

## Advantages

- Makes cursors reusable.
- Eliminates the need to create multiple cursors.
- Improves code readability.
- Allows dynamic filtering of records.

---

## Syntax

```sql
CURSOR cursor_name(parameter datatype) IS
SELECT column_name
FROM table_name
WHERE condition = parameter;
```

---

## Example

```sql
SET SERVEROUTPUT ON;

DECLARE
    v_name VARCHAR2(30);

    CURSOR P_cur_Tulasi(var_e_id VARCHAR2) IS
        SELECT first_name
        FROM employee
        WHERE employee_id < var_e_id;

BEGIN
    OPEN P_cur_Tulasi(105);

    LOOP
        FETCH P_cur_Tulasi INTO v_name;

        EXIT WHEN P_cur_Tulasi%NOTFOUND;

        DBMS_OUTPUT.PUT_LINE(v_name);
    END LOOP;

    CLOSE P_cur_Tulasi;
END;
/
```

---

## Explanation

- A parameterized cursor **P_cur_Tulasi** is declared.
- It accepts one parameter `var_e_id`.
- When the cursor is opened with `105`, it returns employees whose `employee_id` is less than **105**.
- Each employee's first name is fetched and displayed.
- The loop continues until all rows are processed.
- Finally, the cursor is closed.

---

## Output (Example)

```
Steven
Neena
Lex
Alexander
Bruce
David
...
```

*(Actual output depends on the data in the Employee table.)*

---

# 2. FOR UPDATE Clause

The **FOR UPDATE** clause is used with a cursor to lock the selected rows.

The locked rows cannot be modified by other users until the current transaction is committed or rolled back.

This is useful when the program intends to update or delete the selected rows.

---

## Advantages

- Prevents other users from modifying selected rows.
- Maintains data consistency.
- Avoids conflicts during transactions.

---

## Syntax

```sql
CURSOR cursor_name IS
SELECT column_list
FROM table_name
FOR UPDATE;
```

---

## Example

```sql
SET SERVEROUTPUT ON;

DECLARE
    CURSOR emp_cur IS
        SELECT employee_id, salary
        FROM employee
        FOR UPDATE;

    v_id  employee.employee_id%TYPE;
    v_sal employee.salary%TYPE;

BEGIN
    OPEN emp_cur;

    LOOP
        FETCH emp_cur INTO v_id, v_sal;

        EXIT WHEN emp_cur%NOTFOUND;

        UPDATE employee
        SET salary = salary + 1000
        WHERE CURRENT OF emp_cur;
    END LOOP;

    CLOSE emp_cur;

    COMMIT;
END;
/
```

---

## Explanation

- The cursor selects employee records and locks them using `FOR UPDATE`.
- Each row is fetched one by one.
- `WHERE CURRENT OF emp_cur` updates the current row pointed to by the cursor.
- After all rows are updated, the transaction is committed.

---

## Important Points

- `FOR UPDATE` locks rows until `COMMIT` or `ROLLBACK`.
- Mostly used before `UPDATE` or `DELETE`.
- `WHERE CURRENT OF` updates or deletes the current row of the cursor without specifying a WHERE condition.

---

# 3. Cursor-Based Records

A **Cursor-Based Record** is a record whose structure is automatically derived from the **SELECT list** of a cursor.

Instead of declaring individual variables for every selected column, one record variable can store the entire fetched row.

It is created using the `%ROWTYPE` attribute of the cursor.

---

## Advantages

- No need to declare separate variables.
- Automatically matches the cursor's SELECT list.
- Easier to maintain.
- Makes programs shorter and cleaner.

---

## Syntax

```sql
CURSOR cursor_name IS
SELECT column1, column2
FROM table_name;

record_variable cursor_name%ROWTYPE;
```

---

## Example

```sql
SET SERVEROUTPUT ON;

DECLARE
    CURSOR cur_rec IS
        SELECT first_name, salary
        FROM employees
        WHERE employee_id = 100;

    var_emp cur_rec%ROWTYPE;

BEGIN
    OPEN cur_rec;

    FETCH cur_rec INTO var_emp;

    DBMS_OUTPUT.PUT_LINE(var_emp.first_name);
    DBMS_OUTPUT.PUT_LINE(var_emp.salary);

    CLOSE cur_rec;
END;
/
```

---

## Explanation

- The cursor selects `first_name` and `salary`.
- `var_emp` automatically contains the same fields as the cursor.
- One `FETCH` statement stores the entire row into the record.
- Individual fields are accessed using the dot (`.`) operator.

Examples:

```sql
var_emp.first_name
var_emp.salary
```

---

## Output (Example)

```
Steven
24000
```

*(Actual output depends on the data in the Employees table.)*

---

# Difference Between %ROWTYPE of Table and Cursor

| Feature | Table %ROWTYPE | Cursor %ROWTYPE |
|---------|----------------|-----------------|
| Based On | Entire table | Cursor SELECT list |
| Fields Included | All columns of the table | Only selected columns |
| Storage | Complete table row | Selected query result |
| Flexibility | Less flexible | More flexible |

---
