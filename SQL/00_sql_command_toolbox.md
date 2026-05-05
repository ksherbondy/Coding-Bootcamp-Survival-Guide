# SQL Command Toolbox

## Big Idea

SQL is a toolbox.

Each command has a job.

Beginners often struggle because SQL is taught as a pile of keywords:

```text
SELECT
FROM
WHERE
JOIN
GROUP BY
HAVING
ORDER BY
INSERT
UPDATE
DELETE
CREATE
ALTER
DROP
```

But the better question is:

```text
What am I trying to do?
```

Once you know the goal, the command becomes easier to pick.

---

## SQL Command Toolbox

| If your goal is to... | Use this Command / Clause | Works With | Returns / Changes | Think of it as... |
| :--- | :--- | :--- | :--- | :--- |
| "See data" | `SELECT` | Tables / views | Rows / columns | **The Spotlight:** chooses what columns you want to see. |
| "Pick the source" | `FROM` | Tables / views | Source table | **The Shelf:** where the data comes from. |
| "Keep some rows" | `WHERE` | Rows | Filtered rows | **The Gatekeeper:** only lets matching rows through. |
| "Sort results" | `ORDER BY` | Result set | Ordered rows | **The Organizer:** puts rows in a chosen order. |
| "Remove duplicates" | `DISTINCT` | Result set | Unique rows | **The Duplicate Filter:** keeps only unique result rows. |
| "Connect tables" | `JOIN` | Related tables | Combined rows | **The Bridge:** connects rows using matching keys. |
| "Explain the connection" | `ON` | Joined tables | Join condition | **The Bolt:** fastens one table to another. |
| "Group rows" | `GROUP BY` | Rows | Groups | **The Pile Maker:** creates piles before counting or summing. |
| "Filter groups" | `HAVING` | Groups | Filtered groups | **The Group Gatekeeper:** filters after grouping. |
| "Count things" | `COUNT()` | Rows / values | Number | **The Tally Counter:** counts rows or non-null values. |
| "Add values" | `SUM()` | Numeric values | Number | **The Cash Register:** adds values together. |
| "Average values" | `AVG()` | Numeric values | Number | **The Balancer:** finds the average. |
| "Find smallest" | `MIN()` | Values | Value | **The Floor:** finds the lowest value. |
| "Find largest" | `MAX()` | Values | Value | **The Ceiling:** finds the highest value. |
| "Rename output temporarily" | `AS` | Columns / tables | Alias | **The Name Tag:** gives something a temporary readable name. |
| "Match one of many values" | `IN` | Lists | Filtered rows | **The Shortlist:** checks whether a value is in a list. |
| "Match a range" | `BETWEEN` | Comparable values | Filtered rows | **The Fence:** keeps values inside a range. |
| "Match a text pattern" | `LIKE` | Text | Filtered rows | **The Pattern Finder:** searches using wildcards. |
| "Check missing values" | `IS NULL` | Nullable columns | Filtered rows | **The Empty Slot Checker:** finds missing values. |
| "Add new rows" | `INSERT` | Table | Changes data | **The Intake Form:** adds new records. |
| "Change existing rows" | `UPDATE` | Table rows | Changes data | **The Editor:** modifies existing records. |
| "Remove rows" | `DELETE` | Table rows | Changes data | **The Shredder:** removes matching records. |
| "Make a table" | `CREATE TABLE` | Database schema | Changes structure | **The Blueprint Builder:** creates a new table. |
| "Change a table" | `ALTER TABLE` | Database schema | Changes structure | **The Remodel Tool:** changes table structure. |
| "Remove a table" | `DROP TABLE` | Database schema | Changes structure | **The Demolition Tool:** deletes the table structure. |
| "Protect row identity" | `PRIMARY KEY` | Table column(s) | Constraint | **The ID Badge:** uniquely identifies each row. |
| "Connect to another table" | `FOREIGN KEY` | Table column(s) | Constraint | **The Reference Link:** points to a row in another table. |
| "Require a value" | `NOT NULL` | Column | Constraint | **The Required Blank:** this field cannot be empty. |
| "Prevent duplicates" | `UNIQUE` | Column(s) | Constraint | **The Duplicate Lock:** no repeated values allowed. |
| "Limit allowed values" | `CHECK` | Column / table | Constraint | **The Rule Guard:** blocks values that break a rule. |

