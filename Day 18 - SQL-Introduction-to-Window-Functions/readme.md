# Introduction to SQL Window Functions

## Overview

Window Functions perform calculations across a set of related rows while keeping each original row in the result.

This makes them especially useful for:

- Ranking
- Top-N analysis
- Performance comparison
- Running totals
- Moving averages
- Category-level analysis

Unlike `GROUP BY`, Window Functions do not collapse the result into one row per group.

---

## Basic Syntax

```sql id="q22fyf"
function_name() OVER (
    PARTITION BY column_name
    ORDER BY column_name
)
```

### Main Parts

```text id="vn2kas"
function_name()
→ calculation

OVER()
→ defines the analytical window

PARTITION BY
→ creates groups without reducing rows

ORDER BY
→ controls order inside each partition
```

---

# GROUP BY vs PARTITION BY

## GROUP BY

```sql id="4m53xu"
SELECT
    category,
    SUM(sales) AS total_sales
FROM sales
GROUP BY category;
```

Result:

```text id="81g06w"
Electronics → 3800
Home        → 4500
```

Multiple rows are collapsed into summary rows.

---

## PARTITION BY

```sql id="v8v4js"
SELECT
    salesperson,
    category,
    sales,
    SUM(sales) OVER (
        PARTITION BY category
    ) AS category_total
FROM sales;
```

Every salesperson row remains visible while the category total is added.

### Easy Reminder

```text id="q5ryfd"
GROUP BY
→ reduces rows

PARTITION BY
→ keeps rows
```

---

# 1. RANK()

`RANK()` assigns a ranking based on the specified order.

If values tie:

- They receive the same rank
- The next rank is skipped

```sql id="6v17vo"
SELECT
    salesperson,
    category,
    sales,
    RANK() OVER (
        PARTITION BY category
        ORDER BY sales DESC
    ) AS sales_rank
FROM sales;
```

Example result:

```text id="4cltea"
Sales   Rank
1400    1
1100    2
1100    2
900     4
```

### Use Cases

- Sales leaderboards
- Employee performance
- Product rankings

---

# 2. DENSE_RANK()

`DENSE_RANK()` behaves like `RANK()` but does not create gaps.

```sql id="8qk4h8"
SELECT
    salesperson,
    category,
    sales,
    DENSE_RANK() OVER (
        PARTITION BY category
        ORDER BY sales DESC
    ) AS sales_rank
FROM sales;
```

Example:

```text id="db7xjg"
Sales   Rank
1400    1
1100    2
1100    2
900     3
```

### Use When

Ranks should remain consecutive even when ties exist.

---

# 3. ROW_NUMBER()

`ROW_NUMBER()` assigns a unique sequential number to each row.

```sql id="vpsad4"
SELECT
    salesperson,
    category,
    sales,
    ROW_NUMBER() OVER (
        PARTITION BY category
        ORDER BY sales DESC
    ) AS row_num
FROM sales;
```

Example:

```text id="8yze09"
Sales   Row Number
1400    1
1100    2
1100    3
900     4
```

Even tied values receive different row numbers.

---

# Ranking Comparison

| Function | Ties | Gaps? | Unique Number? |
|---|---|---|---|
| `RANK()` | Same rank | Yes | No |
| `DENSE_RANK()` | Same rank | No | No |
| `ROW_NUMBER()` | Different numbers | No | Yes |

---

# Top-N Analysis

A common analytical use case is finding the Top 3 performers per category.

```sql id="v6wno1"
SELECT *
FROM (
    SELECT
        salesperson,
        category,
        sales,
        ROW_NUMBER() OVER (
            PARTITION BY category
            ORDER BY sales DESC
        ) AS rn
    FROM sales
)
WHERE rn <= 3;
```

This returns the Top 3 rows within every category.

---

# Performance Comparison

Window Functions can compare each row with its group.

```sql id="i1j5s8"
SELECT
    salesperson,
    category,
    sales,
    AVG(sales) OVER (
        PARTITION BY category
    ) AS category_avg
FROM sales;
```

This keeps each salesperson while showing the category average beside them.

---

# Real-World Applications

## Leaderboards

Rank:

- Employees
- Products
- Stores
- Regions

## Top-N Analysis

Find:

```text id="yvh9t6"
Top 3 products per category
Top 5 customers per region
Top salesperson per department
```

## Performance Comparison

Compare individual performance with:

- Department average
- Category average
- Regional performance

---

# Must-Remember Notes

### 1. Window Functions do not reduce rows

This is the biggest difference from aggregation with `GROUP BY`.

### 2. PARTITION BY is optional

Without it:

```sql id="5qjj79"
RANK() OVER (
    ORDER BY sales DESC
)
```

the ranking is calculated across the entire result set.

### 3. ORDER BY matters for ranking functions

Ranking requires a clear ordering rule.

### 4. Tie behavior differs

Always choose between:

```text id="2k049m"
RANK()
DENSE_RANK()
ROW_NUMBER()
```

based on how tied values should be handled.

### 5. ROW_NUMBER needs deterministic ordering

If several rows have the same ordering value, add another column when consistent results matter.

Example:

```sql id="8jclv8"
ORDER BY sales DESC, salesperson
```

---

# Interview Quick Recap

### What is a Window Function?

A function that calculates across related rows while keeping individual rows in the result.

### GROUP BY vs PARTITION BY?

```text id="0emw6z"
GROUP BY
→ summarizes and reduces rows

PARTITION BY
→ creates analytical groups without reducing rows
```

### RANK vs DENSE_RANK?

```text id="r03w0g"
RANK
→ leaves gaps after ties

DENSE_RANK
→ no gaps
```

### RANK vs ROW_NUMBER?

```text id="kpygqj"
RANK
→ tied values can share rank

ROW_NUMBER
→ every row gets a unique number
```

### Can Window Functions work without PARTITION BY?

Yes. The whole result set becomes one window.

---

# Key Takeaway

```text id="c7h96g"
Detailed Rows
     ↓
Window Function
     ↓
Partition / Order
     ↓
Ranking & Comparison
     ↓
Deeper Analytical Insight
```

Window Functions are powerful because they provide **summary context without sacrificing row-level detail**.

## Topics Covered

`Window Functions`  
`OVER()`  
`PARTITION BY`  
`RANK()`  
`DENSE_RANK()`  
`ROW_NUMBER()`  
`GROUP BY`  
`Top-N Analysis`

### Tags

`SQL` `Window Functions` `Data Analytics` `SQL Interview` `Ranking Functions` `Business Analytics`
