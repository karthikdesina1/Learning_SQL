# SQL Subqueries - Basics to Real-World Use Cases

## Overview

A **Subquery** is a query written inside another SQL query.

It is useful when a problem needs to be solved in multiple logical steps.

Typical pattern:

```text
Inner Query
   ↓
Intermediate Result
   ↓
Outer Query
   ↓
Final Result
```

Subqueries are commonly used for:

- Dynamic filtering
- Comparisons
- Group-level analysis
- Existence checks
- Business rule logic

---

# 1. Single-Row Subquery

A single-row subquery returns one value.

Example:

```sql
SELECT AVG(salary)
FROM employees;
```

Suppose the result is:

```text
75000
```

The outer query can use that value:

```sql
SELECT *
FROM employees
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
);
```

### Use Case

Find employees earning above the overall average salary.

### Common Operators

```text
=
>
<
>=
<=
<>
```

---

# 2. Multiple-Row Subquery

A multiple-row subquery returns more than one value.

Example:

```sql
SELECT department_id
FROM departments
WHERE location = 'New York';
```

The result may be:

```text
10
20
30
```

Use `IN`:

```sql
SELECT *
FROM employees
WHERE department_id IN (
    SELECT department_id
    FROM departments
    WHERE location = 'New York'
);
```

### Use Case

Find employees working in departments located in New York.

---

# 3. IN

`IN` checks whether a value matches any result returned by the subquery.

```sql
SELECT *
FROM products
WHERE category_id IN (
    SELECT category_id
    FROM categories
    WHERE status = 'Active'
);
```

### Simple Meaning

```text
Value exists in returned list?
→ Keep the row
```

---

# 4. EXISTS

`EXISTS` checks whether the subquery returns at least one row.

```sql
SELECT *
FROM customers c
WHERE EXISTS (
    SELECT 1
    FROM orders o
    WHERE o.customer_id = c.customer_id
);
```

### Use Case

Find customers who have placed at least one order.

### Key Idea

`EXISTS` cares about:

```text
Does a matching row exist?
```

not the actual returned value.

---

# 5. ANY

`ANY` compares a value with at least one value returned by a subquery.

```sql
SELECT *
FROM employees
WHERE salary > ANY (
    SELECT salary
    FROM employees
    WHERE department_id = 10
);
```

This means:

```text
Salary is greater than at least one salary
from department 10.
```

---

# 6. ALL

`ALL` requires the condition to be true for every value returned.

```sql
SELECT *
FROM employees
WHERE salary > ALL (
    SELECT salary
    FROM employees
    WHERE department_id = 10
);
```

This means:

```text
Salary is greater than every salary
in department 10.
```

### Easy Comparison

```text
ANY → at least one
ALL → every value
```

---

# 7. Correlated Subquery

A correlated subquery depends on the current row from the outer query.

Example:

```sql
SELECT e1.employee_id,
       e1.employee_name,
       e1.department_id,
       e1.salary
FROM employees e1
WHERE e1.salary > (
    SELECT AVG(e2.salary)
    FROM employees e2
    WHERE e2.department_id = e1.department_id
);
```

### What Happens?

For each employee:

```text
Identify employee department
        ↓
Calculate that department's average salary
        ↓
Compare employee salary
        ↓
Return employee if salary is above average
```

### Use Case

Find employees earning above the average salary of their own department.

---

# 8. Real-World Example — High-Value Customers

Business question:

> Find customers whose total spending is above the average customer spending.

```sql
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

This breaks the problem into:

```text
Customer Totals
      ↓
Average Customer Total
      ↓
Compare Each Customer
      ↓
High-Value Customers
```

---

# Subquery Types Recap

| Type | Returns | Typical Use |
|---|---|---|
| Single Row | One value | Average / max / min comparison |
| Multiple Row | Multiple values | IN / ANY / ALL |
| Correlated | Depends on outer row | Row vs group comparison |

---

# IN vs EXISTS

## IN

Useful when comparing against a returned list.

```sql
WHERE department_id IN (...)
```

## EXISTS

Useful when checking whether a matching relationship exists.

```sql
WHERE EXISTS (...)
```

### Easy Reminder

```text
IN
→ Is this value in the result?

EXISTS
→ Does a matching row exist?
```

---

# Must-Remember Notes

### 1. Match the operator to the result type

Single-row result:

```text
=
>
<
```

Multiple-row result:

```text
IN
ANY
ALL
```

### 2. Correlated subqueries can run repeatedly

They may be more expensive on large datasets because the inner query can depend on each outer row.

### 3. NULL values matter

`NOT IN` can behave unexpectedly when the subquery contains NULL values.

In many cases, `NOT EXISTS` can be safer for anti-match logic.

### 4. Subqueries are not always the only solution

Some problems can also be solved using:

- JOINs
- CTEs
- Window Functions

Choose the version that is clear, correct, and efficient.

---

# Interview Quick Recap

### What is a subquery?

A query nested inside another query.

### What is a single-row subquery?

A subquery that returns one value.

### What is a multiple-row subquery?

A subquery that returns multiple rows or values.

### What is a correlated subquery?

A subquery that references columns from the outer query.

### IN vs EXISTS?

```text
IN
→ compare against values

EXISTS
→ check whether rows exist
```

### ANY vs ALL?

```text
ANY
→ condition true for at least one value

ALL
→ condition true for every value
```

---

# Key Takeaway

```text
Complex Business Question
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

Subqueries are powerful because they help make complex SQL problems easier to reason about by solving them in smaller logical stages.

## Topics Covered

`Subqueries`  
`Single Row Subquery`  
`Multiple Row Subquery`  
`Correlated Subquery`  
`IN`  
`EXISTS`  
`ANY`  
`ALL`

### Tags

`SQL` `Subqueries` `Data Analytics` `SQL Interview` `Database` `Business Analytics`
