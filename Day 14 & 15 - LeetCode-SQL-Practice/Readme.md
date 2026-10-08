# LeetCode SQL Practice from Top SQL 50 Problems

## Overview

This section documents SQL practice from the **LeetCode Top SQL 50 Study Plan**.

The focus is not just solving queries, but learning how to recognize the right SQL pattern from the problem statement.

### Problems Practiced

1. Replace Employee ID With The Unique Identifier  
2. Product Sales Analysis I  
3. Customer Who Visited But Did Not Make Any Transactions  
4. Employee Bonus  
5. Managers With At Least 5 Direct Reports  
6. Students and Examinations  
7. Continuous Growth  
8. Triangle Judgement  
9. Employee vs Manager  
10. Display Table of Food Orders in a Restaurant  
11. Rank Scores  
12. Additional JOIN / aggregation practice  

---

## SQL Concepts Practiced

- LEFT JOIN
- INNER JOIN
- GROUP BY
- HAVING
- COUNT()
- NULL handling
- CASE WHEN
- Subqueries
- Window Functions
- Conditional logic
- Relationship analysis

---

# 1. LEFT JOIN Pattern

A common problem type is:

> Find records from one table even when no matching record exists in another table.

Typical logic:

```sql id="f3s5ja"
SELECT ...
FROM table_a a
LEFT JOIN table_b b
ON a.id = b.id
WHERE b.id IS NULL;
```

### Example Use Case

Find customers who visited but made no transaction.

### Pattern

```text id="5udtlo"
Keep all left rows
      ↓
Match right table
      ↓
Find NULL matches
      ↓
Return unmatched records
```

---

# 2. INNER JOIN Pattern

Use `INNER JOIN` when only matching records from both tables are needed.

```sql id="lwb18k"
SELECT ...
FROM products p
INNER JOIN sales s
ON p.product_id = s.product_id;
```

### Typical Questions

- Product + Sales
- Employee + Department
- Customer + Orders

### Logic

```text id="8swf7x"
Match exists in both tables
→ keep the row
```

---

# 3. GROUP BY + HAVING Pattern

Example question:

> Find managers with at least 5 direct reports.

Typical query structure:

```sql id="1r9n90"
SELECT manager_id,
       COUNT(*) AS direct_reports
FROM employees
GROUP BY manager_id
HAVING COUNT(*) >= 5;
```

### Logic

```text id="klywgt"
Group related rows
      ↓
Count each group
      ↓
Filter aggregated result
```

---

# 4. NULL Handling

NULL-related problems are common in SQL interviews.

Useful checks:

```sql id="n7ibjn"
IS NULL
IS NOT NULL
```

Example:

```sql id="fij6p6"
WHERE bonus IS NULL
```

Avoid:

```sql id="nqfytz"
bonus = NULL
```

because NULL must be checked using `IS NULL`.

---

# 5. CASE WHEN Pattern

Problems such as **Triangle Judgement** use conditional logic.

```sql id="4h8d2l"
SELECT
    CASE
        WHEN condition THEN 'Yes'
        ELSE 'No'
    END AS result
FROM table_name;
```

### Typical Uses

- Categorization
- Status logic
- Validation rules
- Conditional output

---

# 6. Subquery Pattern

Use a subquery when one calculation is needed before another comparison.

```sql id="69wffx"
SELECT *
FROM employees
WHERE salary >
(
    SELECT AVG(salary)
    FROM employees
);
```

### Logic

```text id="w8gh47"
Calculate result first
      ↓
Use result in outer query
```

---

# 7. Ranking Pattern

Problems such as **Rank Scores** require ranking logic.

Example:

```sql id="tyc1jo"
SELECT
    score,
    DENSE_RANK() OVER (
        ORDER BY score DESC
    ) AS rank_position
FROM scores;
```

### Why DENSE_RANK?

It assigns the same rank to equal values without leaving gaps.

Example:

```text id="qwy1qj"
100 → 1
100 → 1
90  → 2
80  → 3
```

---

# 8. Problem Statement Clues

One of the most useful habits is identifying keywords in the requirement.

| Problem Clue | SQL Pattern |
|---|---|
| "Even if no match" | LEFT JOIN |
| "Only matching records" | INNER JOIN |
| "At least N" | GROUP BY + HAVING |
| "Unique" | DISTINCT |
| "If / Otherwise" | CASE WHEN |
| "No transaction / Missing match" | LEFT JOIN + IS NULL |
| "Rank" | Window Function |
| "Compare with calculated value" | Subquery / CTE |

---

# 9. Example Problem-Solving Flow

Instead of immediately writing SQL:

```text id="8jyu19"
Read Problem
    ↓
Identify Tables
    ↓
Find Relationship
    ↓
Identify Output Columns
    ↓
Recognize SQL Pattern
    ↓
Write Query
    ↓
Check NULLs / Duplicates
    ↓
Validate Result
```

This approach reduces trial-and-error.

---

# 10. Interview-Oriented Lessons

### JOIN Question

Ask:

```text id="0jr2hr"
Do I need only matches?
→ INNER JOIN

Do I need all rows from one side?
→ LEFT / RIGHT JOIN
```

### Aggregation Question

Ask:

```text id="f37y6v"
Do I need totals or counts?
→ GROUP BY

Do I need to filter those totals?
→ HAVING
```

### NULL Question

Ask:

```text id="ht0kt4"
Does missing data have meaning?
→ IS NULL / IS NOT NULL
```

### Ranking Question

Ask:

```text id="9aw20i"
Do tied values share ranks?
→ RANK / DENSE_RANK
```

---

# Key Takeaway

SQL problem solving is less about memorizing syntax and more about recognizing patterns.

```text id="yqdd9a"
Business Question
      ↓
SQL Pattern
      ↓
Query Logic
      ↓
Correct Result
      ↓
Insight
```

The goal is to develop the ability to look at a problem and quickly decide:

- Which JOIN is needed?
- Is grouping required?
- Should HAVING be used?
- Are NULL values important?
- Is conditional logic needed?
- Would a subquery or window function make sense?

That pattern-recognition skill is what makes SQL practice valuable for interviews and real-world analytics.

---

## Topics Practiced

`LEFT JOIN`  
`INNER JOIN`  
`GROUP BY`  
`HAVING`  
`COUNT()`  
`CASE WHEN`  
`NULL`  
`Subqueries`  
`Window Functions`  
`Ranking`

### Tags

`SQL` `LeetCode` `SQL Interview` `Data Analytics` `Problem Solving` `SQL Practice`
