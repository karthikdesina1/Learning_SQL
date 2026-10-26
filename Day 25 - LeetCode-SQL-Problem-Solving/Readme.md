# LeetCode SQL Problem Solving

## Overview

This section focuses on applying SQL concepts through **LeetCode SQL problems**.

The objective is not only to get the correct answer, but to improve the ability to:

- Understand the problem statement
- Identify the underlying SQL pattern
- Choose the right SQL function or technique
- Handle edge cases
- Write readable and efficient queries
- Build interview-level problem-solving skills

---

# Problems Solved

## 1. Department Top Three Salaries — LeetCode 185

### Problem

Find employees who earn one of the top three unique salaries within each department.

### Key Concept

`DENSE_RANK()`

The important requirement is:

```text
Top 3 salaries
within each department
```

This immediately suggests:

```text
PARTITION BY department
+
ORDER BY salary DESC
+
DENSE_RANK()
```

### Query Pattern

```sql
SELECT departmentId,
       name,
       salary
FROM (
    SELECT
        departmentId,
        name,
        salary,
        DENSE_RANK() OVER (
            PARTITION BY departmentId
            ORDER BY salary DESC
        ) AS salary_rank
    FROM Employee
)
WHERE salary_rank <= 3;
```

### Why DENSE_RANK()?

If two employees have the same salary:

```text
90000 → Rank 1
85000 → Rank 2
85000 → Rank 2
80000 → Rank 3
```

There are no gaps in ranking.

### Pattern Recognition

```text
"Top N within each group"
        ↓
Window Function
        ↓
PARTITION BY + DENSE_RANK()
```

---

# 2. Queries Quality and Percentage — LeetCode 1211

### Problem

Calculate:

- Query quality
- Poor query percentage

for each `query_name`.

### Key Concepts

- `GROUP BY`
- `AVG()`
- `ROUND()`
- Boolean / conditional aggregation
- Percentage calculations

### Example Query Pattern

```sql
SELECT
    query_name,
    ROUND(
        AVG(rating / position),
        2
    ) AS quality,

    ROUND(
        AVG(rating < 3) * 100,
        2
    ) AS poor_query_percentage

FROM Queries
GROUP BY query_name;
```

### Analytical Logic

```text
Group by query
      ↓
Calculate average quality
      ↓
Identify poor queries
      ↓
Calculate percentage
```

This problem is a good example of translating a business-style metric into SQL.

### Pattern Recognition

```text
"Calculate metrics per category"
        ↓
GROUP BY
        ↓
Aggregate Functions
```

---

# 3. Consecutive Numbers — LeetCode 180

### Problem

Find numbers that appear at least three times consecutively.

### Key Concepts

- `LAG()`
- `LEAD()`
- Window Functions
- Self Join
- Sequential row comparison

### Window Function Approach

```sql
SELECT DISTINCT num AS ConsecutiveNums
FROM (
    SELECT
        num,
        LAG(num, 1) OVER (
            ORDER BY id
        ) AS prev1,

        LAG(num, 2) OVER (
            ORDER BY id
        ) AS prev2

    FROM Logs
)
WHERE num = prev1
AND num = prev2;
```

### Logic

For every row:

```text
Current Value
      =
Previous Row
      =
Two Rows Back
```

If all three are equal, the value occurs three times consecutively.

---

## Alternative: Self Join

The same problem can also be solved by joining the table with itself.

```sql
SELECT DISTINCT l1.num AS ConsecutiveNums
FROM Logs l1
JOIN Logs l2
    ON l2.id = l1.id + 1
JOIN Logs l3
    ON l3.id = l1.id + 2
WHERE l1.num = l2.num
AND l2.num = l3.num;
```

Comparing different solutions helps improve SQL reasoning and query design.

---

# SQL Pattern Recognition

A major takeaway from these problems is learning to identify clues in the question.

| Problem Requirement | SQL Pattern |
|---|---|
| Top N per group | Window Function |
| Ranking with ties | DENSE_RANK() |
| Metrics by category | GROUP BY |
| Average calculation | AVG() |
| Percentage calculation | Aggregation |
| Compare neighboring rows | LEAD() / LAG() |
| Compare rows in same table | Self Join |

---

# Problem-Solving Approach

Before writing SQL:

```text
1. Read the requirement carefully
        ↓
2. Identify input tables
        ↓
3. Understand relationships
        ↓
4. Identify expected output
        ↓
5. Look for clues in the wording
        ↓
6. Choose the SQL pattern
        ↓
7. Write the query
        ↓
8. Test edge cases
        ↓
9. Optimize if needed
```

---

# What These Problems Improve

## Analytical Thinking

Instead of immediately writing syntax, understand:

```text
What is the question actually asking?
```

## Function Selection

Examples:

```text
"Top 3 per department"
→ DENSE_RANK()

"Previous values"
→ LAG()

"Group-level metrics"
→ GROUP BY
```

## Query Optimization

Compare alternative solutions and ask:

- Can a Window Function simplify this?
- Is a Self Join necessary?
- Am I scanning unnecessary rows?
- Can the logic be made easier to read?

---

# Interview Quick Recap

### Top N within each department?

Use:

```sql
DENSE_RANK() OVER (
    PARTITION BY department
    ORDER BY salary DESC
)
```

### Why DENSE_RANK instead of ROW_NUMBER?

`DENSE_RANK()` preserves ties.

`ROW_NUMBER()` gives every row a unique number.

### How can consecutive values be detected?

Using:

```text
LAG()
LEAD()
Self Join
```

### When is GROUP BY useful?

When rows must be summarized into category-level results.

### What should come before syntax?

Understanding the required logic.

---

# Key Takeaway

SQL problem solving is not about remembering every function.

It is about recognizing what the question is asking and selecting the right SQL tool.

```text
Problem Statement
       ↓
Observation
       ↓
Pattern Recognition
       ↓
Choose Function / Technique
       ↓
Build Query
       ↓
Validate
       ↓
Optimize
```

The stronger this pattern-recognition skill becomes, the easier it is to approach both real-world analytics problems and SQL interviews.

---
## Topics Practiced

`DENSE_RANK()`  
`PARTITION BY`  
`GROUP BY`  
`AVG()`  
`ROUND()`  
`LAG()`  
`LEAD()`  
`Self Join`  
`Window Functions`

### Tags

`SQL` `LeetCode` `SQL Practice` `SQL Interview` `Data Analytics` `Problem Solving` `Window Functions`
