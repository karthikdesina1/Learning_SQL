# Mastering SQL Subqueries

## Overview

Subqueries are not just about writing a query inside another query.

The important skill is understanding **when to use each type** and how that choice affects readability, performance, and correctness.

A subquery generally follows this pattern:

```text id="k9v1xl"
Inner Query
   ↓
Intermediate Result
   ↓
Outer Query
   ↓
Final Result
```

---

# 1. Based on Dependency

## Correlated Subquery

A correlated subquery depends on the current row from the outer query.

```sql id="7ax2wp"
SELECT e.employee_name,
       e.salary
FROM employees e
WHERE e.salary > (
    SELECT AVG(e2.salary)
    FROM employees e2
    WHERE e2.department_id = e.department_id
);
```

### Logic

```text id="2w1gkl"
Take Outer Row
      ↓
Use Outer Value in Inner Query
      ↓
Calculate Result
      ↓
Compare
      ↓
Move to Next Row
```

### Typical Use Cases

- Employee vs department average
- Customer vs segment average
- Product vs category performance
- Row-specific comparison

### Performance Note

Correlated subqueries can be expensive because the inner logic may depend on many outer rows.

Modern optimizers can sometimes rewrite them, but they should still be tested on large datasets.

---

## Non-Correlated Subquery

The inner query is independent of the outer query.

```sql id="o7z7hc"
SELECT *
FROM employees
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
);
```

The inner query calculates one result independently.

### Conceptual Flow

```text id="m0t6cm"
Inner Query
→ Calculate Average

Outer Query
→ Compare Employees
```

### Typical Use Cases

- Above-average salary
- Maximum/minimum comparison
- Fixed threshold calculations
- Static lookup sets

---

# Correlated vs Non-Correlated

| Aspect | Correlated | Non-Correlated |
|---|---|---|
| Dependency | Depends on outer query | Independent |
| Execution Style | Row-dependent | Standalone |
| Complexity | Usually higher | Usually simpler |
| Performance | Can be slower | Often faster |
| Typical Use | Row vs group | Fixed result comparison |

---

# 2. Based on Location in Query

## Subquery in SELECT

Returns a calculated value.

```sql id="1g6f0u"
SELECT
    employee_name,
    salary,
    (SELECT AVG(salary)
     FROM employees) AS avg_salary
FROM employees;
```

Useful for:

- Scalar calculations
- Reference values
- Additional metrics

---

## Subquery in WHERE

Used for filtering.

```sql id="02zyah"
SELECT *
FROM employees
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
);
```

Useful for:

- Dynamic thresholds
- Comparison conditions
- Filtering based on another result

---

## Subquery in FROM

Acts like a temporary or derived table.

```sql id="p6ds9n"
SELECT department_id,
       avg_salary
FROM (
    SELECT department_id,
           AVG(salary) AS avg_salary
    FROM employees
    GROUP BY department_id
);
```

Useful when intermediate data must be created before the outer query.

---

## Subquery in HAVING

Filters aggregated groups.

```sql id="kxtp8q"
SELECT department_id,
       AVG(salary) AS avg_salary
FROM employees
GROUP BY department_id
HAVING AVG(salary) > (
    SELECT AVG(salary)
    FROM employees
);
```

This finds departments whose average salary is above the company average.

---

# 3. Based on Result Type

## Single-Row Subquery

Returns one value.

Use with:

```text id="aqvrhx"
=
>
<
>=
<=
<>
```

Example:

```sql id="cqfa3a"
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
)
```

---

## Multi-Row Subquery

Returns multiple values.

Use with:

```text id="8zm9u3"
IN
ANY
ALL
```

Example:

```sql id="9xpyzn"
SELECT *
FROM employees
WHERE department_id IN (
    SELECT department_id
    FROM departments
    WHERE location = 'New York'
);
```

---

# Important Rules

## Rule 1 — Match Operator to Result Type

Single-row result:

```text id="rw2pdc"
= > < >= <=
```

Multi-row result:

```text id="93hgji"
IN ANY ALL
```

---

## Rule 2 — Check NULL Behavior

`NOT IN` can behave unexpectedly when NULL exists in the subquery result.

In anti-match scenarios, `NOT EXISTS` may often be safer.

---

## Rule 3 — Keep Data Types Compatible

Avoid comparing incompatible values such as:

```text id="c441io"
NUMBER ↔ DATE
VARCHAR ↔ NUMBER
```

unless intentional conversion is used.

---

## Rule 4 — Avoid Unnecessary Nesting

Too many nested levels can make SQL:

- Hard to read
- Hard to debug
- Hard to maintain

Use a CTE when it improves clarity.

---

## Rule 5 — Consider Alternatives

Some subqueries can be rewritten using:

- JOINs
- CTEs
- Window Functions

Choose the approach that best balances:

```text id="fjj655"
Correctness
Readability
Performance
Maintainability
```

---

# Real-World Use Cases

## Employees Above Average Salary

```sql id="1ve09p"
SELECT *
FROM employees
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
);
```

---

## Department-Level Comparison

```sql id="nwcjvg"
SELECT e.*
FROM employees e
WHERE salary > (
    SELECT AVG(e2.salary)
    FROM employees e2
    WHERE e2.department_id = e.department_id
);
```

---

## High-Value Customers

```sql id="06ivbm"
SELECT customer_id,
       SUM(amount) AS total_spend
FROM orders
GROUP BY customer_id
HAVING SUM(amount) > (
    SELECT AVG(customer_total)
    FROM (
        SELECT SUM(amount) AS customer_total
        FROM orders
        GROUP BY customer_id
    )
);
```

---

# Key Concepts to Focus On

### Dependency

Ask:

```text id="9vz6v7"
Does the inner query need
the current outer row?
```

If yes → likely correlated.

---

### Result Type

Ask:

```text id="2jnh8r"
Will the inner query return
one value or multiple values?
```

This determines the operator.

---

### Location

Ask:

```text id="szb26w"
Am I calculating?
Filtering?
Creating a derived table?
Filtering groups?
```

That helps decide where the subquery belongs.

---

### Performance

Ask:

```text id="a7b7vg"
Can JOIN / CTE / Window Function
express this more clearly?
```

---

# Interview Quick Recap

### Correlated vs Non-Correlated?

```text id="23ltje"
Correlated
→ depends on outer query

Non-Correlated
→ independent
```

### Where can subqueries appear?

```text id="y0k4hy"
SELECT
WHERE
FROM
HAVING
```

### What operators work with multi-row subqueries?

```text id="51fhwd"
IN
ANY
ALL
```

### Why can correlated subqueries be slower?

Because their inner logic may depend on outer rows and require repeated evaluation.

### Are subqueries always the best solution?

No. JOINs, CTEs, and Window Functions can sometimes be clearer or more efficient.

---

# Final Takeaway

```text id="7z2iyj"
Business Problem
      ↓
Need Intermediate Result?
      ↓
Check Dependency
      ↓
Check Result Type
      ↓
Choose Subquery Location
      ↓
Compare Alternatives
      ↓
Write Clear SQL
```

Mastering subqueries means understanding **when and why to use them**, not just how to nest one query inside another.

## Topics Covered

`Correlated Subquery`  
`Non-Correlated Subquery`  
`SELECT Subquery`  
`WHERE Subquery`  
`FROM Subquery`  
`HAVING Subquery`  
`Single Row`  
`Multi Row`  
`IN`  
`ANY`  
`ALL`

### Tags

`SQL` `Subqueries` `Advanced SQL` `Data Analytics` `SQL Interview` `Problem Solving`
