# SQL Window Functions - Deep Dive

## Overview

Window Functions allow SQL to perform calculations across related rows **without removing row-level detail**.

They are especially useful for:

- Ranking
- Top-N analysis
- Performance comparison
- Running calculations
- Group-level analysis

---

## Window Function Syntax

```sql
SELECT
    column_name,
    window_function(expression)
    OVER (
        PARTITION BY column_name
        ORDER BY column_name
    )
FROM table_name;
```

### Main Components

```text
OVER()
→ defines the window

PARTITION BY
→ divides rows into groups

ORDER BY
→ defines sequence inside each group
```

---

# GROUP BY vs PARTITION BY

## GROUP BY

Aggregates rows and reduces the result.

```sql
SELECT
    department,
    AVG(salary) AS avg_salary
FROM employees
GROUP BY department;
```

Example result:

```text
Sales       75000
Marketing   62500
Engineering 90000
```

One row is returned per department.

---

## PARTITION BY

Keeps every row while calculating within groups.

```sql
SELECT
    employee_name,
    department,
    salary,
    AVG(salary) OVER (
        PARTITION BY department
    ) AS dept_avg_salary
FROM employees;
```

Example idea:

```text
Alex   Sales      90000   85000
Beth   Sales      80000   85000
Dana   Marketing  70000   67500
Evan   Marketing  65000   67500
```

### Easy Rule

```text
GROUP BY
→ fewer rows

PARTITION BY
→ same rows + extra analytical value
```

---

# 1. ROW_NUMBER()

Assigns a unique sequential number to every row.

```sql
SELECT
    employee_name,
    department,
    salary,
    ROW_NUMBER() OVER (
        PARTITION BY department
        ORDER BY salary DESC
    ) AS row_num
FROM employees;
```

Example:

```text
90000 → 1
80000 → 2
80000 → 3
70000 → 4
```

Even ties receive different row numbers.

### Use Case

Select exactly one top row per group.

---

# 2. RANK()

Tied values share the same rank, and the next rank is skipped.

```sql
SELECT
    employee_name,
    department,
    salary,
    RANK() OVER (
        PARTITION BY department
        ORDER BY salary DESC
    ) AS salary_rank
FROM employees;
```

Example:

```text
95000 → 1
90000 → 2
90000 → 2
85000 → 4
```

### Use Case

Leaderboards where ties should share position.

---

# 3. DENSE_RANK()

Tied values share the same rank, but no ranks are skipped.

```sql
SELECT
    employee_name,
    department,
    salary,
    DENSE_RANK() OVER (
        PARTITION BY department
        ORDER BY salary DESC
    ) AS salary_rank
FROM employees;
```

Example:

```text
95000 → 1
90000 → 2
90000 → 2
85000 → 3
```

---

# Ranking Comparison

| Function | Ties | Gaps | Unique per Row |
|---|---|---|---|
| `ROW_NUMBER()` | No shared rank | No | Yes |
| `RANK()` | Same rank | Yes | No |
| `DENSE_RANK()` | Same rank | No | No |

---

# Ranking Within Groups

Suppose employees belong to different departments.

```sql
SELECT
    employee_name,
    department,
    salary,
    RANK() OVER (
        PARTITION BY department
        ORDER BY salary DESC
    ) AS dept_rank
FROM employees;
```

`PARTITION BY department` makes ranking restart for every department.

Conceptually:

```text
Sales
1
2
3

Marketing
1
2

Engineering
1
2
```

---

# Top-N Within Each Group

A common interview and analytics problem:

> Find the Top 3 employees in each department.

```sql
SELECT *
FROM (
    SELECT
        employee_name,
        department,
        salary,
        ROW_NUMBER() OVER (
            PARTITION BY department
            ORDER BY salary DESC
        ) AS rn
    FROM employees
)
WHERE rn <= 3;
```

This is one of the most common practical uses of Window Functions.

---

# Performance Comparison Within Groups

Compare each employee with the department average:

```sql
SELECT
    employee_name,
    department,
    salary,
    AVG(salary) OVER (
        PARTITION BY department
    ) AS dept_avg
FROM employees;
```

This answers:

```text
Employee salary
vs
Department average
```

without losing the employee row.

---

# Real-World Applications

## Leaderboards

Rank:

- Employees
- Salespeople
- Products
- Stores

## Top-N Analysis

```text
Top 3 products by category
Top 5 customers by region
Best salesperson per territory
```

## Performance Comparison

Compare:

- Employee vs department
- Store vs region
- Product vs category
- Customer vs segment

---

# Must-Remember Notes

### 1. PARTITION BY is optional

Without it:

```sql
RANK() OVER (
    ORDER BY salary DESC
)
```

the entire result becomes one partition.

### 2. ORDER BY controls ranking

For ranking functions, ordering determines the position.

### 3. ROW_NUMBER needs deterministic ordering

If values tie and consistent results matter, add another sort column.

```sql
ORDER BY salary DESC, employee_id
```

### 4. Window Functions do not behave like GROUP BY

They calculate across rows but preserve those rows.

### 5. Window Functions are evaluated after grouping logic

They are commonly used after `WHERE`, `GROUP BY`, and `HAVING` in SQL's logical processing flow.

---

# Interview Quick Recap

### GROUP BY vs PARTITION BY?

```text
GROUP BY
→ aggregates and reduces rows

PARTITION BY
→ creates analytical groups without reducing rows
```

### RANK vs DENSE_RANK?

```text
RANK
→ gaps after ties

DENSE_RANK
→ no gaps
```

### ROW_NUMBER vs RANK?

```text
ROW_NUMBER
→ every row unique

RANK
→ tied values can share rank
```

### How do you rank within each department?

```sql
RANK() OVER (
    PARTITION BY department
    ORDER BY salary DESC
)
```

### How do you find Top 3 per group?

Use a ranking function inside a subquery or CTE, then filter the rank.

---

# Key Takeaway

```text
Detailed Rows
     ↓
PARTITION BY
     ↓
ORDER BY
     ↓
Window Function
     ↓
Ranking / Comparison
     ↓
Advanced Analytical Insight
```

Window Functions are powerful because they allow analysts to compare rows **within context while preserving detail**.

## Topics Covered

`Window Functions`  
`OVER()`  
`PARTITION BY`  
`GROUP BY`  
`ROW_NUMBER()`  
`RANK()`  
`DENSE_RANK()`  
`Top-N Analysis`

### Tags

`SQL` `Window Functions` `Advanced SQL` `Data Analytics` `SQL Interview` `Business Analytics`
