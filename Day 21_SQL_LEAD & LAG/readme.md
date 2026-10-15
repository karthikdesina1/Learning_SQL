# SQL LEAD() & LAG()

## Overview

`LEAD()` and `LAG()` are SQL Window Functions used to access values from following or previous rows without using a self-join.

They are especially useful for:

- Time-series analysis
- Period-over-period comparison
- Sales growth analysis
- Trend detection
- Performance tracking

---

## Basic Difference

```text id="29baf5"
LAG()
→ looks backward

LEAD()
→ looks forward
```

---

# 1. LAG()

`LAG()` retrieves a value from a previous row.

```sql id="x2jz8e"
SELECT
    month,
    sales,
    LAG(sales) OVER (
        ORDER BY month
    ) AS previous_sales
FROM monthly_sales;
```

Example:

| Month | Sales | Previous Sales |
|---|---:|---:|
| Jan | 100 | NULL |
| Feb | 120 | 100 |
| Mar | 90 | 120 |
| Apr | 150 | 90 |

The first row returns `NULL` because no previous row exists.

---

# 2. LEAD()

`LEAD()` retrieves a value from a following row.

```sql id="myo4dq"
SELECT
    month,
    sales,
    LEAD(sales) OVER (
        ORDER BY month
    ) AS next_sales
FROM monthly_sales;
```

Example:

| Month | Sales | Next Sales |
|---|---:|---:|
| Jan | 100 | 120 |
| Feb | 120 | 90 |
| Mar | 90 | 150 |
| Apr | 150 | NULL |

The final row returns `NULL` because there is no following row.

---

# 3. OFFSET

The optional offset tells SQL how many rows to move.

Default:

```sql id="3khffb"
LAG(sales, 1)
```

One row back.

Two rows back:

```sql id="3l1ivx"
LAG(sales, 2)
```

Two rows forward:

```sql id="jidn1u"
LEAD(sales, 2)
```

---

# 4. Default Value

A third argument can provide a value when no previous or next row exists.

```sql id="lb5xu4"
LAG(sales, 1, 0) OVER (
    ORDER BY month
)
```

Instead of returning `NULL`, SQL returns `0`.

General syntax:

```sql id="cjj32g"
LAG(column, offset, default_value)
```

and:

```sql id="0oxais"
LEAD(column, offset, default_value)
```

---

# 5. PARTITION BY

`PARTITION BY` performs comparisons independently inside groups.

```sql id="z9koum"
SELECT
    region,
    month,
    sales,
    LAG(sales) OVER (
        PARTITION BY region
        ORDER BY month
    ) AS previous_sales
FROM regional_sales;
```

Conceptually:

```text id="xkcv3e"
East
Jan → Feb → Mar

West
Jan → Feb → Mar
```

The comparison restarts for each region.

---

# 6. Sales Change

A common use case is calculating the difference from the previous period.

```sql id="g438se"
SELECT
    month,
    sales,
    LAG(sales) OVER (
        ORDER BY month
    ) AS previous_sales,

    sales -
    LAG(sales) OVER (
        ORDER BY month
    ) AS sales_change

FROM monthly_sales;
```

Example:

```text id="q6wew8"
Jan 100 → NULL
Feb 120 → +20
Mar 90  → -30
Apr 150 → +60
```

---

# 7. Growth Percentage

A more useful analytical metric is percentage growth.

```sql id="gfapfz"
SELECT
    month,
    sales,
    LAG(sales) OVER (
        ORDER BY month
    ) AS previous_sales,

    ROUND(
        (
            sales -
            LAG(sales) OVER (ORDER BY month)
        ) /
        NULLIF(
            LAG(sales) OVER (ORDER BY month),
            0
        ) * 100,
        2
    ) AS growth_pct

FROM monthly_sales;
```

`NULLIF()` helps avoid division-by-zero errors.

---

# LEAD/LAG vs Self Join

Without Window Functions, previous-row analysis often requires joining the table to itself.

With `LAG()`:

```sql id="mmdlng"
LAG(sales) OVER (
    ORDER BY month
)
```

the same logic is usually shorter and easier to read.

### Benefits

- Less complex SQL
- Clear analytical intent
- Easier maintenance
- Keeps row-level detail

---

# Real-World Applications

## Month-over-Month Sales

```text id="i5pgiu"
Current Month
vs
Previous Month
```

## Price Change

Compare current price with the previous recorded price.

## Customer Activity

Compare current purchase value with a customer's previous purchase.

## Employee Performance

Compare one period's KPI with the previous period.

## Trend Detection

Identify:

```text id="sbujre"
Growth
Decline
Stable Performance
```

---

# Must-Remember Notes

### 1. ORDER BY is critical

`LEAD()` and `LAG()` depend on row order.

Without meaningful ordering, “previous” and “next” have little analytical meaning.

### 2. First and last rows may return NULL

```text id="zlt6k6"
LAG()
→ first row may have no previous value

LEAD()
→ last row may have no next value
```

### 3. PARTITION BY restarts the calculation

Useful for:

- Region
- Customer
- Product
- Department

### 4. Offset defaults to 1

Specify another value only when the analysis needs it.

### 5. LEAD and LAG do not reduce rows

They preserve every row and add contextual values.

---

# Interview Quick Recap

### What does LAG() do?

Returns a value from a previous row.

### What does LEAD() do?

Returns a value from a following row.

### What is offset?

The number of rows backward or forward to move.

### What is the default offset?

```text id="tcu5vl"
1
```

### Why use PARTITION BY?

To perform previous/next comparisons independently within groups.

### Why are LEAD and LAG useful?

They simplify row-to-row comparisons and can replace many self-join patterns.

---

# Key Takeaway

```text id="9ozosd"
Previous Row ← LAG()
      Current Row
Next Row     → LEAD()
```

Together they help transform:

```text id="67k87v"
Time-Based Rows
      ↓
Previous / Next Context
      ↓
Change Calculation
      ↓
Trend Detection
      ↓
Business Insights
```

## Topics Covered

`LEAD()`  
`LAG()`  
`OFFSET`  
`PARTITION BY`  
`ORDER BY`  
`Growth Analysis`  
`Trend Detection`

### Tags

`SQL` `LEAD` `LAG` `Window Functions` `Data Analytics` `Time Series` `SQL Interview`