---

## How to Pick Your SQL Tool

### Do I want to see data?

Start with:

```sql
SELECT
FROM
```

Example:

```sql
SELECT customer_id,
       first_name,
       last_name
FROM customer;
```

Then ask:

```text
Do I only want some rows?
```

Use:

```sql
WHERE
```

Example:

```sql
SELECT customer_id,
       first_name,
       last_name
FROM customer
WHERE state = 'MD';
```

---

### Do I want the results sorted?

Use:

```sql
ORDER BY
```

Example:

```sql
SELECT customer_id,
       first_name,
       last_name
FROM customer
ORDER BY last_name;
```

Use `DESC` for reverse order:

```sql
SELECT customer_id,
       first_name,
       last_name
FROM customer
ORDER BY last_name DESC;
```

---

### Do I want only unique results?

Use:

```sql
DISTINCT
```

Example:

```sql
SELECT DISTINCT state
FROM customer;
```

This returns each state once.

---

### Do I need data from more than one table?

Use:

```sql
JOIN
ON
```

Example:

```sql
SELECT c.customer_id,
       c.first_name,
       c.last_name,
       o.order_id,
       o.order_date
FROM customer AS c
JOIN order_record AS o
     ON o.customer_id = c.customer_id;
```

Think:

```text
JOIN = connect the tables
ON   = explain how they connect
```

---

### Do I need totals, counts, averages, minimums, or maximums?

Use aggregate functions:

```sql
COUNT()
SUM()
AVG()
MIN()
MAX()
```

Examples:

```sql
SELECT COUNT(*) AS customer_count
FROM customer;
```

```sql
SELECT AVG(price) AS average_price
FROM product;
```

```sql
SELECT MIN(price) AS lowest_price,
       MAX(price) AS highest_price
FROM product;
```

---

### Do I need summaries by category?

Use:

```sql
GROUP BY
```

Example:

```sql
SELECT state,
       COUNT(*) AS customer_count
FROM customer
GROUP BY state;
```

Think:

```text
GROUP BY makes piles.
The aggregate function summarizes each pile.
```

---

### Do I need to filter the summary groups?

Use:

```sql
HAVING
```

Example:

```sql
SELECT state,
       COUNT(*) AS customer_count
FROM customer
GROUP BY state
HAVING COUNT(*) > 1;
```

Remember:

```text
WHERE filters rows before grouping.
HAVING filters groups after grouping.
```

---

## Do I Want to Change Data?

Changing data means modifying the rows stored inside tables.

These commands are powerful.

Use them carefully.

---

### Add new rows

Use:

```sql
INSERT
```

Example:

```sql
INSERT INTO customer (
    customer_id,
    first_name,
    last_name,
    state
)
VALUES (
    'C001',
    'Jane',
    'Smith',
    'MD'
);
```

Think:

```text
INSERT = add a new filled-out form to the table.
```

---

### Change existing rows

Use:

```sql
UPDATE
```

Example:

```sql
UPDATE customer
SET phone = '555-9999'
WHERE customer_id = 'C001';
```

Think:

```text
UPDATE = edit existing records.
```

Warning:

```text
UPDATE without WHERE can change every row.
```

Dangerous:

```sql
UPDATE customer
SET state = 'MD';
```

---

### Remove rows

Use:

```sql
DELETE
```

Example:

```sql
DELETE FROM customer
WHERE customer_id = 'C001';
```

Think:

```text
DELETE = remove matching records.
```

Warning:

```text
DELETE without WHERE can remove every row.
```

Dangerous:

```sql
DELETE FROM customer;
```

---

## Do I Want to Change the Structure?

Changing structure means changing the database schema.

These commands affect tables, columns, and constraints.

---

### Create a table

Use:

```sql
CREATE TABLE
```

Example:

```sql
CREATE TABLE customer (
    customer_id CHAR(8) PRIMARY KEY,
    first_name  VARCHAR(25) NOT NULL,
    last_name   VARCHAR(25) NOT NULL,
    state       CHAR(2)
);
```

Think:

```text
CREATE TABLE = build a new table blueprint.
```

