# SQL UNION & UNION ALL

## Overview

`UNION` and `UNION ALL` are SQL set operations used to combine the results of multiple `SELECT` queries into one result set.

The main difference is duplicate handling:

```text
UNION     → Removes duplicate rows
UNION ALL → Keeps duplicate rows
```

These operations are commonly used when similar data is stored across multiple tables, branches, regions, systems, or time periods.

---

## 1. UNION

`UNION` combines multiple query results and removes duplicate rows.

```sql
SELECT customer_id, name, city
FROM store_a

UNION

SELECT customer_id, name, city
FROM store_b;
```

Example:

### Store A

| ID | Name |
|---|---|
| 1 | Alice |
| 2 | Bob |
| 3 | Carol |

### Store B

| ID | Name |
|---|---|
| 3 | Carol |
| 4 | David |

Result with `UNION`:

| ID | Name |
|---|---|
| 1 | Alice |
| 2 | Bob |
| 3 | Carol |
| 4 | David |

The duplicate Carol row is removed.

---

## 2. UNION ALL

`UNION ALL` combines the results but keeps duplicates.

```sql
SELECT customer_id, name, city
FROM store_a

UNION ALL

SELECT customer_id, name, city
FROM store_b;
```

Result:

```text
Alice
Bob
Carol
Carol
David
```

Both Carol records remain.

---

# UNION vs UNION ALL

| Feature | UNION | UNION ALL |
|---|---|---|
| Combines result sets | Yes | Yes |
| Removes duplicates | Yes | No |
| Keeps all rows | No | Yes |
| Performance | Usually slower | Usually faster |
| Best use | Unique combined results | Complete combined results |

---

## 3. Performance Difference

`UNION` usually requires extra work to detect and remove duplicate rows.

Conceptually:

```text
Query 1
   +
Query 2
   ↓
Combine
   ↓
Check duplicates
   ↓
Remove duplicates
```

`UNION ALL` skips duplicate checking:

```text
Query 1
   +
Query 2
   ↓
Combine all rows
```

Therefore, if duplicate removal is unnecessary, `UNION ALL` is often more efficient.

---

# 4. Important Rules

## Rule 1 - Same Number of Columns

Both queries must return the same number of columns.

Correct:

```sql
SELECT customer_id, name
FROM store_a

UNION

SELECT customer_id, name
FROM store_b;
```

Incorrect:

```sql
SELECT customer_id, name, city
FROM store_a

UNION

SELECT customer_id, name
FROM store_b;
```

---

## Rule 2 - Compatible Data Types

Corresponding columns should contain compatible data types.

Example:

```text
customer_id ↔ customer_id
name        ↔ name
city        ↔ city
```

---

## Rule 3 - Same Logical Column Order

Column position matters.

```sql
SELECT customer_id, name, city
FROM store_a

UNION

SELECT customer_id, name, city
FROM store_b;
```

The first column aligns with the first column, second with second, and so on.

---

## Rule 4 - Final Column Names

The final result generally uses column names or aliases from the first `SELECT`.

```sql
SELECT customer_id AS id,
       name AS customer_name
FROM store_a

UNION

SELECT customer_id,
       name
FROM store_b;
```

Final columns:

```text
id
customer_name
```

---

# 5. UNION vs JOIN

This is an important interview distinction.

## UNION

Combines rows vertically.

```text
Store A
  ↓
Store B
  ↓
More Rows
```

Example:

```sql
SELECT customer_id, name
FROM store_a

UNION ALL

SELECT customer_id, name
FROM store_b;
```

---

## JOIN

Combines columns horizontally using a relationship.

```text
Customers + Orders
        ↓
More Columns
```

Example:

```sql
SELECT
    customers.customer_id,
    customers.name,
    orders.order_amount
FROM customers
JOIN orders
ON customers.customer_id = orders.customer_id;
```

### Easy Reminder

```text
UNION → Adds rows
JOIN  → Adds columns
```

---

# 6. Real-World Applications

## Combine Regional Sales

```sql
SELECT order_id, amount
FROM east_sales

UNION ALL

SELECT order_id, amount
FROM west_sales;
```

---

## Combine Current and Archived Data

```sql
SELECT customer_id, order_date
FROM current_orders

UNION ALL

SELECT customer_id, order_date
FROM archived_orders;
```

---

## Build a Unique Master List

```sql
SELECT email
FROM customers_us

UNION

SELECT email
FROM customers_canada;
```

This removes duplicate email addresses across both datasets.

---

# 7. When to Use UNION

Use `UNION` when:

- Duplicate rows should be removed
- A clean unique result is required
- The business question specifically requires deduplication

Example:

> Find every unique city appearing across two customer tables.

---

# 8. When to Use UNION ALL

Use `UNION ALL` when:

- Every row matters
- Duplicate records are valid
- Maximum performance is preferred
- You know the datasets do not overlap

Example:

> Combine monthly transaction tables into one reporting dataset.

---

# Must-Remember Notes

### 1. UNION does not mean “join tables”

It stacks compatible result sets.

### 2. Column names do not need to match

But the number, position, and compatible data types must align.

### 3. UNION checks the entire row for duplicates

Duplicate handling is based on the complete selected row.

### 4. Prefer UNION ALL when deduplication is unnecessary

Avoid paying the performance cost of duplicate removal without a business reason.

### 5. Verify whether duplicates are valid before removing them

Sometimes duplicate-looking rows represent real transactions.

---

# Quick Recap for Interview POV:

### What is UNION?

A SQL set operation that combines result sets and removes duplicates.

### What is UNION ALL?

Combines result sets and keeps duplicates.

### Which is faster?

Generally `UNION ALL`.

### Why is UNION slower?

Because duplicate elimination requires additional processing.

### Can UNION queries have different numbers of columns?

No.

### UNION vs JOIN?

```text
UNION → combines rows
JOIN  → combines columns
```

### When should UNION ALL be preferred?

When duplicate removal is not required.

---

# Key Takeaway

```text
Multiple SELECT Queries
         ↓
   UNION / UNION ALL
         ↓
Combined Result Set
         ↓
Analysis
         ↓
Business Insights
```

The right choice depends on one key question:

> Should duplicate rows remain in the final result?

If **No** → `UNION`  
If **Yes** → `UNION ALL`

## Topics Covered

`UNION`  
`UNION ALL`  
`Duplicate Handling`  
`Performance`  
`Set Operations`  
`JOIN Comparison`  
`SQL Interview`

### Tags

`SQL` `UNION` `UNION ALL` `Data Analytics` `Business Analytics` `Database` `SQL Interview`
