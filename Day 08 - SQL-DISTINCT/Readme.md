# SQL DISTINCT

## Overview

`DISTINCT` is a SQL keyword used to remove duplicate rows from a query result and return only unique values or unique combinations.

It is especially useful in real-world analytics where repeated values are common.

Typical use cases include:

- Unique customers
- Distinct products
- Payment methods
- Store categories
- Region/category combinations
- Unique values used in reporting

---

## 1. Basic DISTINCT

```sql
SELECT DISTINCT city
FROM customers;
```

Example data:

```text
New York
Chicago
New York
Boston
Chicago
```

Result:

```text
New York
Chicago
Boston
```

`DISTINCT` removes repeated values from the query output.

---

## 2. DISTINCT on Multiple Columns

```sql
SELECT DISTINCT
    store_id,
    payment_method
FROM sales;
```

Important:

`DISTINCT` applies to the **entire selected combination**.

Example:

| store_id | payment_method |
|---|---|
| 101 | Card |
| 101 | Cash |
| 102 | Card |
| 102 | UPI |

Store `101` can appear more than once because the payment method is different.

### Must Remember

```text
DISTINCT column1, column2
```

means:

```text
Unique combination of column1 + column2
```

not:

```text
Each column separately unique
```

---

## 3. DISTINCT with WHERE

`WHERE` can filter rows before the distinct result is returned.

```sql
SELECT DISTINCT payment_method
FROM payments
WHERE amount > 500;
```

This returns payment methods used only in transactions above 500.

---

## 4. DISTINCT with ORDER BY

```sql
SELECT DISTINCT payment_method
FROM payments
WHERE amount > 500
ORDER BY payment_method;
```

Conceptual flow:

```text
FROM
  ↓
WHERE
  ↓
SELECT DISTINCT
  ↓
ORDER BY
```

---

## 5. COUNT(DISTINCT)

To count unique values:

```sql
SELECT COUNT(DISTINCT customer_id)
FROM orders;
```

Example:

```text
Total Orders      = 5,000
Unique Customers  = 1,250
```

This is useful when one customer can appear in multiple transactions.

---

# DISTINCT vs UNIQUE vs GROUP BY

| Concept | Purpose |
|---|---|
| `DISTINCT` | Removes duplicate rows from a query result |
| `UNIQUE` | Prevents duplicate values through a database constraint |
| `GROUP BY` | Groups rows for aggregation and summarization |

---

## DISTINCT

```sql
SELECT DISTINCT city
FROM customers;
```

Used when the goal is to retrieve unique query results.

---

## UNIQUE

Example:

```sql
CREATE TABLE customers (
    customer_id INT PRIMARY KEY,
    email VARCHAR(150) UNIQUE
);
```

`UNIQUE` helps maintain data integrity by preventing duplicate values.

---

## GROUP BY

```sql
SELECT city,
       COUNT(*) AS customer_count
FROM customers
GROUP BY city;
```

`GROUP BY` is useful when the objective is not only uniqueness, but also aggregation.

---

# Real-World Applications

## Unique Customers

```sql
SELECT DISTINCT customer_id
FROM orders;
```

---

## Distinct Products Sold

```sql
SELECT DISTINCT product_name
FROM sales;
```

---

## Payment Methods

```sql
SELECT DISTINCT payment_method
FROM payments;
```

---

## Store and Category Combinations

```sql
SELECT DISTINCT
    store_id,
    category
FROM sales;
```

This identifies unique store/category combinations.

---

# DISTINCT with Business Analysis

### Business Question

Which payment methods were used for transactions above $1,000?

```sql
SELECT DISTINCT payment_method
FROM payments
WHERE amount > 1000
ORDER BY payment_method;
```

This query combines:

- Filtering
- Deduplication
- Sorting

---

# Must-Remember Rules

### Rule 1

`DISTINCT` applies to the selected columns.

```sql
SELECT DISTINCT city
FROM customers;
```

Only `city` determines uniqueness.

---

### Rule 2

With multiple columns, the complete combination is evaluated.

```sql
SELECT DISTINCT city, state
FROM customers;
```

SQL returns unique `city + state` combinations.

---

### Rule 3

NULL values are also considered when determining distinct results.

The exact handling of multiple NULLs can depend on the SQL operation and database system, but `SELECT DISTINCT` generally represents duplicate NULLs as one distinct result.

---

### Rule 4

Use `COUNT(DISTINCT column)` when only the number of unique values is required.

```sql
SELECT COUNT(DISTINCT product_id)
FROM sales;
```

---

### Rule 5

Do not use `DISTINCT` as a shortcut to fix an incorrect JOIN.

Unexpected duplicates after a JOIN may indicate:

- Incorrect join conditions
- One-to-many relationships
- Duplicate source data

Understand the cause before removing duplicates.

---

# Should-Remember Practices

- Select only the columns required
- Use DISTINCT only when uniqueness matters
- Review NULL behavior
- Check JOIN logic before applying DISTINCT
- Consider whether GROUP BY is more appropriate
- Validate that removing duplicates does not hide useful detail

---

# Performance Note

⚠️ `DISTINCT` can sometimes slow down queries on very large datasets.

The database may need additional work to:

- Sort values
- Compare rows
- Remove duplicate results

For example:

```sql
SELECT DISTINCT *
FROM very_large_table;
```

may be expensive because SQL must evaluate uniqueness across every selected column.

A better approach is to retrieve only the required fields:

```sql
SELECT DISTINCT customer_id
FROM orders;
```

---

# Interview Quick Recap

### What does DISTINCT do?

Removes duplicate rows from a query result.

### Can DISTINCT be used on multiple columns?

Yes. It returns unique combinations of the selected columns.

### DISTINCT vs GROUP BY?

`DISTINCT` removes duplicate output rows.

`GROUP BY` creates groups and is commonly used with aggregate functions.

### DISTINCT vs UNIQUE?

`DISTINCT` is used in queries.

`UNIQUE` is commonly a table constraint.

### How do you count unique values?

```sql
COUNT(DISTINCT column_name)
```

### Can DISTINCT affect performance?

Yes, especially with large datasets or many selected columns.

---

# Key Takeaway

```text
Duplicate Data
     ↓
DISTINCT
     ↓
Unique Results
     ↓
Cleaner Analysis
     ↓
Business Insights
```

The most important idea is not just knowing the syntax.

It is understanding **which columns define uniqueness in the business question being answered**.

## Topics Covered

`DISTINCT`  
`COUNT(DISTINCT)`  
`WHERE`  
`ORDER BY`  
`UNIQUE`  
`GROUP BY`  
`Duplicate Handling`

### Tags

`SQL` `DISTINCT` `Data Analytics` `Business Analytics` `SQL Interview` `Database` `Data Quality`
