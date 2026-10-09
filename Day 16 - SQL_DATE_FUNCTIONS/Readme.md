# SQL Date Functions

## Overview

Date functions help analysts work with time-based data such as:

- Sales dates
- Order dates
- Delivery timelines
- Monthly reports
- Customer activity
- Campaign periods

This section focuses on commonly used **Oracle SQL date functions**.

---

# 1. SYSDATE

`SYSDATE` returns the current date and time from the database server.

```sql
SELECT SYSDATE
FROM dual;
```

### Simple Meaning

```text
"What is the current date and time?"
```

### Use Cases

- Today's transactions
- Report generation date
- Current-age calculations
- Delivery comparisons

---

# 2. ADD_MONTHS()

Adds or subtracts months from a date.

```sql
SELECT ADD_MONTHS(
    DATE '2026-01-15',
    3
)
FROM dual;
```

Result:

```text
2026-04-15
```

Subtract months:

```sql
SELECT ADD_MONTHS(
    DATE '2026-06-15',
    -2
)
FROM dual;
```

Useful for subscriptions, renewals, forecasting, and future dates.

---

# 3. MONTHS_BETWEEN()

Calculates the difference between two dates in months.

```sql
SELECT MONTHS_BETWEEN(
    DATE '2026-06-01',
    DATE '2026-01-01'
)
FROM dual;
```

Result:

```text
5
```

### Simple Meaning

```text
"How many months separate these dates?"
```

Useful for:

- Customer tenure
- Employee experience
- Subscription duration

---

# 4. NEXT_DAY()

Returns the next occurrence of a specified weekday after a date.

```sql
SELECT NEXT_DAY(
    DATE '2026-01-15',
    'MONDAY'
)
FROM dual;
```

### Use Cases

- Delivery planning
- Meeting schedules
- Weekly reporting dates

---

# 5. LAST_DAY()

Returns the last day of the month containing the given date.

```sql
SELECT LAST_DAY(
    DATE '2026-02-10'
)
FROM dual;
```

Result:

```text
2026-02-28
```

Useful for:

- Month-end reporting
- Billing cycles
- Financial closing dates

---

# 6. EXTRACT()

Extracts one date component.

```sql
SELECT EXTRACT(
    YEAR FROM DATE '2026-10-09'
)
FROM dual;
```

Result:

```text
2026
```

Extract month:

```sql
SELECT EXTRACT(
    MONTH FROM DATE '2026-10-09'
)
FROM dual;
```

Result:

```text
10
```

Extract day:

```sql
SELECT EXTRACT(
    DAY FROM DATE '2026-10-09'
)
FROM dual;
```

Result:

```text
9
```

Useful for grouping and time-based analysis.

---

# 7. TRUNC()

`TRUNC(date)` removes the time portion from a date.

```sql
SELECT TRUNC(SYSDATE)
FROM dual;
```

For example:

```text
2026-10-09 18:45:20
        ↓
2026-10-09
```

This is extremely useful when comparing dates without worrying about time.

---

# Practical Business Queries

## Today's Sales

```sql
SELECT
    order_id,
    customer_id,
    order_date,
    amount
FROM sales
WHERE TRUNC(order_date) = TRUNC(SYSDATE);
```

### Logic

```text
Remove time
   ↓
Compare only calendar dates
   ↓
Return today's records
```

---

## Last 7 Calendar Days

```sql
SELECT
    order_id,
    customer_id,
    order_date,
    amount
FROM sales
WHERE order_date >= TRUNC(SYSDATE) - 6;
```

This includes today plus the previous six calendar days.

---

## Daily Revenue

```sql
SELECT
    TRUNC(order_date) AS sales_date,
    SUM(amount) AS daily_revenue
FROM sales
GROUP BY TRUNC(order_date)
ORDER BY sales_date;
```

Example output:

| sales_date | daily_revenue |
|---|---:|
| 2026-10-07 | 12500 |
| 2026-10-08 | 14800 |
| 2026-10-09 | 16200 |

---

# Quick Function Recap

| Function | Purpose |
|---|---|
| `SYSDATE` | Current date and time |
| `ADD_MONTHS()` | Add/subtract months |
| `MONTHS_BETWEEN()` | Difference in months |
| `NEXT_DAY()` | Next specified weekday |
| `LAST_DAY()` | Last day of month |
| `EXTRACT()` | Get year/month/day |
| `TRUNC()` | Remove time portion |

---

# Real-World Applications

## Daily Sales Tracking

```text
TRUNC(order_date)
→ Group or filter by calendar day
```

## Monthly Reporting

```text
EXTRACT(MONTH FROM order_date)
```

## Seasonal Offers

Historical dates can help identify seasonal buying patterns.

## Delivery Timelines

`NEXT_DAY()` and date arithmetic can help create scheduling logic.

---

# Must-Remember Notes

### 1. Date functions vary by database

Functions such as:

```text
SYSDATE
ADD_MONTHS
MONTHS_BETWEEN
NEXT_DAY
LAST_DAY
```

are strongly associated with Oracle SQL.

Other databases may use different syntax.

### 2. Dates may include time

Two values on the same day may still differ because their time components are different.

Use:

```sql
TRUNC(date_column)
```

when the business question is based only on the calendar date.

### 3. Avoid unnecessary functions on indexed date columns

For very large tables, applying functions directly to indexed columns can sometimes reduce index efficiency.

A range filter can often perform better.

### 4. Understand inclusive date ranges

Always check whether the requirement means:

```text
last 7 calendar days
last 7 × 24 hours
previous full week
```

These are different questions.

---

# Quick Recap

### What does SYSDATE return?

Current database date and time.

### ADD_MONTHS vs MONTHS_BETWEEN?

```text
ADD_MONTHS
→ creates another date

MONTHS_BETWEEN
→ returns a numeric difference
```

### Why use TRUNC(date)?

To remove time and compare calendar dates.

### What does LAST_DAY do?

Returns the final day of the given month.

### How do you extract a year?

```sql
EXTRACT(YEAR FROM date_column)
```

### How would you calculate daily revenue?

```sql
SELECT TRUNC(order_date),
       SUM(amount)
FROM sales
GROUP BY TRUNC(order_date);
```

---

# Key Takeaway

```text
Date & Time Data
      ↓
Date Functions
      ↓
Periods / Comparisons
      ↓
Trends
      ↓
Business Insights
```

Date functions are essential because business performance is rarely analyzed without considering **when** something happened.

## Topics Covered

`SYSDATE`  
`ADD_MONTHS()`  
`MONTHS_BETWEEN()`  
`NEXT_DAY()`  
`LAST_DAY()`  
`EXTRACT()`  
`TRUNC()`  
`Daily Revenue`  
`Date Filtering`

### Tags

`SQL` `Oracle SQL` `Date Functions` `Data Analytics` `Business Analytics` `SQL Interview`
