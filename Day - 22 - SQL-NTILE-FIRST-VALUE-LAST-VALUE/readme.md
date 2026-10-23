# SQL NTILE(), FIRST_VALUE() & LAST_VALUE()

## Overview

`NTILE()`, `FIRST_VALUE()`, and `LAST_VALUE()` are advanced SQL Window Functions used for segmentation, comparison, and trend analysis.

They are useful for:

- Customer segmentation
- Quartile analysis
- First vs latest value comparison
- Trend tracking
- Performance analysis
- Time-based reporting

---

# 1. NTILE()

`NTILE(n)` divides ordered rows into `n` groups or buckets.

```sql
SELECT
    customer_id,
    sales,
    NTILE(4) OVER (
        ORDER BY sales DESC
    ) AS quartile
FROM customers;
```

If `NTILE(4)` is used:

```text
Bucket 1 → Highest group
Bucket 2 → Next group
Bucket 3 → Next group
Bucket 4 → Lowest group
```

With a sufficiently large, evenly distributed dataset, these are often interpreted as quartile-like groups.

---

## Example

| Customer | Sales | Bucket |
|---|---:|---:|
| C1 | 400 | 1 |
| C2 | 320 | 1 |
| C3 | 250 | 2 |
| C4 | 200 | 2 |
| C5 | 150 | 3 |
| C6 | 100 | 3 |
| C7 | 80 | 4 |
| C8 | 50 | 4 |

### Use Cases

- Customer value segmentation
- Sales performance bands
- Quartiles
- Marketing targeting
- Employee performance groups

---

# 2. NTILE() with PARTITION BY

Buckets can restart inside groups.

```sql
SELECT
    region,
    customer_id,
    sales,
    NTILE(4) OVER (
        PARTITION BY region
        ORDER BY sales DESC
    ) AS region_quartile
FROM customers;
```

Now each region gets its own buckets.

```text
East → Bucket 1–4
West → Bucket 1–4
South → Bucket 1–4
```

---

# 3. FIRST_VALUE()

`FIRST_VALUE()` returns the first value in the defined window.

```sql
SELECT
    month,
    sales,
    FIRST_VALUE(sales) OVER (
        ORDER BY month
    ) AS starting_sales
FROM monthly_sales;
```

Example:

| Month | Sales | Starting Sales |
|---|---:|---:|
| Jan | 100 | 100 |
| Feb | 120 | 100 |
| Mar | 150 | 100 |

### Simple Meaning

```text
Current Value
vs
Where We Started
```

---

# 4. FIRST_VALUE() with PARTITION BY

Compare each row with the starting value inside its own group.

```sql
SELECT
    region,
    month,
    sales,
    FIRST_VALUE(sales) OVER (
        PARTITION BY region
        ORDER BY month
    ) AS first_region_sales
FROM regional_sales;
```

Useful for comparing each region's current performance with its initial period.

---

# 5. LAST_VALUE()

`LAST_VALUE()` returns the last value within the current window frame.

```sql
SELECT
    month,
    sales,
    LAST_VALUE(sales) OVER (
        ORDER BY month
        ROWS BETWEEN UNBOUNDED PRECEDING
        AND UNBOUNDED FOLLOWING
    ) AS latest_sales
FROM monthly_sales;
```

Example:

| Month | Sales | Latest Sales |
|---|---:|---:|
| Jan | 100 | 150 |
| Feb | 120 | 150 |
| Mar | 150 | 150 |

---

# 6. Why the Window Frame Matters

This is one of the most important concepts to remember.

A query like:

```sql
LAST_VALUE(sales) OVER (
    ORDER BY month
)
```

may not return the final dataset value on every row because the default frame can end at the current row.

To explicitly include the entire ordered partition:

```sql
LAST_VALUE(sales) OVER (
    ORDER BY month
    ROWS BETWEEN UNBOUNDED PRECEDING
    AND UNBOUNDED FOLLOWING
)
```

### Easy Reminder

```text
FIRST_VALUE
→ usually straightforward

LAST_VALUE
→ always check the frame
```

---

# 7. First vs Latest Comparison

We can display both values together.

```sql
SELECT
    month,
    sales,

    FIRST_VALUE(sales) OVER (
        ORDER BY month
    ) AS first_sales,

    LAST_VALUE(sales) OVER (
        ORDER BY month
        ROWS BETWEEN UNBOUNDED PRECEDING
        AND UNBOUNDED FOLLOWING
    ) AS latest_sales

FROM monthly_sales;
```

This helps show where performance started and where it ended.

---

# 8. Growth from Starting Point

```sql
SELECT
    month,
    sales,
    sales -
    FIRST_VALUE(sales) OVER (
        ORDER BY month
    ) AS growth_from_start
FROM monthly_sales;
```

Example:

```text
Jan 100 → 0
Feb 120 → +20
Mar 150 → +50
```

---

# 9. Real-World Applications

## NTILE()

```text
Customer Segmentation
Top 25% Customers
Performance Quartiles
Risk Groups
```

## FIRST_VALUE()

```text
Starting Sales
Opening Price
Initial Customer Value
Beginning KPI
```

## LAST_VALUE()

```text
Latest Sales
Final Period Value
Closing Performance
Latest KPI
```

---

# Quick Comparison

| Function | Purpose |
|---|---|
| `NTILE(n)` | Divide ordered rows into buckets |
| `FIRST_VALUE()` | Return first value in window |
| `LAST_VALUE()` | Return last value in frame |

---

# Must-Remember Notes

### 1. NTILE depends on ORDER BY

The sorting determines which rows go into which bucket.

### 2. Buckets may not be perfectly equal

When the number of rows cannot be divided evenly, some buckets may contain one extra row.

### 3. PARTITION BY restarts analysis

Useful for region, category, department, or customer groups.

### 4. FIRST_VALUE keeps row detail

It adds the starting value without reducing rows.

### 5. LAST_VALUE requires frame awareness

Always verify whether the defined frame actually includes the final row.

---

# Interview Quick Recap

### What does NTILE(4) do?

Divides ordered rows into four buckets.

### Is NTILE the same as RANK?

No.

`RANK()` assigns ranking positions.

`NTILE()` assigns bucket numbers.

### What does FIRST_VALUE return?

The first value in the defined window.

### Why can LAST_VALUE be confusing?

Its output depends on the active window frame.

### How can you return the final value for the full partition?

```sql
ROWS BETWEEN UNBOUNDED PRECEDING
AND UNBOUNDED FOLLOWING
```

### Can these functions use PARTITION BY?

Yes.

This allows the analysis to restart within each group.

---

# Key Takeaway

```text
Raw Data
   ↓
Window Functions
   ↓
NTILE → Segmentation
FIRST_VALUE → Starting Point
LAST_VALUE → Ending Point
   ↓
Comparison & Trends
   ↓
Business Insights
```

These functions provide additional context while preserving every original row.

## Topics Covered

`NTILE()`  
`FIRST_VALUE()`  
`LAST_VALUE()`  
`PARTITION BY`  
`ORDER BY`  
`Window Frames`  
`Segmentation`  
`Trend Analysis`

### Tags

`SQL` `Window Functions` `NTILE` `FIRST_VALUE` `LAST_VALUE` `Data Analytics` `SQL Interview`
