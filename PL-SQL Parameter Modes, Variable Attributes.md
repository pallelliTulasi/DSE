
# Parameter Modes:-
dictate how values are passed in and out of the procedure.

* **IN MODE:-** passes a value into subprogram and acts like read-only constraint inside the block.
* **OUT MODE:-** returns a value back to calling program.
* **IN-OUT mode:-** passes an initial value into the subprogram & returns an updated value to the caller.

## Methods for passing parameters:-
When calling a procedure, actual parameters can be passed using 3 different notations:
1. **Positional Notation:** The 1st actual parameter is substituted for 1st formal parameter, second for 2nd.
2. **Named Notation:** The actual parameter is explicitly associated with its corresponding formal parameter using arrow symbol (`=>`).
3. **Mixed Notation:** combine both methods in a single call. However, any positional notation must precede the named notation for the call to be legal.

---

# Handling variables in PL/SQL:-
- Declared and (optionally) initialized in declarative section
- Passed as parameters to PL/SQL subprograms

-> **PL/SQL variables:** Scalar, Reference, LOB, composite
-> **Non-PL/SQL variables:** Bind variables.

## PL/SQL:-
* **Scalar data type:** holds single value.
* **Reference:** holds values, called pointers, which point to a storage location.
* **LOB:** hold values, called locators, which specify location of large objects that are stored outside the table.
* **Composite:** available by using PL/SQL collection and record variables.

**Non-PL/SQL:** variables holds language variables declared in precompiler programs, screen fields in forms applications and host variables. `SET AUTOPRINT`
* -> composite data type - PL/SQL records.
* -> PL/SQL collections - internal components that you can treat as individual variables.

-> A variable should not exceed 30 characters.

### Syntax:
`var_name [CONSTANT] datatype [NOT NULL] [:= | DEFAULT initial_val]`

### Example!
```sql
DECLARE
    a integer := 30;
    b integer := 40;
    c integer;
BEGIN
    c := a+b;
    dbms_output.put_line('value of c: ' || c);
    f := 100.0/3.0;
    dbms_output.put_line('value of f: ' || f);
END;
```

---

# Variable Attributes:-

1) `%TYPE` -> column.
2) `%ROWTYPE` -> row.

## `%TYPE`:
matches data type of single table column or another variable.

**Syntax:**
`<var_name> <tab_name>.<col_name>%TYPE;`

**Ex:**
```sql
DECLARE
    SAL EMP.Sal%TYPE;
    EMP_ID EMP.EID%TYPE;
BEGIN
    Emp_id := &Enter;
    Select salary into Sal from Emp
        where EID=Emp_ID;
    dbms_output.put_line('Salary of ' || ECODE || ' is= ' || Sal);
END;
```

## `%ROWTYPE`:-
matches the entire row of database table or a cursor result.

**Syntax:** `<var_name> <tab_name>%ROWTYPE;`

**Example!**
```sql
DECLARE
    EMPLOYEE EMP%ROWTYPE;
BEGIN
    EMPLOYEE.Empno := 2092;
    EMPLOYEE.Ename := 'Pandu';
    Insert into Emp where (EID, Ename) values
        (employee.empno, employee.ename);
    dbms_output.put_line('Row Inserted');
END;
```