---

### Change an existing table

Use:

```sql
ALTER TABLE
```

Example:

```sql
ALTER TABLE customer
ADD email VARCHAR(100);
```

Think:

```text
ALTER TABLE = remodel an existing table.
```

---

### Remove a table

Use:

```sql
DROP TABLE
```

Example:

```sql
DROP TABLE customer;
```

Think:

```text
DROP TABLE = demolish the table structure.
```

Warning:

```text
DROP TABLE removes the table itself, not just the rows.
```

---

## Decision Tree: What Am I Trying to Do?

```text
Do I want to see data?
    → SELECT

Do I need to choose where the data comes from?
    → FROM

Do I need only some rows?
    → WHERE

Do I need to connect tables?
    → JOIN ... ON

Do I need unique results?
    → DISTINCT

Do I need sorted results?
    → ORDER BY

Do I need counts, totals, averages, smallest, or largest?
    → COUNT(), SUM(), AVG(), MIN(), MAX()

Do I need summaries by category?
    → GROUP BY

Do I need to filter summary groups?
    → HAVING

Do I need to add rows?
    → INSERT

Do I need to change rows?
    → UPDATE

Do I need to remove rows?
    → DELETE

Do I need to create structure?
    → CREATE TABLE

Do I need to change structure?
    → ALTER TABLE

Do I need to remove structure?
    → DROP TABLE
```

---

## Decision Tree Update: What Am I Working With?

Before adding the next SQL clause, ask what you are currently working with.

### Am I working with raw table rows?

Use:

```sql
WHERE
```

Example:

```sql
SELECT customer_id,
       first_name,
       last_name
FROM customer
WHERE state = 'MD';
```

---

### Am I working with grouped rows?

Use:

```sql
HAVING
```

Example:

```sql
SELECT state,
       COUNT(*) AS customer_count
FROM customer
GROUP BY state
HAVING COUNT(*) > 5;
```

---

### Am I combining tables?

Use:

```sql
JOIN ... ON
```

Example:

```sql
SELECT c.customer_id,
       o.order_id
FROM customer AS c
JOIN order_record AS o
     ON o.customer_id = c.customer_id;
```

---

### Am I summarizing values?

Use:

```sql
COUNT()
SUM()
AVG()
MIN()
MAX()
```

Example:

```sql
SELECT COUNT(*) AS order_count
FROM order_record;
```

---

### Am I changing stored data?

Use:

```sql
INSERT
UPDATE
DELETE
```

Reminder:

```text
INSERT adds.
UPDATE edits.
DELETE removes.
```

---

### Am I changing the database structure?

Use:

```sql
CREATE
ALTER
DROP
```

Reminder:

```text
CREATE builds.
ALTER remodels.
DROP demolishes.
```

---

## SELECT Command Family

The `SELECT` family is for reading data.

| If your goal is to... | Use | Example |
|---|---|---|
| Pick columns | `SELECT` | `SELECT first_name FROM customer;` |
| Pick the source table | `FROM` | `FROM customer` |
| Filter rows | `WHERE` | `WHERE state = 'MD'` |
| Remove duplicates | `DISTINCT` | `SELECT DISTINCT state` |
| Sort rows | `ORDER BY` | `ORDER BY last_name` |
| Sort descending | `DESC` | `ORDER BY order_date DESC` |
| Rename output | `AS` | `COUNT(*) AS total` |

Basic shape:

```sql
SELECT column_1,
       column_2
FROM table_name
WHERE condition
ORDER BY column_name;
```

---

## Filtering Toolbox

| If your goal is to... | Use | Example |
|---|---|---|
| Match exactly | `=` | `WHERE state = 'MD'` |
| Exclude value | `<>` or `!=` | `WHERE state <> 'MD'` |
| Compare numbers/dates | `>`, `<`, `>=`, `<=` | `WHERE price >= 100` |
| Match list | `IN` | `WHERE state IN ('MD', 'VA')` |
| Match range | `BETWEEN` | `WHERE price BETWEEN 10 AND 50` |
| Match pattern | `LIKE` | `WHERE last_name LIKE 'S%'` |
| Find missing values | `IS NULL` | `WHERE phone IS NULL` |
| Find present values | `IS NOT NULL` | `WHERE phone IS NOT NULL` |
| Require both conditions | `AND` | `WHERE state = 'MD' AND city = 'Crofton'` |
| Allow either condition | `OR` | `WHERE state = 'MD' OR state = 'VA'` |

