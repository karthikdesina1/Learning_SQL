# SQL Window Framing - Running, Rolling & Moving Totals

## Overview

Window Framing controls **which rows are included in a Window Function calculation**.

It is commonly used for:

- Running totals
- Rolling sums
- Moving averages
- Full-window aggregations
- Time-series analysis
- Business reporting

---

## Basic Syntax

```sql
window_function(expression)
OVER (
    PARTITION BY column_name
    ORDER BY column_name
    ROWS BETWEEN frame_start AND frame_end
)
```

### Important Frame Terms

```text
CURRENT ROW
→ current row

N PRECEDING
→ previous N rows

N FOLLOWING
→ next N rows

UNBOUNDED PRECEDING
→ beginning of the partition

UNBOUNDED FOLLOWING
→ end of the partition
```

---

# 1. Running Total

A running total adds values from the beginning up to the current row.

```sql
SELECT
    order_date,
    sales,
    SUM(sales) OVER (
        ORDER BY order_date
        ROWS BETWEEN UNBOUNDED PRECEDING
        AND CURRENT ROW
    ) AS running_total
FROM sales;
```

Example:

```text
Sales   Running Total
100     100
200     300
150     450
300     750
```

### Use Cases

- Cumulative revenue
- Budget tracking
- Monthly growth
- Target progress

---

# 2. Rolling Sum - Last 3 Rows

A rolling sum calculates over a fixed recent window.

```sql
SELECT
    order_date,
    sales,
    SUM(sales) OVER (
        ORDER BY order_date
        ROWS BETWEEN 2 PRECEDING
        AND CURRENT ROW
    ) AS rolling_sum_3
FROM sales;
```

For each row, SQL uses:

```text
Previous 2 Rows
+ Current Row
```

Example:

```text
100 → 100
200 → 300
150 → 450
300 → 650
```

---

# 3. Moving Average - Last 3 Rows

A moving average smooths short-term fluctuations.

```sql
SELECT
    order_date,
    sales,
    AVG(sales) OVER (
        ORDER BY order_date
        ROWS BETWEEN 2 PRECEDING
        AND CURRENT ROW
    ) AS moving_avg_3
FROM sales;
```

Example:

```text
100           → 100.00
100,200       → 150.00
100,200,150   → 150.00
200,150,300   → 216.67
```

### Use Cases

- Sales trends
- Website traffic
- Financial prices
- Demand analysis

---

# 4. Full Dataset Aggregation

Window Functions can calculate a total across the complete dataset while keeping every row.

```sql
SELECT
    order_date,
    sales,
    SUM(sales) OVER (
        ROWS BETWEEN UNBOUNDED PRECEDING
        AND UNBOUNDED FOLLOWING
    ) AS total_sales
FROM sales;
```

If total sales are 1,400:

```text
Row 1 → 1400
Row 2 → 1400
Row 3 → 1400
...
```

The original rows remain visible.

---

# 5. PRECEDING and FOLLOWING

## PRECEDING

Looks backward from the current row.

```sql
ROWS BETWEEN 2 PRECEDING
AND CURRENT ROW
```

## FOLLOWING

Looks forward from the current row.

```sql
ROWS BETWEEN CURRENT ROW
AND 2 FOLLOWING
```

This can be useful when analyzing future adjacent rows.

---

# GROUP BY vs Window Framing

```text
GROUP BY
→ combines multiple rows
→ reduces result rows

Window Function
→ calculates across related rows
→ keeps original rows
```

Window framing goes one step further by controlling exactly which rows inside the window are used.

---

# Important WHERE Caution

SQL logically applies `WHERE` before Window Functions.

So:

```sql
SELECT
    order_date,
    sales,
    SUM(sales) OVER (
        ORDER BY order_date
    ) AS running_total
FROM sales
WHERE order_date >= DATE '2026-01-03';
```

calculates the running total only on rows that survived the filter.

If the business requirement needs calculations over a broader dataset and filtering afterward, use a subquery or CTE.

Example:

```sql
WITH calculated_sales AS (
    SELECT
        order_date,
        sales,
        SUM(sales) OVER (
            ORDER BY order_date
            ROWS BETWEEN UNBOUNDED PRECEDING
            AND CURRENT ROW
        ) AS running_total
    FROM sales
)
SELECT *
FROM calculated_sales
WHERE order_date >= DATE '2026-01-03';
```

---

# Real-World Applications

## Running Total

```text
Cumulative Sales
Cumulative Revenue
Budget Progress
```

## Rolling Sum

```text
Last 7 Days Sales
Last 3 Transactions
Rolling KPI
```

## Moving Average

```text
Sales Trend
Price Trend
Demand Trend
```

## Full Window

```text
Compare each row with total business performance
```

---

# Interview Quick Recap

### What is Window Framing?

It defines which rows participate in a Window Function calculation.

### What does PRECEDING mean?

Rows before the current row.

### What does FOLLOWING mean?

Rows after the current row.

### How do you create a running total?

```sql
SUM(value) OVER (
    ORDER BY date
    ROWS BETWEEN UNBOUNDED PRECEDING
    AND CURRENT ROW
)
```

### How do you calculate a 3-row moving average?

```sql
AVG(value) OVER (
    ORDER BY date
    ROWS BETWEEN 2 PRECEDING
    AND CURRENT ROW
)
```

### Why can WHERE change Window Function results?

Because rows are filtered before the window calculation is performed.

---

# Must-Remember Notes

1. Always define a meaningful `ORDER BY` for running or rolling calculations.
2. `2 PRECEDING + CURRENT ROW` represents a 3-row frame.
3. Window Functions preserve row-level detail.
4. Filtering too early can change the frame and therefore the result.
5. Use a subquery or CTE when you need to filter after calculating the Window Function.

---

# Key Takeaway

```text
Detailed Data
     ↓
ORDER BY
     ↓
Window Frame
     ↓
SUM / AVG
     ↓
Running / Rolling / Moving Metrics
     ↓
Business Insights
```

Window Framing gives analysts precise control over **which rows contribute to each calculation**.

## Topics Covered

`Window Framing`  
`ROWS BETWEEN`  
`PRECEDING`  
`FOLLOWING`  
`Running Total`  
`Rolling Sum`  
`Moving Average`  
`SUM()`  
`AVG()`

### Tags

`SQL` `Window Functions` `Window Framing` `Data Analytics` `Time Series` `SQL Interview`
