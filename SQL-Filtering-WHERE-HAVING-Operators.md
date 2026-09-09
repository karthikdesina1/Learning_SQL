# SQL Learning Journey — Filtering, WHERE, HAVING & Operators

## Overview

This section of my SQL learning journey focuses on **filtering data efficiently** using SQL conditions and operators.

Filtering is one of the most important concepts in SQL because real-world datasets often contain thousands or millions of rows, while analysts usually need only a specific portion of that data to answer a business question.

The main concepts covered are:

* Filtering
* Types of Filtering
* WHERE Clause
* HAVING Clause
* WHERE vs HAVING
* Comparison Operators
* Special Operators
* Logical Operators
* Pattern Matching
* NULL Handling

---

## 1. What is Filtering?

Filtering means retrieving only the records that satisfy a specific condition.

Example:

```sql
SELECT *
FROM customers
WHERE city = 'New York';
```

This returns only customers located in New York.

### Why Filtering Matters

Instead of analyzing an entire dataset, filtering helps answer focused questions such as:

* Which customers are from New York?
* Which orders are above $1,000?
* Which products belong to a specific category?
* Which employees joined after a certain date?
* Which records contain missing values?

---

# 2. Types of Filtering

## Basic Filtering

Used for simple conditions.

SELECT *
FROM customers
WHERE country = 'USA';

## Numeric Filtering

Used for numerical comparisons.

SELECT *
FROM sales
WHERE revenue > 5000;

## Date Filtering

Used to retrieve records within a date condition.

SELECT *
FROM orders
WHERE order_date >= '2026-01-01';

## Multi-Condition Filtering

Used when multiple conditions are required.

SELECT *
FROM sales
WHERE region = 'East'
AND revenue > 10000;

## Pattern Match Filtering

Used to search text based on a pattern.

SELECT *
FROM customers
WHERE customer_name LIKE 'M%';
---

# 3. WHERE Clause

The `WHERE` clause filters individual rows before grouping or aggregation.

Example:

SELECT *
FROM employees
WHERE salary > 60000;

This retrieves employees whose salary is greater than 60,000.

---

# 4. HAVING Clause

The `HAVING` clause filters grouped or aggregated data after `GROUP BY`.

Example:
SELECT region, SUM(sales) AS total_sales
FROM orders
GROUP BY region
HAVING SUM(sales) > 50000;

This returns only regions whose total sales exceed 50,000.

---

# 5. WHERE vs HAVING

| WHERE                                | HAVING                                 |
| ------------------------------------ | -------------------------------------- |
| Filters individual rows              | Filters grouped data                   |
| Applied before GROUP BY              | Applied after GROUP BY                 |
| Commonly used with normal conditions | Commonly used with aggregate functions |

### Easy Way to Remember
WHERE  → Rows
HAVING → Groups
---

# 6. Comparison Operators

SQL provides several comparison operators.

| Operator     | Meaning               |
| ------------ | --------------------- |
| `=`          | Equal                 |
| `!=` or `<>` | Not Equal             |
| `>`          | Greater Than          |
| `<`          | Less Than             |
| `>=`         | Greater Than or Equal |
| `<=`         | Less Than or Equal    |

Example:

SELECT *
FROM products
WHERE price >= 100;
---

# 7. Special Operators

## BETWEEN

Used for range filtering.

SELECT *
FROM employees
WHERE salary BETWEEN 50000 AND 80000;
---

## IN

Used to match multiple values.

SELECT *
FROM customers
WHERE city IN ('New York', 'Chicago', 'Boston');
---

## NOT IN

Used to exclude multiple values.

SELECT *
FROM customers
WHERE city NOT IN ('Miami', 'Dallas');
---

## LIKE

Used for pattern matching.

Examples:

'M%'   → Starts with M
'%M'   → Ends with M
'%M%'  → Contains M
'M_'   → Starts with M followed by one character

Example:
SELECT *
FROM customers
WHERE customer_name LIKE 'M%';
---

# 8. NULL Filtering

NULL represents missing or unknown data.

## IS NULL

SELECT *
FROM customers
WHERE phone_number IS NULL;

## IS NOT NULL

SELECT *
FROM customers
WHERE email IS NOT NULL;

Using `= NULL` is not the correct way to test for NULL values.
---

# 9. Logical Operators
## AND

All conditions must be true.

SELECT *
FROM customers
WHERE city = 'New York'
AND total_spend > 5000;

## OR

At least one condition must be true.

SELECT *
FROM customers
WHERE city = 'New York'
OR city = 'New Jersey';

## NOT

Returns the opposite of a condition.

SELECT *
FROM products
WHERE NOT category = 'Electronics';
---
# 10. Real-World Business Example

#Business Question

Find customers from New York or New Jersey who spent more than $5,000 and have a valid email address.

SELECT *
FROM customers
WHERE state IN ('New York', 'New Jersey')
AND total_spend > 5000
AND email IS NOT NULL;

This query combines:

* Multiple-value filtering
* Numeric filtering
* Logical operators
* NULL handling

This is how SQL filtering becomes useful for real business analysis.

---

# Filtering in an Analytics Workflow

Business Question
       ↓
Identify Conditions
       ↓
Apply WHERE / HAVING
       ↓
Use Operators
       ↓
Retrieve Relevant Records
       ↓
Analyze Results
       ↓
Generate Insights
       ↓
Support Decisions

# Interview Focus Topics

Important concepts to practice:

1. What is SQL filtering?
2. Difference between WHERE and HAVING
3. Difference between AND and OR
4. How does BETWEEN work?
5. Difference between IN and NOT IN
6. How does LIKE work?
7. Difference between `%M%` and `M%`
8. How do you find NULL values?
9. Can HAVING be used with aggregate functions?
10. Why is filtering important in analytics?

---

# Key Takeaway

Filtering is one of the foundations of SQL analysis.

The main objective is not just to memorize operators, but to understand how to translate a business question into conditions that retrieve the correct data.

Raw Data
   ↓
Filtering
   ↓
Relevant Records
   ↓
Analysis
   ↓
Insights
   ↓
Business Decisions

## Learning Goal

I am continuing to strengthen my SQL skills step by step with a focus on:

* SQL Fundamentals
* Practical Queries
* Business Use Cases
* Interview Preparation
* Data Analytics
* Business Analytics

### Topics
`SQL` `WHERE` `HAVING` `Filtering` `Operators` `LIKE` `BETWEEN` `IN` `NULL` `DataAnalytics` `BusinessAnalytics`
