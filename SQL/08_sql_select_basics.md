# 08 — SQL SELECT Basics: Asking Questions From a Database

## Big Idea

SQL is the language we use to talk to relational databases.

A `SELECT` statement asks the database a question.

It can answer questions like:

```text
Show me all customers.
Show me only customer names.
Show me customers from Maryland.
Show me orders sorted by date.
Show me customers and their orders together.
```

For beginners, the important thing is this:

```text
SQL is not magic.
SQL is a structured way to ask for data.
```

A basic `SELECT` query usually answers four questions:

```text
What columns do you want?
What table should they come from?
Which rows should be included?
How should the results be sorted?
```

That becomes:

```sql
SELECT ...
FROM ...
WHERE ...
ORDER BY ...;
```

---

## The Basic SELECT Shape

The most common beginner structure is:

```sql
SELECT column_name
FROM table_name;
```

Example:

```sql
SELECT first_name
FROM customer;
```

This says:

```text
Show me the first_name column
from the customer table.
```

---

## SELECT and FROM

The two required ideas in a basic query are usually:

```text
SELECT = what columns do you want?
FROM   = what table are you using?
```

Example:

```sql
SELECT customer_id,
       first_name,
       last_name
FROM customer;
```

### Beginner Translation

```text
SELECT = what do you want to see?
FROM   = where should the database look?
```

---

## Example Table

For this lesson, imagine we have a `customer` table.

| customer_id | first_name | last_name | city | state | zip_code |
|---|---|---|---|---|---|
| C001 | George | Washington | Mt. Vernon | VA | 21208 |
| C002 | Abraham | Lincoln | Springfield | IL | 55555 |
| C003 | Barack | Obama | Washington | DC | 20500 |
| C004 | Lily | Johnson | Crofton | MD | 21114 |
| C005 | Chloe | McKinley | Crofton | MD | 21114 |
| C006 | Eden | Kennedy | Odenton | MD | 21113 |

We will use this table for examples.

---

## Selecting All Columns

You can use `*` to select all columns.

```sql
SELECT *
FROM customer;
```

This means:

```text
Show all columns from the customer table.
```

### Beginner Warning

`SELECT *` is useful while learning or quickly exploring a table.

But in production code, it is usually better to list the exact columns you need.

Why?

Because:

```text
The table may change later.
You may retrieve more data than needed.
The result can become harder to read.
Application code may break if columns are added or removed.
```

Better:

```sql
SELECT customer_id,
       first_name,
       last_name
FROM customer;
```

### Beginner Translation

`SELECT *` means:

```text
Give me everything.
```

But professional code usually says:

```text
Give me exactly these columns.
```

---

## Selecting Specific Columns

Most of the time, you should list the columns you want.

```sql
SELECT customer_id,
       first_name,
       last_name,
       zip_code
FROM customer;
```

This returns only those columns.

| customer_id | first_name | last_name | zip_code |
|---|---|---|---|
| C001 | George | Washington | 21208 |
| C002 | Abraham | Lincoln | 55555 |
| C003 | Barack | Obama | 20500 |
| C004 | Lily | Johnson | 21114 |
| C005 | Chloe | McKinley | 21114 |
| C006 | Eden | Kennedy | 21113 |

### Beginner Translation

The column list is like choosing which fields from a form you want to see.

---

## Column Order

The result columns appear in the order you list them.

```sql
SELECT last_name,
       first_name,
       customer_id
FROM customer;
```

This returns:

| last_name | first_name | customer_id |
|---|---|---|
| Washington | George | C001 |
| Lincoln | Abraham | C002 |

The table itself did not change.

Only the query output changed.

### Beginner Translation

`SELECT` controls the display order of the result columns.

---

## DISTINCT: Removing Duplicates From Results

Sometimes you want only unique values.

Example:

```sql
SELECT state
FROM customer;
```

Might return:

| state |
|---|
| VA |
| IL |
| DC |
| MD |
| MD |
| MD |

If you only want each state once:

