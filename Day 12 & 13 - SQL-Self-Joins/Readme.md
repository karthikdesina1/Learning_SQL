# SQL Self Joins

## Overview

A **Self Join** is used when a table needs to be joined with itself.

It is especially useful when relationships exist between rows inside the same dataset.

Common examples include:

- Employee → Manager
- Product → Category
- Parent → Child
- Customer combinations
- Row-to-row comparisons
- Organizational hierarchies

A Self Join does not use a special `SELF JOIN` keyword.

Instead, the same table is referenced twice using different aliases.

---

## Basic Syntax

```sql
SELECT
    a.column1,
    b.column2
FROM table_name a
JOIN table_name b
ON a.key = b.related_key;
```

The aliases `a` and `b` allow SQL to distinguish between the two references.

---

# 1. Employee → Manager Example

Consider an employee table:

| emp_id | emp_name | manager_id |
|---|---|---|
| 1 | Asha | NULL |
| 2 | Ben | 1 |
| 3 | Carol | 1 |
| 4 | David | 2 |

Here:

- Asha has no manager
- Ben reports to Asha
- Carol reports to Asha
- David reports to Ben

Query:

```sql
SELECT
    e.emp_name AS employee,
    m.emp_name AS manager
FROM employee e
LEFT JOIN employee m
ON e.manager_id = m.emp_id;
```

Result:

| employee | manager |
|---|---|
| Asha | NULL |
| Ben | Asha |
| Carol | Asha |
| David | Ben |

---

# 2. Why LEFT JOIN is Often Used

If the top-level manager has `manager_id = NULL`, using an `INNER JOIN` would remove that row.

A `LEFT JOIN` keeps every employee:

```sql
FROM employee e
LEFT JOIN employee m
ON e.manager_id = m.emp_id;
```

This is useful when the complete hierarchy must be preserved.

---

# 3. Advanced Self Join: Products & Categories

A single products table can store both categories and products.

Example:

| product_id | product_name | parent_id |
|---|---|---|
| 10 | Electronics | NULL |
| 11 | Mobile | 10 |
| 12 | Laptop | 10 |
| 20 | Furniture | NULL |
| 21 | Chair | 20 |

Interpretation:

```text
parent_id = NULL
→ top-level category

parent_id has value
→ belongs to another row/category
```

Query:

```sql
SELECT
    p.product_name,
    c.product_name AS category_name
FROM products p
LEFT JOIN products c
ON p.parent_id = c.product_id;
```

Possible result:

| product_name | category_name |
|---|---|
| Mobile | Electronics |
| Laptop | Electronics |
| Chair | Furniture |

---

# 4. Category-Wise Analysis

After mapping products to categories, aggregation can be added.

```sql
SELECT
    c.product_name AS category_name,
    COUNT(p.product_id) AS product_count
FROM products p
JOIN products c
ON p.parent_id = c.product_id
GROUP BY c.product_name;
```

This helps answer:

- How many products belong to each category?
- Which category contains the most products?
- How is inventory distributed by category?

---

# 5. Customer Combination Example

Self Joins can generate unique pairs.

Example customer table:

```text
Alice
Bob
Carol
```

Query:

```sql
SELECT
    c1.cname AS customer1,
    c2.cname AS customer2
FROM customers c1
JOIN customers c2
ON c1.cname < c2.cname;
```

Result:

```text
Alice | Bob
Alice | Carol
Bob   | Carol
```

---

## Why Use `<`?

If we use:

```sql
c1.cname != c2.cname
```

we may get:

```text
Alice | Bob
Bob   | Alice
```

Both represent the same pair.

Using:

```sql
c1.cname < c2.cname
```

helps ensure:

- No self-pairs
- No reverse duplicates
- Each combination appears once

For production systems, comparing stable unique IDs is usually safer than names.

---

# 6. Common Self Join Use Cases

## Employee Hierarchy

```text
Employee → Manager
```

## Product Hierarchy

```text
Product → Category
```

## Parent-Child Relationships

```text
Child Row → Parent Row
```

## Row Comparison

Compare records inside the same dataset.

## Unique Combinations

Generate combinations without duplicate pairs.

---

# Must-Remember Rules

### 1. Aliases are essential

```sql
employee e
employee m
```

Without aliases, column references become ambiguous.

### 2. Understand the relationship

Identify which column points to another row in the same table.

Examples:

```text
manager_id → emp_id
parent_id  → product_id
```

### 3. Choose the JOIN type carefully

Use `LEFT JOIN` when unmatched parent/top-level rows must remain.

### 4. Watch for duplicate combinations

When generating pairs, use conditions carefully to prevent reversed duplicates.

### 5. Hierarchical depth matters

A basic Self Join typically handles one relationship level.

Multi-level hierarchies may require:

- Multiple Self Joins
- Recursive CTEs

---

# Interview Quick Recap

### What is a Self Join?

A table joined with itself using different aliases.

### Is SELF JOIN a SQL keyword?

No.

It uses regular JOIN syntax.

### Why are aliases required?

They distinguish each instance of the same table.

### Common Self Join example?

Employee-manager relationship.

### Self Join vs regular JOIN?

```text
Regular JOIN
→ different tables

Self Join
→ same table referenced multiple times
```

### When can Self Join become complex?

When hierarchical data contains multiple levels.

Recursive CTEs may be more suitable for deep hierarchies.

---

# Key Takeaway

```text
Single Table
    ↓
Different Aliases
    ↓
Compare Related Rows
    ↓
Understand Relationships
    ↓
Business Insights
```

Self Joins are powerful because a single table can contain relationships that become visible only when its rows are compared with one another.

## Topics Covered

`Self Join`  
`Table Aliases`  
`Employee Manager`  
`Products Categories`  
`Hierarchical Data`  
`Unique Combinations`  
`Row Comparison`

### Tags

`SQL` `Self Join` `SQL Joins` `Data Analytics` `Business Analytics` `Database` `SQL Interview`
