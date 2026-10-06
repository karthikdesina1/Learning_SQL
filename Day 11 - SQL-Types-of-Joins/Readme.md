# Types of JOINS in SQL

## Overview

SQL JOINS combine data from related tables using a common column.

Different JOIN types return different sets of records, so choosing the right JOIN depends on the business question.

Example tables:

```text id="nbro0c"
Table A
Customers
1, 2, 3, 4

Table B
Orders / Customer IDs
3, 4, 5, 6
```

Common column:

```text id="hxrk3z"
customer_id
```

---

# 1. INNER JOIN

Returns only matching records from both tables.

```sql id="1nd6p0"
SELECT *
FROM customers c
INNER JOIN orders o
ON c.customer_id = o.customer_id;
```

Result:

```text id="9ukpnn"
3, 4
```

### Venn Diagram Idea

```text id="4qhgqy"
A ∩ B
```

Only the overlapping section is returned.

### Use Case

Find customers who have placed orders.

---

# 2. LEFT JOIN

Returns all rows from the left table plus matching rows from the right table.

```sql id="2f9kdb"
SELECT *
FROM customers c
LEFT JOIN orders o
ON c.customer_id = o.customer_id;
```

Result preserves:

```text id="od4ox3"
1, 2, 3, 4
```

Rows `1` and `2` have no matching order, so right-table columns become `NULL`.

### Venn Idea

Keep all of A.

### Use Case

Find all customers, including customers without orders.

---

# 3. RIGHT JOIN

Returns all rows from the right table and matching rows from the left table.

```sql id="s0oy8o"
SELECT *
FROM customers c
RIGHT JOIN orders o
ON c.customer_id = o.customer_id;
```

Result preserves:

```text id="xvsuiq"
3, 4, 5, 6
```

### Venn Idea

Keep all of B.

### Use Case

Find all orders, even if customer details are missing.

---

# 4. FULL OUTER JOIN

Returns all matched and unmatched rows from both tables.

```sql id="kvofp7"
SELECT *
FROM customers c
FULL OUTER JOIN orders o
ON c.customer_id = o.customer_id;
```

Result:

```text id="ucgmyv"
1, 2, 3, 4, 5, 6
```

### Venn Idea

```text id="ys6wsv"
A ∪ B
```

Everything from both tables is included.

### Use Case

Compare two datasets and identify matches and gaps.

---

# 5. CROSS JOIN

Returns every possible row combination between two tables.

```sql id="76lo63"
SELECT *
FROM customers
CROSS JOIN products;
```

If:

```text id="2mn4br"
Customers = 4 rows
Products  = 4 rows
```

Then:

```text id="zzf8au"
4 × 4 = 16 rows
```

### Important Note

CROSS JOIN can grow very quickly.

```text id="769q25"
1,000 × 1,000 = 1,000,000 rows
```

Use it only when every combination is actually required.

---

# 6. LEFT ANTI JOIN

Returns rows from the left table that do not have a match in the right table.

For the example:

```text id="qp00om"
1, 2
```

A common SQL pattern is:

```sql id="s9mhze"
SELECT c.*
FROM customers c
LEFT JOIN orders o
ON c.customer_id = o.customer_id
WHERE o.customer_id IS NULL;
```

### Use Case

Find customers who never placed an order.

---

# 7. RIGHT ANTI JOIN

Returns rows from the right table that do not have a match in the left table.

Example result:

```text id="lxttr3"
5, 6
```

One approach is:

```sql id="33ccvi"
SELECT o.*
FROM customers c
RIGHT JOIN orders o
ON c.customer_id = o.customer_id
WHERE c.customer_id IS NULL;
```

### Use Case

Find orders with no matching customer record.

---

# Quick Comparison

| JOIN | What It Returns |
|---|---|
| INNER | Matching rows only |
| LEFT | All left + matching right |
| RIGHT | All right + matching left |
| FULL OUTER | All rows from both |
| CROSS | Every possible combination |
| LEFT ANTI | Only unmatched left rows |
| RIGHT ANTI | Only unmatched right rows |

---

# Venn Diagram Recap

```text id="bk497h"
INNER      → overlap
LEFT       → all A
RIGHT      → all B
FULL       → A + B
LEFT ANTI  → A only
RIGHT ANTI → B only
```

CROSS JOIN is not naturally represented by a normal Venn diagram because it creates a Cartesian product rather than selecting overlapping sets.

---

# Real-World Applications

## Customer + Transactions
Identify customers with or without transactions.

## Products + Sales
Link product details with sales activity.

## Stores + Revenue
Combine store information with revenue data.

## Inventory + Suppliers
Match inventory items with suppliers.

## Data Quality Checks
ANTI JOIN logic is especially useful for identifying missing relationships.

---

# Must-Remember Rules

### 1. Know which table is LEFT and RIGHT

The result of LEFT and RIGHT JOIN depends on table position.

### 2. Understand table relationships

Joining on the wrong key can create incorrect results.

### 3. Watch for duplicate multiplication

One-to-many and many-to-many relationships can increase row counts.

### 4. NULL values matter

Unmatched rows in outer joins often contain `NULL`.

### 5. CROSS JOIN can become huge

Always estimate expected row count before using it.

---

# Interview Quick Recap

### INNER vs LEFT JOIN?

```text id="qbi4m8"
INNER → matching rows only
LEFT  → all left rows + matches
```

### LEFT JOIN vs LEFT ANTI JOIN?

```text id="3xpzmr"
LEFT JOIN      → matched + unmatched left rows
LEFT ANTI JOIN → unmatched left rows only
```

### FULL JOIN vs CROSS JOIN?

```text id="jthztr"
FULL  → related matched/unmatched rows
CROSS → every possible row combination
```

### Why use an ANTI JOIN?

To find records that are missing a relationship in another table.

---

# Key Takeaway

```text id="d3qf6x"
Business Question
       ↓
Choose JOIN
       ↓
Combine Related Data
       ↓
Validate Matches
       ↓
Analyze Results
       ↓
Business Insights
```

Different JOIN types answer different business questions.

## Topics Covered

`INNER JOIN`  
`LEFT JOIN`  
`RIGHT JOIN`  
`FULL OUTER JOIN`  
`CROSS JOIN`  
`LEFT ANTI JOIN`  
`RIGHT ANTI JOIN`  
`Venn Diagrams`

### Tags

`SQL` `SQL JOINS` `Data Analytics` `Business Analytics` `Relational Database` `SQL Interview`