```sql
SELECT DISTINCT state
FROM customer;
```

Result:

| state |
|---|
| VA |
| IL |
| DC |
| MD |

### DISTINCT With Multiple Columns

```sql
SELECT DISTINCT city,
                state
FROM customer;
```

This returns unique combinations of `city` and `state`.

### Beginner Translation

`DISTINCT` means:

```text
Do not show duplicate result rows.
```

---

## WHERE: Filtering Rows

The `WHERE` clause filters rows.

Example:

```sql
SELECT customer_id,
       first_name,
       last_name,
       state
FROM customer
WHERE state = 'MD';
```

This says:

```text
Show customers where state equals MD.
```

Result:

| customer_id | first_name | last_name | state |
|---|---|---|---|
| C004 | Lily | Johnson | MD |
| C005 | Chloe | McKinley | MD |
| C006 | Eden | Kennedy | MD |

### Beginner Translation

`WHERE` means:

```text
Only include rows that pass this test.
```

---

## Common Comparison Operators

| Operator | Meaning |
|---|---|
| = | equal to |
| <> or != | not equal to |
| > | greater than |
| < | less than |
| >= | greater than or equal to |
| <= | less than or equal to |

Example:

```sql
SELECT customer_id,
       first_name,
       last_name,
       zip_code
FROM customer
WHERE zip_code >= '21114';
```

### Beginner Warning

If ZIP codes are stored as text, comparisons may behave like text comparisons.

That is not always the same as numeric comparison.

This is one reason data types matter.

---

## AND: Multiple Conditions Must Be True

Use `AND` when both conditions must be true.

```sql
SELECT customer_id,
       first_name,
       last_name,
       city,
       state
FROM customer
WHERE state = 'MD'
  AND city = 'Crofton';
```

This says:

```text
Only show rows where state is MD
and city is Crofton.
```

Both must be true.

---

## OR: Either Condition Can Be True

Use `OR` when either condition can be true.

```sql
SELECT customer_id,
       first_name,
       last_name,
       city,
       state
FROM customer
WHERE city = 'Crofton'
   OR city = 'Odenton';
```

This says:

```text
Show customers in Crofton or Odenton.
```

### Beginner Translation

```text
AND narrows results.
OR expands results.
```

---

## Parentheses With AND and OR

When mixing `AND` and `OR`, use parentheses to make your intent clear.

Bad or unclear:

```sql
SELECT customer_id,
       first_name,
       last_name,
       city,
       state
FROM customer
WHERE state = 'MD'
  AND city = 'Crofton'
   OR city = 'Odenton';
```

Better:

```sql
SELECT customer_id,
       first_name,
       last_name,
       city,
       state
FROM customer
WHERE state = 'MD'
  AND (
       city = 'Crofton'
       OR city = 'Odenton'
  );
```

### Beginner Translation

Parentheses say:

```text
Do this logic together first.
```

---

## IN: Matching One of Several Values

Instead of writing many `OR` conditions, use `IN`.

Longer version:

```sql
SELECT customer_id,
       first_name,
       last_name,
       state
FROM customer
WHERE state = 'MD'
   OR state = 'VA'
   OR state = 'DC';
```

Cleaner version:

```sql
SELECT customer_id,
       first_name,
       last_name,
       state
FROM customer
WHERE state IN ('MD', 'VA', 'DC');
```

### Beginner Translation

`IN` means:

```text
Match anything in this list.
```

---

## BETWEEN: Matching a Range

Use `BETWEEN` for ranges.

Example:

```sql
SELECT customer_id,
       first_name,
       last_name,
       zip_code
FROM customer
WHERE zip_code BETWEEN '21113' AND '21114';
```

For numeric values:

```sql
SELECT product_id,
       product_name,
       price
FROM product
WHERE price BETWEEN 10.00 AND 50.00;
```

### Beginner Translation

`BETWEEN` means:

```text
Value is inside this range.
```

---

## LIKE: Pattern Matching

Use `LIKE` to search for patterns in text.

