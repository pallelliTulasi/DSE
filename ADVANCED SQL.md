# Advanced SQL - Subqueries

## 📌 What is a Query?

A **Query** is a collection of SQL keywords and expressions used to retrieve, insert, update, or delete data from a database.

Example:

```sql
SELECT first_name, salary
FROM employees;
```

---

# What is a Subquery?

A **Subquery** is a query written inside another SQL query.

- Also called an **Inner Query**.
- The main query is called the **Outer Query**.
- The result of the inner query is used as input for the outer query.

### Syntax

```sql
SELECT column_name
FROM table1          -- Outer Query
WHERE column_name = (
    SELECT column_name
    FROM table2      -- Inner Query
    WHERE condition
);
```

---

# How Subqueries Work

The database executes the queries in this order:

1. Execute the **Inner Query**
2. Store its result
3. Execute the **Outer Query** using that result

Example:

Find employees who earn more than the average salary.

### Step 1

Find the average salary.

```sql
SELECT AVG(salary)
FROM employees;
```

Suppose the result is:

```
65000
```

### Step 2

Use that value in another query.

```sql
SELECT first_name, salary
FROM employees
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
);
```

---

# Where Can We Use Subqueries?

Subqueries can be placed in:

- WHERE clause
- HAVING clause
- FROM clause

---

# Types of Subqueries

There are two types:

1. Single-row Subqueries
2. Multiple-row Subqueries

---

# 1. Single-row Subqueries

A **Single-row Subquery** returns only **one row**.

These use **single-row comparison operators**.

Operators:

- =
- >
- <
- >=
- <=
- <>

Example:

```sql
SELECT dname, first_name, last_name, employee_id
FROM employees
WHERE salary >
(
    SELECT AVG(salary)
    FROM employees
);
```

The subquery returns one value (average salary).

The outer query compares every employee salary with that value.

---

# Single-row Subquery Example

Find employees whose job is the same as employee 141 and whose salary is greater than employee 143.

```sql
SELECT last_name, job_id, salary
FROM employees
WHERE job_id =
(
    SELECT job_id
    FROM employees
    WHERE employee_id = 141
)
AND salary >
(
    SELECT salary
    FROM employees
    WHERE employee_id = 143
);
```

---

# Using Group Functions in Subqueries

Group functions such as:

- AVG()
- MIN()
- MAX()
- COUNT()
- SUM()

can be used inside subqueries.

Example:

Find employees earning the minimum salary.

```sql
SELECT last_name, job_id, salary
FROM employees
WHERE salary =
(
    SELECT MIN(salary)
    FROM employees
);
```

---

# HAVING Clause with Subqueries

The database first executes the subquery and then evaluates the HAVING clause.

Example:

```sql
SELECT department_id,
       MIN(salary)
FROM employees
GROUP BY department_id
HAVING MIN(salary) >
(
    SELECT MIN(salary)
    FROM employees
    WHERE department_id = 50
);
```

---

# 2. Multiple-row Subqueries

A **Multiple-row Subquery** returns **more than one row**.

Single-row operators (`=`, `>`, `<`) cannot handle multiple values.

Instead, use:

- IN
- ANY
- ALL
- EXISTS

---

## IN Operator

The most commonly used multi-row operator.

Example:

```sql
SELECT employee_id,
       first_name,
       department_id
FROM employees
WHERE department_id IN
(
    SELECT department_id
    FROM departments
    WHERE department_name IN ('Sales','Finance')
);
```

---

## ANY Operator

Compares a value with **each value** returned by the subquery.

### Rules

- `> ANY` → Greater than the smallest value
- `< ANY` → Less than the largest value

Example:

```sql
SELECT employee_id,
       salary
FROM employees
WHERE salary > ANY
(
    SELECT salary
    FROM employees
    WHERE department_id = 30
);
```

---

## ALL Operator

Compares a value with **every value** returned by the subquery.

### Rules

- `> ALL` → Greater than the maximum value
- `< ALL` → Less than the minimum value

Example:

```sql
SELECT employee_id,
       salary
FROM employees
WHERE salary > ALL
(
    SELECT salary
    FROM employees
    WHERE department_id = 30
);
```

---

## EXISTS Operator

Checks whether the subquery returns **at least one row**.

If rows exist, the condition becomes TRUE.

Example:

