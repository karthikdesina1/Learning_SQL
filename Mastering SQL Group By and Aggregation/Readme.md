# Mastering GROUP BY & Aggregation in SQL

## Overview

This learning section focuses on one of the most important concepts in SQL:

**GROUP BY and Aggregation**

These concepts help transform detailed row-level data into meaningful summaries that can answer real business questions.

Instead of analyzing every record individually, SQL allows us to group related records and calculate totals, averages, counts, minimums, and maximums.

---

## What is Aggregation?

Aggregation means combining multiple rows and producing a summarized value.

Common examples include:

```sql
COUNT()
SUM()
AVG()
MAX()
MIN()
```

### Example

```sql
SELECT SUM(sales) AS total_sales
FROM orders;
```

This returns the total sales across all records.

---

## Common Aggregate Functions

### COUNT()

Returns the number of records.

```sql
SELECT COUNT(*)
FROM customers;
```

---

### SUM()

Calculates the total.

```sql
SELECT SUM(sales)
FROM orders;
```

---

### AVG()

Calculates the average.

```sql
SELECT AVG(salary)
FROM employees;
```

---

### MAX()

Returns the highest value.

```sql
SELECT MAX(order_amount)
FROM orders;
```

---

### MIN()

Returns the lowest value.

```sql
SELECT MIN(order_amount)
FROM orders;
```

---

# What is GROUP BY?

`GROUP BY` divides rows into categories and allows aggregate functions to be applied to each category.

Example:

```sql
SELECT region,
       SUM(sales) AS total_sales
FROM orders
GROUP BY region;
```

This calculates total sales for each region.

---

## Real-World Example

Imagine this data:

| Region | Sales |
| ------ | ----: |
| North  |  1000 |
| South  |   700 |
| North  |   450 |
| East   |   620 |

Using:

```sql
SELECT region,
       SUM(sales) AS total_sales
FROM orders
GROUP BY region;
```

The result becomes:

| Region | Total Sales |
| ------ | ----------: |
| North  |        1450 |
| South  |         700 |
| East   |         620 |

This makes category-level analysis much easier.

---

# Must-Follow GROUP BY Rules

## Rule 1

Every non-aggregated column selected should generally appear in the `GROUP BY` clause.

Correct:

```sql
SELECT department,
       AVG(salary)
FROM employees
GROUP BY department;
```

---

## Rule 2

Aggregate functions summarize the rows inside each group.

```sql
SELECT category,
       COUNT(*) AS total_products
FROM products
GROUP BY category;
```

---

## Rule 3

Use `WHERE` to filter rows before grouping.

```sql
SELECT region,
       SUM(sales)
FROM orders
WHERE order_date >= '2026-01-01'
GROUP BY region;
```

---

## Rule 4

Use `HAVING` to filter aggregated groups.

```sql
SELECT region,
       SUM(sales) AS total_sales
FROM orders
GROUP BY region
HAVING SUM(sales) > 50000;
```

---

# WHERE vs HAVING

| WHERE                       | HAVING                                |
| --------------------------- | ------------------------------------- |
| Filters rows                | Filters groups                        |
| Applied before GROUP BY     | Applied after aggregation             |
| Works on individual records | Commonly works with aggregate results |

### Easy Flow

```text
Raw Data
   ↓
WHERE
   ↓
GROUP BY
   ↓
Aggregation
   ↓
HAVING
   ↓
Final Result
```

---

# When Should WHERE Be Used With GROUP BY?

Use `WHERE` when some records should be removed before the grouping and aggregation happen.

Example business question:

> Find total sales by region only for orders placed in 2026.

```sql
SELECT region,
       SUM(sales) AS total_sales
FROM orders
WHERE order_date >= '2026-01-01'
GROUP BY region;
```

The filtering happens first, then the remaining rows are grouped.

---

# Multiple Aggregations Together

We can use multiple aggregate functions in one query.

```sql
SELECT department,
       COUNT(*) AS employee_count,
       AVG(salary) AS average_salary,
       MAX(salary) AS highest_salary,
       MIN(salary) AS lowest_salary
FROM employees
GROUP BY department;
```

This can answer several business questions in one query.

---

# Good Practices

When working with `GROUP BY` and aggregation:

* Use meaningful aliases
* Group only by necessary columns
* Check for NULL values
* Understand the business question first
* Choose the correct aggregate function
* Validate grouped results
* Avoid grouping by unnecessary fields

---

# Business Use Cases

GROUP BY and aggregation are useful for questions such as:

* Total revenue by region
* Average salary by department
* Number of customers by country
* Total orders by month
* Highest sales by product category
* Lowest price by supplier
* Customer count by segment

---

# Example Analytics Query

### Business Question

What are total and average sales by region for orders placed in 2026?

```sql
SELECT region,
       SUM(sales) AS total_sales,
       AVG(sales) AS average_sales
FROM orders
WHERE order_date >= '2026-01-01'
GROUP BY region;
```

This query combines:

* Filtering
* Grouping
* Aggregation
* Business analysis

---

# Key Takeaway

`GROUP BY` helps divide data into meaningful categories.

Aggregate functions summarize the data inside those categories.

Together they help transform:

```text
Raw Rows
   ↓
Grouped Categories
   ↓
Aggregation
   ↓
Metrics
   ↓
Business Insights
```

This is one of the core concepts required for real-world SQL analysis.

---

## Learning Focus

Current concepts practiced:

* GROUP BY
* COUNT()
* SUM()
* AVG()
* MAX()
* MIN()
* WHERE with GROUP BY
* HAVING
* Aggregation rules
* Business use cases

### Topics

`SQL` `GROUP BY` `Aggregation` `COUNT` `SUM` `AVG` `MAX` `MIN` `WHERE` `HAVING` `DataAnalytics`