The percent sign `%` means:

```text
any number of characters
```

Example:

```sql
SELECT customer_id,
       first_name,
       last_name
FROM customer
WHERE last_name LIKE 'J%';
```

This means:

```text
Last name starts with J.
```

It might match:

```text
Johnson
Jones
Jackson
```

Another example:

```sql
SELECT customer_id,
       first_name,
       last_name
FROM customer
WHERE last_name LIKE '%son';
```

This means:

```text
Last name ends with son.
```

### Beginner Translation

`LIKE` is for pattern matching.

`%` is the wildcard.

---

## NULL and IS NULL

A `NULL` means no value is stored.

Do not check NULL like this:

```sql
WHERE secondary_phone = NULL
```

Use:

```sql
WHERE secondary_phone IS NULL
```

Example:

```sql
SELECT customer_id,
       first_name,
       last_name
FROM customer
WHERE secondary_phone IS NULL;
```

To find rows that do have a value:

```sql
SELECT customer_id,
       first_name,
       last_name
FROM customer
WHERE secondary_phone IS NOT NULL;
```

### Beginner Translation

NULL is not a normal value.

Use `IS NULL` or `IS NOT NULL`.

---

## ORDER BY: Sorting Results

The `ORDER BY` clause sorts the result.

```sql
SELECT customer_id,
       first_name,
       last_name,
       zip_code
FROM customer
ORDER BY zip_code;
```

This sorts by ZIP code ascending by default.

### Beginner Translation

`ORDER BY` means:

```text
Sort the results this way.
```

---

## ASC and DESC

Ascending order is the default.

```sql
ORDER BY zip_code ASC;
```

Descending order reverses the sort:

```sql
ORDER BY zip_code DESC;
```

Example:

```sql
SELECT customer_id,
       first_name,
       last_name,
       zip_code
FROM customer
ORDER BY zip_code DESC;
```

### Beginner Translation

```text
ASC  = low to high / A to Z
DESC = high to low / Z to A
```

---

## Sorting by Multiple Columns

You can sort by more than one column.

```sql
SELECT customer_id,
       first_name,
       last_name,
       zip_code
FROM customer
ORDER BY zip_code,
         last_name,
         first_name;
```

This says:

```text
Sort by ZIP code first.
If ZIP codes match, sort by last name.
If last names match, sort by first name.
```

You can mix directions:

```sql
SELECT customer_id,
       first_name,
       last_name,
       zip_code
FROM customer
ORDER BY zip_code DESC,
         last_name ASC,
         first_name ASC;
```

### Beginner Translation

Multiple `ORDER BY` columns are like tie-breakers.

---

## Clause Order

SQL clauses must appear in a specific order.

Common beginner order:

```sql
SELECT ...
FROM ...
WHERE ...
ORDER BY ...;
```

Do not write:

```sql
SELECT ...
WHERE ...
FROM ...;
```

That is invalid.

### Beginner Memory Pattern

```text
SELECT: what columns?
FROM: where from?
WHERE: which rows?
ORDER BY: what order?
```

---

## Aliases

An alias gives a table or column a temporary name in the query.

### Column Alias

```sql
SELECT first_name AS given_name,
       last_name AS family_name
FROM customer;
```

This changes the output column labels.

### Table Alias

```sql
SELECT c.customer_id,
       c.first_name,
       c.last_name
FROM customer AS c;
```

Here, `c` is an alias for `customer`.

### Beginner Translation

An alias is a nickname used inside the query result or query itself.

---

## Why Table Aliases Matter

Aliases become very useful when joining tables.

Without alias:

```sql
SELECT customer.customer_id,
       customer.first_name,
       order_record.order_id
FROM customer
JOIN order_record
     ON order_record.customer_id = customer.customer_id;
```

With alias:

```sql
SELECT c.customer_id,
       c.first_name,
       o.order_id
FROM customer AS c
JOIN order_record AS o
     ON o.customer_id = c.customer_id;
```

Cleaner.

Easier to read.

---