---

## JOIN Toolbox

| If your goal is to... | Use | Think of it as... |
|---|---|---|
| Keep only matching rows | `INNER JOIN` | **The Matchmaker:** only matched pairs survive. |
| Keep all left table rows | `LEFT JOIN` | **The Preserve Left Tool:** keeps the left side even without matches. |
| Explain table connection | `ON` | **The Connector Rule:** tells SQL which keys match. |

Example:

```sql
SELECT c.customer_id,
       c.first_name,
       o.order_id
FROM customer AS c
INNER JOIN order_record AS o
        ON o.customer_id = c.customer_id;
```

Left join example:

```sql
SELECT c.customer_id,
       c.first_name,
       o.order_id
FROM customer AS c
LEFT JOIN order_record AS o
       ON o.customer_id = c.customer_id;
```

---

## Aggregate Toolbox

| If your goal is to... | Use | Think of it as... |
|---|---|---|
| Count rows | `COUNT(*)` | **The Row Counter** |
| Count non-null values | `COUNT(column)` | **The Filled-In Counter** |
| Add values | `SUM(column)` | **The Cash Register** |
| Average values | `AVG(column)` | **The Balancer** |
| Find smallest | `MIN(column)` | **The Floor** |
| Find largest | `MAX(column)` | **The Ceiling** |

Example:

```sql
SELECT COUNT(*) AS order_count,
       SUM(order_total) AS revenue_total,
       AVG(order_total) AS average_order_total
FROM order_record;
```

Grouped example:

```sql
SELECT customer_id,
       COUNT(*) AS order_count
FROM order_record
GROUP BY customer_id;
```

---

## Data Change Toolbox

| If your goal is to... | Use | Think of it as... |
|---|---|---|
| Add rows | `INSERT` | **The Intake Form** |
| Edit rows | `UPDATE` | **The Editor** |
| Remove rows | `DELETE` | **The Shredder** |

Insert:

```sql
INSERT INTO customer (
    customer_id,
    first_name,
    last_name
)
VALUES (
    'C001',
    'Jane',
    'Smith'
);
```

Update:

```sql
UPDATE customer
SET last_name = 'Johnson'
WHERE customer_id = 'C001';
```

Delete:

```sql
DELETE FROM customer
WHERE customer_id = 'C001';
```

Safety rule:

```text
Before UPDATE or DELETE, write the WHERE clause carefully.
```

---

## Structure Change Toolbox

| If your goal is to... | Use | Think of it as... |
|---|---|---|
| Create table | `CREATE TABLE` | **The Blueprint Builder** |
| Add/change table structure | `ALTER TABLE` | **The Remodel Tool** |
| Remove table | `DROP TABLE` | **The Demolition Tool** |

Create:

```sql
CREATE TABLE customer (
    customer_id CHAR(8) PRIMARY KEY,
    first_name  VARCHAR(25) NOT NULL,
    last_name   VARCHAR(25) NOT NULL
);
```

Alter:

```sql
ALTER TABLE customer
ADD email VARCHAR(100);
```

Drop:

```sql
DROP TABLE customer;
```

Warning:

```text
DROP TABLE removes the table structure.
DELETE removes rows.
```

---

## Constraint Toolbox

| If your goal is to... | Use | Think of it as... |
|---|---|---|
| Identify each row | `PRIMARY KEY` | **The ID Badge** |
| Connect to another table | `FOREIGN KEY` | **The Reference Link** |
| Require a value | `NOT NULL` | **The Required Blank** |
| Prevent duplicates | `UNIQUE` | **The Duplicate Lock** |
| Limit allowed values | `CHECK` | **The Rule Guard** |
| Provide fallback value | `DEFAULT` | **The Auto-Fill** |

Example:

```sql
CREATE TABLE customer (
    customer_id CHAR(8) PRIMARY KEY,
    email       VARCHAR(100) UNIQUE,
    first_name  VARCHAR(25) NOT NULL,
    last_name   VARCHAR(25) NOT NULL,
    state       CHAR(2) DEFAULT 'MD',
    CHECK (state IN ('MD', 'VA', 'DC'))
);
```

