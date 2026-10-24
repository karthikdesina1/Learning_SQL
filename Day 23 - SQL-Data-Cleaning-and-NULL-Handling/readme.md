# Data Cleaning & Handling NULL Values in SQL

## Overview

NULL handling is one of the most practical parts of SQL data cleaning.

`NULL` represents missing or unknown information.

It is important to handle NULL values correctly because they can affect:

- Calculations
- Aggregations
- Reports
- Joins
- Filters
- Data quality

---

# What is NULL?

NULL means:

```text
Missing value
Unknown value
Unavailable value
```

NULL is not:

```text
0
''
False
```

Example:

```sql
SELECT salary + NULL
FROM employees;
```

The result is generally:

```text
NULL
```

because arithmetic involving NULL usually returns NULL.

---

# 1. IS NULL / IS NOT NULL

Never use:

```sql
WHERE email = NULL;
```

Use:

```sql
SELECT *
FROM customers
WHERE email IS NULL;
```

To find available values:

```sql
SELECT *
FROM customers
WHERE email IS NOT NULL;
```

---

# 2. NVL()

`NVL()` is commonly used in Oracle SQL.

Syntax:

```sql
NVL(expression, replacement)
```

Example:

```sql
SELECT
    employee_name,
    NVL(salary, 0) AS salary
FROM employees;
```

If salary is NULL:

```text
NULL → 0
```

### Use Case

Prevent missing numeric values from affecting reporting.

---

# 3. NVL2()

`NVL2()` returns one value when an expression is not NULL and another when it is NULL.

Syntax:

```sql
NVL2(expression, value_if_not_null, value_if_null)
```

Example:

```sql
SELECT
    customer_name,
    NVL2(phone, 'Available', 'Missing') AS phone_status
FROM customers;
```

Logic:

```text
Phone exists
→ Available

Phone is NULL
→ Missing
```

---

# 4. COALESCE()

`COALESCE()` returns the first non-NULL value.

```sql
SELECT
    customer_id,
    COALESCE(email, phone, 'No Contact') AS contact_info
FROM customers;
```

Logic:

```text
Check email
   ↓
If NULL → check phone
   ↓
If NULL → 'No Contact'
```

### Why It Is Useful

It can check multiple fallback columns, unlike `NVL()`, which handles two expressions.

---

# 5. NULLIF()

`NULLIF(a, b)` returns NULL when two values are equal.

```sql
SELECT
    NULLIF(discount, 0)
FROM sales;
```

Example:

```text
discount = 0
→ NULL

discount = 10
→ 10
```

---

# Practical Example — Customer Contact Cleaning

```sql
SELECT
    customer_id,
    customer_name,
    COALESCE(email, phone, 'No Contact') AS contact_info
FROM customers;
```

This ensures the report displays the first available contact method.

---

# Practical Example — Revenue Cleaning

Suppose some revenue values are NULL.

```sql
SELECT
    product_id,
    NVL(revenue, 0) AS cleaned_revenue
FROM sales;
```

This can prevent missing values from affecting downstream calculations.

---

# NULL and Aggregate Functions

Most aggregate functions ignore NULL values.

Example:

```sql
SELECT
    COUNT(*),
    COUNT(phone)
FROM customers;
```

If there are 100 customers but only 80 phone numbers:

```text
COUNT(*)     = 100
COUNT(phone) = 80
```

This is useful for data-quality checks.

---

# NULL and AVG()

Example:

```sql
SELECT AVG(salary)
FROM employees;
```

If some salaries are NULL, they are generally excluded from the average.

That may be correct or incorrect depending on the business requirement.

If NULL should mean zero:

```sql
SELECT AVG(NVL(salary, 0))
FROM employees;
```

These two calculations can produce very different results.

---

# NVL vs COALESCE

| Function | Purpose |
|---|---|
| `NVL()` | Replace NULL with one alternative |
| `COALESCE()` | Return first non-NULL from multiple values |

Example:

```sql
NVL(email, 'Missing')
```

vs:

```sql
COALESCE(email, phone, address, 'No Contact')
```

---

# NVL2 vs NVL

```text
NVL
→ Replace NULL

NVL2
→ Return one result if NOT NULL
→ Return another result if NULL
```

---

# NULLIF Use Case - Avoid Division Problems

Example:

```sql
SELECT
    revenue / NULLIF(quantity, 0) AS revenue_per_unit
FROM sales;
```

If `quantity = 0`, `NULLIF(quantity, 0)` becomes NULL instead of zero.

This can help avoid division-by-zero errors in many SQL systems.

---

# Real-World Applications

## Customer Data

- Missing phone numbers
- Missing emails
- Incomplete addresses

## Financial Data

- Missing salary
- Missing revenue
- Missing discounts

## Reporting

- Replace blanks with meaningful labels
- Prevent misleading calculations
- Improve dashboard readability

---

# Must-Remember Notes

### 1. Never use `= NULL`

Use:

```sql
IS NULL
IS NOT NULL
```

### 2. NULL is not zero

Do not replace NULL with `0` unless the business meaning supports it.

### 3. Aggregates often ignore NULL

Examples:

```text
SUM()
AVG()
COUNT(column)
```

### 4. COUNT(*) is different

`COUNT(*)` counts rows, including rows that contain NULL values.

### 5. COALESCE is portable

`COALESCE()` is standard SQL and is supported by many database systems.

`NVL()` and `NVL2()` are strongly associated with Oracle.

---

# Interview Quick Recap

### What is NULL?

Missing or unknown data.

### Can NULL be compared using `=`?

No.

### NVL vs COALESCE?

```text
NVL
→ one replacement

COALESCE
→ first non-NULL from multiple values
```

### What does NVL2 do?

Returns one value if an expression is not NULL and another if it is NULL.

### What does NULLIF do?

Returns NULL when two expressions are equal.

### Does COUNT(column) include NULL?

No.

### Does COUNT(*) include rows with NULL?

Yes.

---

# Key Takeaway

```text
Raw Data
   ↓
Detect NULLs
   ↓
Replace / Interpret / Preserve
   ↓
Clean Data
   ↓
Reliable Analysis
   ↓
Better Business Decisions
```

Handling NULL values correctly is not just data cleaning — it is part of producing trustworthy analytics.

## Topics Covered

`NULL`  
`IS NULL`  
`IS NOT NULL`  
`NVL()`  
`NVL2()`  
`COALESCE()`  
`NULLIF()`  
`Data Cleaning`

### Tags

`SQL` `Data Cleaning` `NULL Handling` `Oracle SQL` `Data Analytics` `SQL Interview`