```sql
SELECT *
FROM orders
WHERE EXISTS
(
    SELECT *
    FROM order_items
    WHERE order_items.order_id = orders.order_id
);
```

---

## NOT EXISTS

Returns rows only if the subquery returns **no rows**.

Example:

```sql
SELECT *
FROM orders
WHERE NOT EXISTS
(
    SELECT *
    FROM order_items
    WHERE order_items.order_id = orders.order_id
);
```

---

# Comparison Operators Used with Subqueries

## Single-row Operators

Used when the subquery returns exactly one row.

- =
- >
- <
- >=
- <=
- <>

---

## Multiple-row Operators

Used when the subquery returns multiple rows.

- IN
- ANY
- ALL
- EXISTS

---

# Errors in Subqueries

## ORA-01427

### Error

```
ORA-01427:
single-row subquery returns more than one row
```

### Reason

Using a single-row operator like `=` while the subquery returns multiple rows.

Example:

```sql
SELECT *
FROM employees
WHERE department_id =
(
    SELECT department_id
    FROM departments
);
```

The subquery returns multiple department IDs.

### Solution

Use `IN`.

```sql
SELECT *
FROM employees
WHERE department_id IN
(
    SELECT department_id
    FROM departments
);
```

---

# No Rows Returned

Sometimes the subquery returns **no rows**.

Example:

```sql
SELECT *
FROM employees
WHERE manager_id =
(
    SELECT employee_id
    FROM employees
    WHERE last_name = 'XYZ'
);
```

If employee **XYZ** does not exist, the inner query returns nothing.

The outer query also returns no rows.

---

# Null Values in Subqueries

NULL values can cause unexpected results.

---

## IN Operator with NULL

`IN` checks whether a value exists in the list returned by the subquery.

It is equivalent to:

```sql
= ANY
```

If **at least one comparison is TRUE**, the row is returned.

Example:

```sql
SELECT employee_id
FROM employees
WHERE department_id IN
(
    SELECT department_id
    FROM departments
);
```

---

## NOT IN with NULL (Important)

`NOT IN` is equivalent to:

```sql
<> ALL
```

If the subquery returns **even one NULL**, the comparison becomes **UNKNOWN**.

As a result, **no rows are returned**.

Example:

```sql
SELECT employee_id
FROM employees
WHERE manager_id NOT IN
(
    SELECT manager_id
    FROM employees
);
```

If one manager_id is NULL:

```
1
2
3
NULL
```

The condition becomes:

```
manager_id <> 1
AND manager_id <> 2
AND manager_id <> 3
AND manager_id <> NULL
```

Since comparison with NULL is UNKNOWN, the entire condition becomes UNKNOWN.

Therefore, **no rows are returned**.

---

# Correct Way

Always remove NULL values before using `NOT IN`.

```sql
SELECT employee_id
FROM employees
WHERE manager_id NOT IN
(
    SELECT manager_id
    FROM employees
    WHERE manager_id IS NOT NULL
);
```

---

# Better Alternative

Instead of `NOT IN`, use `NOT EXISTS`.

Example:

```sql
SELECT *
FROM orders o
WHERE NOT EXISTS
(
    SELECT *
    FROM order_items oi
    WHERE oi.order_id = o.order_id
);
```

`NOT EXISTS` works correctly even when NULL values are present.

---

# Guidelines for Using Subqueries

- Always enclose subqueries in parentheses.
- Place the subquery on the right side of the comparison operator.
- The inner query executes before the outer query.
- Use single-row operators for single-row subqueries.
- Use multi-row operators for multi-row subqueries.
- Avoid using `NOT IN` when the subquery may return NULL values.
- Filter NULL values using `IS NOT NULL` or use `NOT EXISTS`.
- `ORDER BY` is generally unnecessary inside a subquery unless performing Top-N analysis.

---

# Key Takeaways

- A subquery is a query inside another SQL query.
- The inner query always executes before the outer query.
- Use single-row operators for one returned value.
- Use multi-row operators when multiple values are returned.
- `IN` is the safest and most commonly used multi-row operator.
- `NOT IN` should not be used if the subquery may return NULL values.
- Prefer `NOT EXISTS` when working with possible NULL values.
- Group functions such as `AVG()`, `MIN()`, and `MAX()` are commonly used in subqueries.