Foreign key example:

```sql
CREATE TABLE order_record (
    order_id    CHAR(8) PRIMARY KEY,
    customer_id CHAR(8) NOT NULL,
    order_date  DATE NOT NULL,
    FOREIGN KEY (customer_id) REFERENCES customer(customer_id)
);
```

---

## SQL Clause Order

When writing a `SELECT` query, clauses appear in this common order:

```sql
SELECT
FROM
JOIN
WHERE
GROUP BY
HAVING
ORDER BY
```

Memory version:

```text
SELECT     → What do I want to see?
FROM       → Where does it come from?
JOIN       → What other tables are connected?
WHERE      → Which rows should pass?
GROUP BY   → What piles should I make?
HAVING     → Which piles should pass?
ORDER BY   → How should the final result be sorted?
```

Example:

```sql
SELECT c.state,
       COUNT(o.order_id) AS order_count
FROM customer AS c
JOIN order_record AS o
     ON o.customer_id = c.customer_id
WHERE o.order_date >= '2026-01-01'
GROUP BY c.state
HAVING COUNT(o.order_id) > 5
ORDER BY order_count DESC;
```

---

## SQL Safety Rules

### 1. `SELECT` before `UPDATE`

Before running an `UPDATE`, test the `WHERE` clause with `SELECT`.

```sql
SELECT customer_id,
       first_name,
       last_name
FROM customer
WHERE customer_id = 'C001';
```

Then update:

```sql
UPDATE customer
SET phone = '555-9999'
WHERE customer_id = 'C001';
```

---

### 2. `SELECT` before `DELETE`

Before running a `DELETE`, test the `WHERE` clause with `SELECT`.

```sql
SELECT customer_id,
       first_name,
       last_name
FROM customer
WHERE customer_id = 'C001';
```

Then delete:

```sql
DELETE FROM customer
WHERE customer_id = 'C001';
```

---

### 3. Avoid `SELECT *` in production code

Use:

```sql
SELECT customer_id,
       first_name,
       last_name
FROM customer;
```

Instead of:

```sql
SELECT *
FROM customer;
```

---

### 4. Always list columns in `INSERT`

Use:

```sql
INSERT INTO customer (
    customer_id,
    first_name,
    last_name
)
VALUES (
    'C001',
    'Jane',
    'Smith'
);
```

Avoid:

```sql
INSERT INTO customer
VALUES ('C001', 'Jane', 'Smith');
```

---

### 5. Be careful with `DROP`

`DROP TABLE` removes the table structure.

This is not the same as deleting one row.

```sql
DROP TABLE customer;
```

Think twice before running it.

---

## Common Beginner Confusions

### `WHERE` vs. `HAVING`

| Clause | Filters |
|---|---|
| `WHERE` | Rows before grouping |
| `HAVING` | Groups after grouping |

Example:

```sql
SELECT state,
       COUNT(*) AS customer_count
FROM customer
WHERE account_status = 'ACTIVE'
GROUP BY state
HAVING COUNT(*) > 10;
```

Meaning:

```text
First keep only active customers.
Then group by state.
Then keep only states with more than 10 active customers.
```

---

### `DELETE` vs. `DROP`

| Command | Removes |
|---|---|
| `DELETE` | Rows from a table |
| `DROP TABLE` | The table itself |

Example:

```sql
DELETE FROM customer
WHERE customer_id = 'C001';
```

removes one matching row.

```sql
DROP TABLE customer;
```

removes the table structure.

---

### `INNER JOIN` vs. `LEFT JOIN`

| Join | Result |
|---|---|
| `INNER JOIN` | Only matching rows |
| `LEFT JOIN` | All rows from the left table, plus matches from the right |

Use `LEFT JOIN` when you want to keep rows even if related data is missing.

Example:

```sql
SELECT c.customer_id,
       c.first_name,
       o.order_id
FROM customer AS c
LEFT JOIN order_record AS o
       ON o.customer_id = c.customer_id;
```

This can show customers even if they have no orders.

---

### `COUNT(*)` vs. `COUNT(column)`