## JOIN: Getting Data From Related Tables

A `JOIN` connects rows from related tables.

Example tables:

### customer

| customer_id | first_name | last_name |
|---|---|---|
| C001 | Jane | Smith |
| C002 | Robert | Jones |

### order_record

| order_id | customer_id | order_date |
|---|---|---|
| O1001 | C001 | 2026-05-01 |
| O1002 | C001 | 2026-05-02 |
| O1003 | C002 | 2026-05-03 |

To show orders with customer names:

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

### Beginner Translation

A join says:

```text
Use the key in one table to match related rows in another table.
```

---

## INNER JOIN

An `INNER JOIN` returns rows where a match exists in both tables.

```sql
SELECT c.customer_id,
       c.first_name,
       c.last_name,
       o.order_id
FROM customer AS c
INNER JOIN order_record AS o
        ON o.customer_id = c.customer_id;
```

If a customer has no orders, that customer will not appear in this result.

### Beginner Translation

INNER JOIN means:

```text
Only show rows with matching partners.
```

---

## LEFT JOIN

A `LEFT JOIN` keeps all rows from the left table, even if there is no match on the right.

```sql
SELECT c.customer_id,
       c.first_name,
       c.last_name,
       o.order_id
FROM customer AS c
LEFT JOIN order_record AS o
       ON o.customer_id = c.customer_id;
```

If a customer has no orders, the customer still appears, but the order columns are NULL.

### Beginner Translation

LEFT JOIN means:

```text
Keep everything from the left table.
Add matching data from the right table when it exists.
```

---

## Aggregates: Counting and Summarizing

Aggregate functions calculate summary values.

Common aggregate functions:

| Function | Meaning |
|---|---|
| COUNT() | Count rows or values |
| SUM() | Add values |
| AVG() | Average values |
| MIN() | Smallest value |
| MAX() | Largest value |

Example:

```sql
SELECT COUNT(*) AS customer_count
FROM customer;
```

This returns the number of rows in `customer`.

---

## GROUP BY

`GROUP BY` groups rows before applying aggregate functions.

Example:

```sql
SELECT state,
       COUNT(*) AS customer_count
FROM customer
GROUP BY state;
```

This means:

```text
Group customers by state,
then count how many customers are in each state.
```

Result:

| state | customer_count |
|---|---|
| DC | 1 |
| IL | 1 |
| MD | 3 |
| VA | 1 |

### Beginner Translation

`GROUP BY` means:

```text
Make piles, then summarize each pile.
```

---

## HAVING

`HAVING` filters groups after aggregation.

Example:

```sql
SELECT state,
       COUNT(*) AS customer_count
FROM customer
GROUP BY state
HAVING COUNT(*) > 1;
```

This means:

```text
Group by state.
Count customers.
Only show states with more than one customer.
```

### WHERE vs. HAVING

| Clause | Filters |
|---|---|
| WHERE | rows before grouping |
| HAVING | groups after grouping |

### Beginner Translation

`WHERE` filters individual rows.

`HAVING` filters grouped results.

---

## Full SELECT Clause Order

A more complete SELECT query follows this order:

```sql
SELECT ...
FROM ...
JOIN ...
WHERE ...
GROUP BY ...
HAVING ...
ORDER BY ...;
```

Memory version:

```text
Choose columns.
Choose table.
Connect tables.
Filter rows.
Group rows.
Filter groups.
Sort output.
```

---

## Common Beginner Mistakes

### Mistake 1: Forgetting FROM

Bad:

```sql
SELECT first_name;
```

Usually, SQL needs to know where the column comes from.

Better:

```sql
SELECT first_name
FROM customer;
```

### Mistake 2: Using SELECT * Everywhere

`SELECT *` is okay for quick exploring.

But for real code, list columns.

### Mistake 3: Confusing WHERE and ORDER BY

`WHERE` filters.

`ORDER BY` sorts.

### Mistake 4: Putting Clauses in the Wrong Order

Bad:

```sql
SELECT first_name
WHERE state = 'MD'
FROM customer;
```

