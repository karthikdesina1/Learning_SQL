# SQL COUNT & Filtering

## Overview

This learning section focuses on two important SQL concepts:

* `COUNT()`
* Filtering

These concepts are commonly used in Data Analytics and Business Intelligence to measure records, identify unique values, and answer business questions using conditions.

---

## 1. COUNT()

The `COUNT()` function is used to calculate the number of records.

### COUNT(*)

Counts all rows.

```sql
SELECT COUNT(*)
FROM orders;
```

Example result:

```text
Total Orders = 5,000
```

---

## 2. COUNT(1)

`COUNT(1)` also counts rows.

```sql
SELECT COUNT(1)
FROM orders;
```

In many modern database systems, `COUNT(*)` and `COUNT(1)` are typically optimized similarly.

For readability, `COUNT(*)` is commonly preferred when the goal is simply to count rows.

---

## 3. COUNT(column)

Counts only the non-NULL values in a specific column.

```sql
SELECT COUNT(email)
FROM customers;
```

If 1,000 customers exist but 80 have no email:

```text
COUNT(*)     = 1000
COUNT(email) = 920
```

This distinction is useful when checking data completeness.

---

## 4. COUNT(DISTINCT)

Used to count unique values.

```sql
SELECT COUNT(DISTINCT customer_id)
FROM orders;
```

Example:

```text
Total Orders      = 5,000
Unique Customers  = 1,250
```

One customer may create multiple transactions, which is why total records and unique records can differ.

---

# COUNT Comparison

| Function                 | Purpose                |
| ------------------------ | ---------------------- |
| `COUNT(*)`               | Counts all rows        |
| `COUNT(1)`               | Counts rows            |
| `COUNT(column)`          | Counts non-NULL values |
| `COUNT(DISTINCT column)` | Counts unique values   |

---

# 5. Filtering with WHERE

The `WHERE` clause filters records before they are analyzed.

```sql
SELECT *
FROM orders
WHERE amount > 500;
```

This returns only orders with an amount greater than 500.

---

# 6. AND Operator

`AND` requires all conditions to be true.

```sql
SELECT COUNT(*)
FROM orders
WHERE store = 'New York'
AND amount > 500;
```

This counts orders from the New York store where the value is greater than 500.

---

# 7. OR Operator

`OR` allows any condition to be true.

```sql
SELECT COUNT(*)
FROM payments
WHERE payment_method = 'Card'
OR payment_method = 'UPI';
```

---

# 8. IN Operator

`IN` is useful when matching several values.

```sql
SELECT COUNT(*)
FROM payments
WHERE payment_method IN ('Card', 'UPI', 'Wallet');
```

This is cleaner than writing multiple `OR` conditions.

---

# 9. NOT IN

Used to exclude selected values.

```sql
SELECT *
FROM customers
WHERE city NOT IN ('Boston', 'Chicago');
```

---

# 10. BETWEEN

Used for range filtering.

```sql
SELECT COUNT(*)
FROM orders
WHERE amount BETWEEN 100 AND 1000;
```

`BETWEEN` includes both boundary values.

---

# Real-World Analytics Example

### Business Question

How many Card and UPI transactions occurred between $100 and $1,000?

```sql
SELECT COUNT(*) AS transaction_count
FROM payments
WHERE payment_method IN ('Card', 'UPI')
AND amount BETWEEN 100 AND 1000;
```

This combines:

* COUNT
* WHERE
* IN
* AND
* BETWEEN

---

# Business Use Cases

## Total Transactions

```sql
SELECT COUNT(*)
FROM transactions;
```

## Unique Customers

```sql
SELECT COUNT(DISTINCT customer_id)
FROM transactions;
```

## Store Activity

```sql
SELECT store_id,
       COUNT(*) AS transaction_count
FROM transactions
GROUP BY store_id;
```

## Payment Method Trends

```sql
SELECT payment_method,
       COUNT(*) AS usage_count
FROM payments
GROUP BY payment_method;
```

These queries can support dashboard metrics and operational analysis.

---

# Analytical Thinking

A useful SQL workflow is:

```text
Business Question
      ↓
Identify Required Records
      ↓
Apply Filters
      ↓
Count or Aggregate
      ↓
Compare Results
      ↓
Find Patterns
      ↓
Generate Insights
```

---

# Key Difference to Remember

```text
COUNT(*)              → How many records?

COUNT(DISTINCT value) → How many unique values?

WHERE                 → Which records should be included?
```

Together, these concepts help transform large datasets into focused analytical answers.

---

# Interview Focus

Important topics to practice:

1. What does `COUNT(*)` do?
2. Difference between `COUNT(*)` and `COUNT(column)`
3. Difference between `COUNT(*)` and `COUNT(DISTINCT column)`
4. Does `COUNT(column)` include NULL values?
5. Difference between `IN` and `OR`
6. How does `BETWEEN` work?
7. How can COUNT be combined with WHERE?
8. How do you count unique customers?
9. When should `NOT IN` be used?
10. Why is filtering important in analytics?

---

# Key Takeaway

`COUNT()` measures records.

Filtering defines which records should be measured.

Together they help analysts answer questions such as:

```text
How many transactions?
How many customers?
How many unique customers?
How many Card payments?
How many orders fall within a range?
```

The objective is not just to write SQL syntax, but to use it to answer meaningful business questions.

---

## Topics Covered

`COUNT(*)`
`COUNT(1)`
`COUNT(column)`
`COUNT(DISTINCT)`
`WHERE`
`AND`
`OR`
`IN`
`NOT IN`
`BETWEEN`

---

## Learning Goal

Continue developing SQL skills through practical business use cases and move from:

```text
Raw Data
   ↓
Filtering
   ↓
Counting
   ↓
Analysis
   ↓
Business Insights
```

### Tags

`SQL` `Data Analytics` `Business Analytics` `COUNT` `Filtering` `WHERE` `IN` `BETWEEN` `Database`
