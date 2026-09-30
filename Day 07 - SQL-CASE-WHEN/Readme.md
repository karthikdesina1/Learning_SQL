# SQL CASE WHEN

## Overview

`CASE WHEN` is one of the most useful SQL expressions for applying **conditional logic** inside a query.

It allows analysts to:

- Categorize data
- Create conditional calculations
- Build dynamic reports
- Compare groups
- Create business rules directly in SQL
- Generate KPI logic

It works similarly to an `IF...ELSE` condition.

---

## Basic Syntax

```sql
CASE
    WHEN condition1 THEN value1
    WHEN condition2 THEN value2
    ELSE value3
END
```

SQL evaluates the conditions from top to bottom and returns the result of the first matching condition.

---

## 1. Categorizing Data

Example:

```sql
SELECT
    order_id,
    amount,
    CASE
        WHEN amount >= 1000 THEN 'High'
        WHEN amount >= 500 THEN 'Medium'
        ELSE 'Low'
    END AS order_category
FROM orders;
```

Example result:

| Order | Amount | Category |
|---|---:|---|
| 101 | 1200 | High |
| 102 | 750 | Medium |
| 103 | 300 | Low |

This is useful when raw numerical values need to be converted into business-friendly categories.

---

## 2. CASE WHEN with SUM()

`CASE WHEN` can be used inside `SUM()` for conditional aggregation.

```sql
SELECT
    SUM(CASE
        WHEN payment_method = 'Card'
        THEN amount
        ELSE 0
    END) AS card_total,

    SUM(CASE
        WHEN payment_method = 'UPI'
        THEN amount
        ELSE 0
    END) AS upi_total
FROM payments;
```

This allows multiple conditional totals to be calculated in a single query.

### Use Cases

- Revenue by payment method
- Sales by region
- Revenue from premium customers
- Completed vs cancelled order value

---

## 3. CASE WHEN with COUNT()

Conditional counts can be created using `COUNT()`.

```sql
SELECT
    COUNT(CASE
        WHEN status = 'Completed'
        THEN 1
    END) AS completed_orders,

    COUNT(CASE
        WHEN status = 'Cancelled'
        THEN 1
    END) AS cancelled_orders
FROM orders;
```

This is commonly used in dashboards and KPI reporting.

---

## 4. CASE WHEN with GROUP BY

`CASE WHEN` can first create categories and then analyze them using `GROUP BY`.

```sql
SELECT
    CASE
        WHEN amount >= 1000 THEN 'High'
        WHEN amount >= 500 THEN 'Medium'
        ELSE 'Low'
    END AS order_category,

    COUNT(*) AS total_orders,
    SUM(amount) AS total_amount

FROM orders

GROUP BY
    CASE
        WHEN amount >= 1000 THEN 'High'
        WHEN amount >= 500 THEN 'Medium'
        ELSE 'Low'
    END;
```

This helps compare category-level performance.

---

## 5. CASE WHEN for Business Logic

CASE expressions are useful when a report needs custom rules.

Example:

```sql
SELECT
    customer_id,
    total_spend,
    CASE
        WHEN total_spend >= 10000 THEN 'VIP'
        WHEN total_spend >= 5000 THEN 'Premium'
        ELSE 'Standard'
    END AS customer_segment
FROM customers;
```

This allows analysts to convert raw data into business segments.

---

## 6. CASE WHEN in Filtering Logic

`CASE WHEN` can also be used in more advanced `WHERE` or `HAVING` conditions.

Example:

```sql
SELECT *
FROM orders
WHERE
    CASE
        WHEN customer_type = 'Premium'
             AND amount > 1000
        THEN 1
        ELSE 0
    END = 1;
```

In many cases, simple Boolean conditions are clearer, but this demonstrates how CASE can support dynamic logic.

---

## Important Rules

### Rule 1

Conditions are checked from top to bottom.

```text
First matching condition wins.
```

### Rule 2

Use `ELSE` when possible.

If `ELSE` is omitted and no condition matches, SQL usually returns `NULL`.

### Rule 3

Return values should ideally be of compatible data types.

### Rule 4

Keep conditions readable and avoid unnecessary complexity.

### Rule 5

Use aliases so derived values are easy to understand.

```sql
END AS customer_segment
```

---

## Real-World Applications

### Customer Segmentation

```text
High Value
Medium Value
Low Value
```

### Transaction Classification

```text
Large Transaction
Normal Transaction
Small Transaction
```

### Conditional Totals

```text
Card Revenue
UPI Revenue
Cash Revenue
```

### Dynamic Reports

CASE expressions can create:

- Performance labels
- Risk categories
- Customer tiers
- Revenue bands
- Order status groups
- KPI flags

---

## Interview Quick Recap

### What is CASE WHEN?

A conditional expression used to return different values depending on whether conditions are true or false.

### Is CASE WHEN a function?

It is generally treated as a SQL conditional expression rather than a normal function.

### Can CASE WHEN be used with SUM()?

Yes.

```sql
SUM(CASE WHEN condition THEN value ELSE 0 END)
```

This is called conditional aggregation.

### Can CASE WHEN be used with COUNT()?

Yes.

```sql
COUNT(CASE WHEN condition THEN 1 END)
```

### Does condition order matter?

Yes.

SQL returns the value for the **first matching WHEN condition**.

### What happens if ELSE is missing?

If no condition matches, SQL typically returns `NULL`.

### CASE WHEN vs WHERE

`WHERE` filters rows.

`CASE WHEN` usually transforms or categorizes values.

---

## Business Example

### Question

How much revenue came from high-value and regular orders?

```sql
SELECT
    SUM(CASE
        WHEN amount >= 1000
        THEN amount
        ELSE 0
    END) AS high_value_revenue,

    SUM(CASE
        WHEN amount < 1000
        THEN amount
        ELSE 0
    END) AS regular_revenue

FROM orders;
```

This turns transaction-level data into useful business metrics.

---

## Key Takeaway

```text
Raw Data
   ↓
CASE WHEN
   ↓
Conditions
   ↓
Categories / Calculations
   ↓
Analysis
   ↓
Business Insights
```

`CASE WHEN` is especially valuable because it lets analysts embed business logic directly inside SQL queries.

---

## Topics Covered

- CASE WHEN
- Conditional Logic
- SUM()
- COUNT()
- GROUP BY
- Conditional Aggregation
- Data Categorization
- Business Segmentation
- Dynamic Reporting

### Tags

`SQL` `CASE WHEN` `Data Analytics` `Business Analytics` `Conditional Aggregation` `SQL Interview` `Business Intelligence`