Good:

```sql
SELECT first_name
FROM customer
WHERE state = 'MD';
```

### Mistake 5: Forgetting Quotes Around Text

Bad:

```sql
WHERE state = MD;
```

Good:

```sql
WHERE state = 'MD';
```

### Mistake 6: Checking NULL With Equals

Bad:

```sql
WHERE phone = NULL;
```

Good:

```sql
WHERE phone IS NULL;
```

### Mistake 7: Joining Without an ON Condition

A join without the right condition can produce incorrect results.

Always ask:

```text
Which key connects these tables?
```

---

## The “Question Builder” Analogy

A `SELECT` query is like filling out a request form.

```text
What do you want to see?       SELECT
Where should we look?          FROM
How are tables connected?      JOIN / ON
Which rows should count?       WHERE
Should we make groups?         GROUP BY
Which groups should count?     HAVING
How should it be sorted?       ORDER BY
```

Once you see SQL this way, it becomes less mysterious.

You are not chanting keywords.

You are building a question.

---

## Key Terms

| Term | Meaning |
|---|---|
| SELECT | Chooses columns or expressions to return |
| FROM | Identifies the table |
| WHERE | Filters rows |
| DISTINCT | Removes duplicate result rows |
| ORDER BY | Sorts the result |
| ASC | Ascending sort |
| DESC | Descending sort |
| AND | Both conditions must be true |
| OR | Either condition can be true |
| IN | Match one value from a list |
| BETWEEN | Match a range |
| LIKE | Match a text pattern |
| NULL | Absence of a value |
| IS NULL | Tests for NULL |
| Alias | Temporary name |
| JOIN | Combines related tables |
| INNER JOIN | Keeps matching rows |
| LEFT JOIN | Keeps all left-side rows |
| COUNT | Counts rows or values |
| GROUP BY | Groups rows for summaries |
| HAVING | Filters grouped results |

---

## Quick Check

Answer these in your own words:

1. What does `SELECT` do?
2. What does `FROM` do?
3. What does `WHERE` do?
4. What does `ORDER BY` do?
5. Why should you avoid `SELECT *` in production code?
6. What does `DISTINCT` do?
7. What is the difference between `AND` and `OR`?
8. What does `LIKE 'A%'` mean?
9. Why do we use `IS NULL` instead of `= NULL`?
10. What is the difference between `WHERE` and `HAVING`?
11. What does a `JOIN` do?
12. What is the difference between `INNER JOIN` and `LEFT JOIN`?

---

## Practice Exercise

Using this table:

```text
customer
- customer_id
- first_name
- last_name
- city
- state
- zip_code
```

Write SQL queries for the following:

```text
1. Show all columns from customer.
2. Show only first_name and last_name.
3. Show customers from MD.
4. Show customers from Crofton, MD.
5. Show customers from MD, VA, or DC.
6. Show unique states.
7. Show customers sorted by last_name.
8. Show customers sorted by state, then city, then last_name.
9. Show customers whose last name starts with J.
10. Count how many customers are in each state.
```

Starter answer for #3:

```sql
SELECT customer_id,
       first_name,
       last_name,
       city,
       state
FROM customer
WHERE state = 'MD';
```

---

## Summary

A `SELECT` statement is how we ask a database for information.

The beginner structure is:

```sql
SELECT ...
FROM ...
WHERE ...
ORDER BY ...;
```

As queries grow, we add:

```sql
JOIN
GROUP BY
HAVING
```

The important mental model is:

```text
SQL is a question builder.
```

You tell the database:

```text
what to show,
where to get it,
how to connect it,
what to filter,
how to group it,
and how to sort it.
```

Once that structure clicks, SQL becomes much easier to read and write.

---

## Source Notes

This lesson was drafted from the uploaded SQL and relational database materials, especially the readings on SELECT statements, FROM clauses, filtering columns, filtering rows, DISTINCT, ORDER BY, WHERE conditions, joins, grouping, and SQL formatting.
