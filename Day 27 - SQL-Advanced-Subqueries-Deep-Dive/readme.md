# Advanced SQL Subqueries - Deep Dive

## Overview

Advanced Subqueries help solve complex SQL problems by breaking them into smaller logical steps.

A subquery produces an intermediate result that can be used by another query.

Basic flow:

```text
Inner Query
   ↓
Intermediate Result
   ↓
Outer Query
   ↓
Final Result
```

---

## 1. Subqueries in SELECT

Subqueries can return calculated values inside the `SELECT` list.

```sql
SELECT
    employee_name,
    salary,
    (SELECT AVG(salary)
     FROM employees) AS company_avg
FROM employees;
```

This keeps every employee row while adding the overall average salary.

---

## 2. Subqueries in WHERE

A common use case is filtering based on another calculation.

```sql
SELECT *
FROM employees
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
);
```

### Logic

```text
Calculate average salary
        ↓
Return intermediate value
        ↓
Compare each employee
        ↓
Keep above-average employees
```

---

# Types of Subqueries

## 3. Single-Row Subquery

Returns one value.

Example:

```sql
SELECT *
FROM employees
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
);
```

Common operators:

```text
=
>
<
>=
<=
<>
```

### Use Cases

- Above-average values
- Maximum or minimum comparisons
- Threshold analysis

---

## 4. Multi-Row Subquery

Returns multiple values.

```sql
SELECT *
FROM employees
WHERE department_id IN (
    SELECT department_id
    FROM departments
    WHERE location = 'New York'
);
```

Common operators:

```text
IN
ANY
ALL
```

---

## 5. IN

Checks whether a value matches one of the returned values.

```sql
WHERE department_id IN (
    SELECT department_id
    FROM departments
);
```

Meaning:

```text
Is this value present
in the subquery result?
```

---

## 6. ANY

Condition must be true for at least one returned value.

```sql
SELECT *
FROM employees
WHERE salary > ANY (
    SELECT salary
    FROM employees
    WHERE department_id = 10
);
```

Meaning:

```text
Greater than at least one value
```

---

## 7. ALL

Condition must be true for every returned value.

```sql
SELECT *
FROM employees
WHERE salary > ALL (
    SELECT salary
    FROM employees
    WHERE department_id = 10
);
```

Meaning:

```text
Greater than every returned value
```

---

# 8. Correlated Subquery

A correlated subquery depends on values from the outer query.

```sql
SELECT e1.employee_name,
       e1.department_id,
       e1.salary
FROM employees e1
WHERE e1.salary > (
    SELECT AVG(e2.salary)
    FROM employees e2
    WHERE e2.department_id = e1.department_id
);
```

### Logic

For each employee:

```text
Identify department
      ↓
Calculate department average
      ↓
Compare employee salary
      ↓
Return qualifying row
```

### Use Case

Find employees earning above the average salary of their own department.

---

# 9. Nested Subqueries

Subqueries can exist at multiple levels.

```text
Outer Query
    ↓
Middle Query
    ↓
Inner Query
```

Example pattern:

```sql
SELECT *
FROM employees
WHERE department_id IN (
    SELECT department_id
    FROM departments
    WHERE manager_id IN (
        SELECT manager_id
        FROM managers
        WHERE status = 'Active'
    )
);
```

Deep nesting can work, but readability should always be considered.

---

# 10. Data Type Compatibility

The compared values should have compatible data types.

Example:

```sql
employee.department_id
```

should normally be compared with another compatible `department_id` value.

Poor type matching can cause errors or implicit conversions.

---

# 11. Correlated Subquery Performance

Correlated subqueries can be powerful, but they may be expensive because the inner query can depend on each outer row.

Conceptually:

```text
Outer Row 1 → Run Inner Logic
Outer Row 2 → Run Inner Logic
Outer Row 3 → Run Inner Logic
...
```

Modern optimizers may rewrite queries internally, but performance should still be tested on large datasets.

---

# 12. Subquery vs JOIN

Sometimes a JOIN is a better alternative.

Subquery:

```sql
SELECT *
FROM customers
WHERE customer_id IN (
    SELECT customer_id
    FROM orders
);
```

JOIN:

```sql
SELECT DISTINCT c.*
FROM customers c
JOIN orders o
ON c.customer_id = o.customer_id;
```

Choose based on:

- Readability
- Data volume
- Business requirement
- Execution plan
- Maintainability

---

# 13. Subquery vs CTE

A CTE can improve readability for complex logic.

```sql
WITH dept_avg AS (
    SELECT department_id,
           AVG(salary) AS avg_salary
    FROM employees
    GROUP BY department_id
)
SELECT e.*
FROM employees e
JOIN dept_avg d
ON e.department_id = d.department_id
WHERE e.salary > d.avg_salary;
```

This can be easier to debug and extend.

---

# Real-World Applications

Subqueries are useful for:

- Employees above average salary
- High-value customers
- Top-performing products
- Dynamic thresholds
- Matching against another result set
- Group-level comparisons
- Existence checks

---

# Must-Remember Notes

1. Subqueries conceptually work from inner logic to outer logic.
2. Match operators with result type.
3. Single-row subqueries must return one value where scalar comparison is expected.
4. Use `IN`, `ANY`, or `ALL` for multiple values.
5. Correlated subqueries reference the outer query.
6. Deep nesting can reduce readability.
7. JOINs, CTEs, or Window Functions may sometimes be better alternatives.
8. Always test performance on real data volumes.

---

# Interview Quick Recap

### What is a subquery?

A query nested inside another query.

### Single-row vs multi-row?

```text
Single-row → one value
Multi-row  → multiple values
```

### What is a correlated subquery?

A subquery that depends on the current row of the outer query.

### ANY vs ALL?

```text
ANY → at least one value
ALL → every value
```

### Why can correlated subqueries be expensive?

Because the inner logic may be evaluated repeatedly for outer rows.

### Are subqueries always better than JOINs?

No. The better approach depends on the problem, readability, and performance.

---

# Key Takeaway

```text
Complex Problem
      ↓
Break Into Steps
      ↓
Inner Query
      ↓
Intermediate Result
      ↓
Outer Query
      ↓
Final Insight
```

Advanced Subqueries are most useful when they make the SQL logic easier to understand, maintain, and analyze.

## Topics Covered

`Subqueries`  
`Single Row Subquery`  
`Multi Row Subquery`  
`Correlated Subquery`  
`IN`  
`ANY`  
`ALL`  
`Nested Queries`  
`JOIN Alternatives`

### Tags

`SQL` `Advanced SQL` `Subqueries` `Data Analytics` `SQL Interview` `Business Analytics`
