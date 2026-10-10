# SQL String Functions & Wildcards

## Overview

String functions help clean, transform, and analyze text data.

Wildcards help search text using patterns.

These concepts are commonly used for:

- Data cleaning
- Pattern matching
- Input validation
- Customer and product analysis
- Preparing data for reports and dashboards

---

## 1. LOWER()

Converts text to lowercase.

```sql
SELECT LOWER(customer_name)
FROM customers;
```

Example:

```text
JOHN DOE → john doe
```

---

## 2. UPPER()

Converts text to uppercase.

```sql
SELECT UPPER(city)
FROM customers;
```

```text
new york → NEW YORK
```

---

## 3. INITCAP()

Capitalizes the first letter of each word.

```sql
SELECT INITCAP(customer_name)
FROM customers;
```

```text
john doe → John Doe
```

Useful for standardizing names and locations.

---

## 4. LENGTH()

Returns the number of characters.

```sql
SELECT LENGTH('Analytics')
FROM dual;
```

Result:

```text
9
```

Useful for validating codes, IDs, and text lengths.

---

## 5. CONCAT()

Combines strings.

```sql
SELECT CONCAT(first_name, last_name)
FROM customers;
```

In Oracle, the `||` operator is often more convenient:

```sql
SELECT first_name || ' ' || last_name AS full_name
FROM customers;
```

---

## 6. SUBSTR()

Extracts part of a string.

```sql
SELECT SUBSTR('Analytics', 1, 4)
FROM dual;
```

Result:

```text
Anal
```

Useful for product codes, prefixes, and text extraction.

---

## 7. TRIM(), LTRIM(), RTRIM()

Remove unwanted spaces or characters.

```sql
SELECT TRIM(customer_name)
FROM customers;
```

```text
"  John Doe  " → "John Doe"
```

Left side only:

```sql
SELECT LTRIM(customer_name)
FROM customers;
```

Right side only:

```sql
SELECT RTRIM(customer_name)
FROM customers;
```

---

## 8. REPLACE()

Replaces specific text.

```sql
SELECT REPLACE(
    'NY City',
    'NY',
    'New York'
)
FROM dual;
```

Result:

```text
New York City
```

Useful for correcting or standardizing text.

---

## 9. INSTR()

Finds the position of a substring.

```sql
SELECT INSTR(
    'Database',
    'base'
)
FROM dual;
```

Result:

```text
5
```

If the substring is not found, Oracle returns `0`.

---

# Wildcards with LIKE

SQL commonly uses two wildcard symbols.

```text
% → zero or more characters
_ → exactly one character
```

## Starts With

```sql
SELECT *
FROM customers
WHERE name LIKE 'A%';
```

Matches:

```text
Alice
Andrew
Analytics
```

---

## Ends With

```sql
SELECT *
FROM customers
WHERE name LIKE '%a';
```

---

## Contains

```sql
SELECT *
FROM customers
WHERE name LIKE '%a%';
```

---

## Second Character is "a"

```sql
SELECT *
FROM customers
WHERE name LIKE '_a%';
```

The `_` represents exactly one character before `a`.

---

## Exact Length

Three-character values:

```sql
SELECT *
FROM products
WHERE product_code LIKE '___';
```

Three underscores = exactly three characters.

---

# Practical Cleaning Example

```sql
SELECT
    INITCAP(TRIM(customer_name)) AS clean_name,
    LOWER(TRIM(email)) AS clean_email,
    UPPER(TRIM(city)) AS clean_city
FROM customers;
```

This combines several functions to standardize messy text.

---

# Practical Pattern Example

Find customers using Gmail:

```sql
SELECT *
FROM customers
WHERE LOWER(email) LIKE '%@gmail.com';
```

---

# String Function Quick Recap

| Function | Purpose |
|---|---|
| `LOWER()` | Convert to lowercase |
| `UPPER()` | Convert to uppercase |
| `INITCAP()` | Capitalize each word |
| `LENGTH()` | Count characters |
| `CONCAT()` | Combine strings |
| `SUBSTR()` | Extract text |
| `TRIM()` | Remove leading/trailing spaces |
| `LTRIM()` | Remove from left |
| `RTRIM()` | Remove from right |
| `REPLACE()` | Replace text |
| `INSTR()` | Find substring position |

---

# Must-Remember Notes

### 1. `%` and `_` are different

```text
% → any number of characters
_ → exactly one character
```

### 2. Case sensitivity depends on the database

If needed, standardize before searching:

```sql
WHERE LOWER(name) LIKE 'john%'
```

### 3. Functions can be combined

```sql
SELECT UPPER(TRIM(city))
FROM customers;
```

### 4. Watch NULL values

Many string functions return `NULL` when the input is `NULL`.

### 5. Avoid unnecessary transformations on huge datasets

Applying functions to columns used for filtering may affect index usage and performance.

---

# Interview Quick Recap

**LOWER vs UPPER?**

```text
LOWER → lowercase
UPPER → uppercase
```

**SUBSTR vs INSTR?**

```text
SUBSTR → extracts text
INSTR  → finds position
```

**% vs _?**

```text
% → multiple characters
_ → one character
```

**How do you find names beginning with A?**

```sql
WHERE name LIKE 'A%'
```

**How do you find a 3-character code?**

```sql
WHERE code LIKE '___'
```

---

# Key Takeaway

```text
Messy Text
    ↓
String Functions
    ↓
Clean / Standardized Text
    ↓
Wildcards
    ↓
Pattern Matching
    ↓
Analysis-Ready Data
```

String functions reshape text.

Wildcards help search it.

Together, they make text data much easier to analyze.

## Topics Covered

`LOWER()` `UPPER()` `INITCAP()` `LENGTH()` `CONCAT()` `SUBSTR()` `TRIM()` `REPLACE()` `INSTR()` `LIKE` `%` `_`

### Tags

`SQL` `String Functions` `Wildcards` `Data Cleaning` `Oracle SQL` `Data Analytics` `SQL Interview`
