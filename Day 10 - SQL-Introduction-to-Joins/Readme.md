# Introduction to SQL JOINS

## Overview

SQL JOINS are used to combine data from multiple related tables using a common column.

They are one of the most important concepts in relational databases because real-world business data is often split across different tables.

For example:

- Customer details may be stored in one table
- Order details may be stored in another table
- Product information may be stored separately from sales data

JOINS help connect those tables to answer meaningful business questions.

---

## Why JOINS are Important

Without JOINS, analysis is limited to one table at a time.

With JOINS, we can answer questions such as:

- Which customers placed which orders?
- Which products contributed to revenue?
- Which stores generated sales?
- Which inventory items belong to which suppliers?

---

## Basic JOIN Idea

A JOIN combines data using a common column.

Example common column:

```text id="1tq1is"
customer_id
```

If both `customers` and `orders` contain `customer_id`, they can be joined.

---

# Types of JOINS

## 1. INNER JOIN

Returns only matching records from both tables.

```sql id="s6lp4d"
SELECT c.customer_name, o.order_id, o.amount
FROM customers c
INNER JOIN orders o
ON c.customer_id = o.customer_id;
```

### Use when:
Only matched data is needed.

### Example:
Show customers who placed orders.

---

## 2. LEFT JOIN

Returns all records from the left table and matching records from the right table.

```sql id="6rpvcs"
SELECT c.customer_name, o.order_id, o.amount
FROM customers c
LEFT JOIN orders o
ON c.customer_id = o.customer_id;
```

### Use when:
You want every record from the left table, even if no match exists.

### Example:
Show all customers, including those with no orders.

---

## 3. RIGHT JOIN

Returns all records from the right table and matching records from the left table.

```sql id="xe5pko"
SELECT c.customer_name, o.order_id, o.amount
FROM customers c
RIGHT JOIN orders o
ON c.customer_id = o.customer_id;
```

### Use when:
You want every record from the right table, even if there is no match in the left table.

### Example:
Show all orders, including orders not matched with customer details.

---

## 4. FULL JOIN

Returns all records when there is a match in either table.

```sql id="rlyr1d"
SELECT c.customer_name, o.order_id, o.amount
FROM customers c
FULL JOIN orders o
ON c.customer_id = o.customer_id;
```

### Use when:
You want matched and unmatched records from both tables.

### Example:
Show every customer and every order, whether matched or not.

---

# Easy Comparison

| JOIN Type | Result |
|---|---|
| INNER JOIN | Only matching rows |
| LEFT JOIN | All rows from left + matches from right |
| RIGHT JOIN | All rows from right + matches from left |
| FULL JOIN | All rows from both tables |

---

# Real-World Applications

## Customer + Transactions
Combine customer details with order or payment data.

## Product + Sales
Link product information with sales records.

## Store + Revenue
Connect store tables with revenue summaries.

## Inventory + Supplier
Match inventory items with supplier information.

---

# Example Tables

## Customers

| customer_id | customer_name |
|---|---|
| 101 | Alice |
| 102 | Bob |
| 103 | Carol |
| 104 | David |

## Orders

| customer_id | order_id | amount |
|---|---|---:|
| 101 | O-501 | 250 |
| 101 | O-502 | 320 |
| 103 | O-503 | 180 |
| 105 | O-504 | 500 |

---

# What Happens with Each JOIN?

## INNER JOIN
Returns:
- Alice with O-501
- Alice with O-502
- Carol with O-503

Only matching `customer_id` values are included.

## LEFT JOIN
Returns:
- Alice with her orders
- Bob with NULL
- Carol with O-503
- David with NULL

All customers are included.

## RIGHT JOIN
Returns:
- Alice with her orders
- Carol with O-503
- NULL with O-504

All orders are included.

## FULL JOIN
Returns:
- All customer rows
- All order rows
- Matched where possible
- NULL where unmatched

---

# Key Idea

A JOIN works best when you understand:

1. Which tables are related  
2. Which column connects them  
3. Which JOIN type fits the question  

---

# Interview Quick Recap

### What is a JOIN?
A SQL operation used to combine data from multiple tables using a common column.

### What is the difference between INNER JOIN and LEFT JOIN?
- INNER JOIN returns only matching rows
- LEFT JOIN returns all rows from the left table plus matching rows from the right

### Why are JOINS important?
Because real business data is stored across multiple related tables.

### What is required for a JOIN?
- Related tables
- A common column
- A join condition using `ON`

---

# Business Example

### Question
Show customer names with their order details.

```sql id="74p8lf"
SELECT c.customer_name, o.order_id, o.amount
FROM customers c
INNER JOIN orders o
ON c.customer_id = o.customer_id;
```

This turns separate customer and order data into one useful result.

---

# Key Takeaway

```text id="g83s0l"
Separate Tables
      ↓
Common Column
      ↓
JOIN
      ↓
Connected Data
      ↓
Analysis
      ↓
Business Insights
```

JOINS are foundational in SQL because they help analysts move from isolated datasets to connected analysis.

## Topics Covered

- What JOINS are
- Why JOINS matter
- INNER JOIN
- LEFT JOIN
- RIGHT JOIN
- FULL JOIN
- Real-world applications
- Interview recap

### Tags

`SQL` `JOINS` `INNER JOIN` `LEFT JOIN` `RIGHT JOIN` `FULL JOIN` `Data Analytics` `Business Analytics`