| Function | Counts |
|---|---|
| `COUNT(*)` | Rows |
| `COUNT(column)` | Non-null values in that column |

Example:

```sql
SELECT COUNT(*) AS total_rows,
       COUNT(phone) AS rows_with_phone
FROM customer;
```

---

### `NULL` vs. Empty String vs. Zero

`NULL` means no value.

It is not the same as:

```text
0
''
false
blank space
```

Use:

```sql
WHERE phone IS NULL
```

not:

```sql
WHERE phone = NULL
```

---

## Mini Examples by Goal

### Goal: Show all customers from Maryland

```sql
SELECT customer_id,
       first_name,
       last_name,
       state
FROM customer
WHERE state = 'MD';
```

---

### Goal: Show unique states

```sql
SELECT DISTINCT state
FROM customer;
```

---

### Goal: Count customers by state

```sql
SELECT state,
       COUNT(*) AS customer_count
FROM customer
GROUP BY state;
```

---

### Goal: Show customers and their orders

```sql
SELECT c.customer_id,
       c.first_name,
       c.last_name,
       o.order_id,
       o.order_date
FROM customer AS c
JOIN order_record AS o
     ON o.customer_id = c.customer_id;
```

---

### Goal: Add a new product

```sql
INSERT INTO product (
    product_id,
    product_name,
    price
)
VALUES (
    'P001',
    'Laptop',
    999.99
);
```

---

### Goal: Update a product price

```sql
UPDATE product
SET price = 899.99
WHERE product_id = 'P001';
```

---

### Goal: Delete a product

```sql
DELETE FROM product
WHERE product_id = 'P001';
```

---

### Goal: Create a new table

```sql
CREATE TABLE product (
    product_id   CHAR(8) PRIMARY KEY,
    product_name VARCHAR(50) NOT NULL,
    price        DECIMAL(10, 2) NOT NULL
);
```

---

## Quick Check

Answer these in your own words:

1. Which command do you use to see data?
2. Which clause chooses the source table?
3. Which clause filters rows?
4. Which clause sorts results?
5. Which clause connects tables?
6. Which clause explains how joined tables connect?
7. Which function counts rows?
8. Which clause groups rows before summarizing?
9. Which clause filters groups?
10. Which command adds rows?
11. Which command edits rows?
12. Which command removes rows?
13. Which command creates a table?
14. Which command changes a table structure?
15. Which command removes a table structure?

---

## Practice Exercise

For each goal, choose the SQL command or clause.

| Goal | SQL Tool |
|---|---|
| Show customer names | |
| Filter customers from MD | |
| Sort customers by last name | |
| Connect customers to orders | |
| Count orders | |
| Group orders by customer | |
| Add a new customer | |
| Change a customer's phone number | |
| Delete one customer | |
| Create a customer table | |
| Add a new column to customer | |
| Remove a table | |

Suggested answers:

| Goal | SQL Tool |
|---|---|
| Show customer names | `SELECT` |
| Filter customers from MD | `WHERE` |
| Sort customers by last name | `ORDER BY` |
| Connect customers to orders | `JOIN ... ON` |
| Count orders | `COUNT()` |
| Group orders by customer | `GROUP BY` |
| Add a new customer | `INSERT` |
| Change a customer's phone number | `UPDATE` |
| Delete one customer | `DELETE` |
| Create a customer table | `CREATE TABLE` |
| Add a new column to customer | `ALTER TABLE` |
| Remove a table | `DROP TABLE` |

---

## Summary

SQL becomes easier when you treat commands as tools.

The main decision is:

```text
What am I trying to do?
```

Then pick the tool:

```text
See data       → SELECT
Choose source  → FROM
Filter rows    → WHERE
Connect tables → JOIN ... ON
Sort results   → ORDER BY
Summarize      → COUNT, SUM, AVG, MIN, MAX
Group rows     → GROUP BY
Filter groups  → HAVING
Add rows       → INSERT
Edit rows      → UPDATE
Remove rows    → DELETE
Build tables   → CREATE TABLE
Change tables  → ALTER TABLE
Remove tables  → DROP TABLE
Protect data   → constraints
```

The goal is not to memorize SQL as a wall of keywords.

The goal is to learn which tool solves which problem.
