# SQL HAVING Clause

## Overview

This learning section focuses on the `HAVING` clause in SQL.

`HAVING` is used to filter **grouped and aggregated results** after a `GROUP BY` operation.

It is especially useful in analytics when we want to keep only the groups that satisfy a business condition.

---

## What is HAVING?

The `HAVING` clause filters grouped data after aggregation.

Example:

```sql
SELECT category,
       SUM(sales) AS total_sales
FROM orders
GROUP BY category
HAVING SUM(sales) > 50000;
```

This query returns only categories whose total sales are greater than 50,000.

---

## WHERE vs HAVING

| WHERE                   | HAVING                         |
| ----------------------- | ------------------------------ |
| Filters individual rows | Filters grouped results        |
| Applied before GROUP BY | Applied after GROUP BY         |
| Works on raw records    | Commonly works with aggregates |

### Easy Rule

```text
WHERE  → rows
HAVING → groups
```

---

## SQL Execution Order

A useful logical execution order is:

```text
FROM
  ↓
WHERE
  ↓
GROUP BY
  ↓
HAVING
  ↓
SELECT
  ↓
ORDER BY
```

Understanding this order helps explain why `WHERE` cannot usually be used to filter aggregate results.

---

## Using HAVING with Aggregate Functions

### SUM()

```sql
SELECT region,
       SUM(sales) AS total_sales
FROM orders
GROUP BY region
HAVING SUM(sales) > 100000;
```

Finds regions with total sales above 100,000.

---

### COUNT()

```sql
SELECT store_id,
       COUNT(*) AS transaction_count
FROM transactions
GROUP BY store_id
HAVING COUNT(*) > 1000;
```

Finds stores with more than 1,000 transactions.

---

### AVG()

```sql
SELECT customer_segment,
       AVG(order_amount) AS avg_order_value
FROM orders
GROUP BY customer_segment
HAVING AVG(order_amount) > 500;
```

Finds customer segments with an average order value above 500.

---

### MIN()

```sql
SELECT category,
       MIN(price) AS lowest_price
FROM products
GROUP BY category
HAVING MIN(price) > 20;
```

---

### MAX()

```sql
SELECT region,
       MAX(sales) AS highest_sale
FROM orders
GROUP BY region
HAVING MAX(sales) > 10000;
```

---

## Important Rules

### Rule 1

`HAVING` is normally used with grouped or aggregated data.

### Rule 2

`WHERE` filters records before grouping.

### Rule 3

`HAVING` filters the results after groups have been created.

### Rule 4

Aggregate functions such as `SUM()`, `COUNT()`, `AVG()`, `MIN()`, and `MAX()` are commonly used inside `HAVING`.

### Rule 5

Use `WHERE` whenever possible to remove unnecessary rows before aggregation.

This can make the query easier to understand and often more efficient.

---

## WHERE + GROUP BY + HAVING

These clauses are often used together.

```sql
SELECT category,
       SUM(sales) AS total_sales
FROM orders
WHERE order_date >= '2026-01-01'
GROUP BY category
HAVING SUM(sales) > 50000
ORDER BY total_sales DESC;
```

### Query Flow

```text
Orders
  ↓
WHERE
Filter 2026 records
  ↓
GROUP BY
Create category groups
  ↓
SUM()
Calculate category sales
  ↓
HAVING
Keep groups above 50,000
  ↓
ORDER BY
Sort final results
```

---

## Real-World Business Applications

The `HAVING` clause can answer questions such as:

* Which product categories have high total sales?
* Which stores generate significant revenue?
* Which regions process large transaction volumes?
* Which customer segments have high average spending?
* Which departments have more than a certain number of employees?
* Which products exceed a target number of orders?

---

## Example: High-Revenue Stores

### Business Question

Find stores whose total revenue exceeds $250,000.

```sql
SELECT store_id,
       SUM(revenue) AS total_revenue
FROM sales
GROUP BY store_id
HAVING SUM(revenue) > 250000;
```

This converts raw sales records into store-level insights.

---

## Example: Active Customers

Find customers who placed more than five orders.

```sql
SELECT customer_id,
       COUNT(*) AS total_orders
FROM orders
GROUP BY customer_id
HAVING COUNT(*) > 5;
```

This could help identify repeat or highly active customers.

---

## Example: Combining WHERE and HAVING

### Business Question

Find regions with more than 100 completed orders in 2026.

```sql
SELECT region,
       COUNT(*) AS completed_orders
FROM orders
WHERE order_date >= '2026-01-01'
AND status = 'Completed'
GROUP BY region
HAVING COUNT(*) > 100;
```

`WHERE` removes irrelevant records first.

`HAVING` evaluates the grouped result afterward.

---

## Why HAVING Matters in Analytics

Without `HAVING`, we can summarize data.

With `HAVING`, we can filter those summaries and focus only on important groups.

```text
Raw Data
   ↓
Filtering
   ↓
Grouping
   ↓
Aggregation
   ↓
HAVING
   ↓
Relevant Business Groups
   ↓
Insights
```

This makes `HAVING` especially valuable in reporting, KPI analysis, customer segmentation, sales analysis, and operational analytics.

---

## Interview Focus

Important questions to practice:

1. What is the HAVING clause?
2. What is the difference between WHERE and HAVING?
3. Can HAVING be used with aggregate functions?
4. Why is HAVING used after GROUP BY?
5. Can WHERE filter aggregate results?
6. How does SQL execution order affect HAVING?
7. Difference between `WHERE COUNT(*) > 5` and `HAVING COUNT(*) > 5`
8. When should WHERE and HAVING be used together?

---

## Key Takeaway

```text
WHERE  → Filter rows before grouping

GROUP BY → Create groups

HAVING → Filter groups after aggregation
```

Understanding the timing of each clause makes analytical SQL much easier to write.

### Topics Covered

`HAVING`
`WHERE`
`GROUP BY`
`SUM()`
`COUNT()`
`AVG()`
`MIN()`
`MAX()`
`SQL Execution Order`

### Tags

`SQL` `HAVING` `GROUP BY` `Data Analytics` `Business Analytics` `SQL Learning` `Database`
