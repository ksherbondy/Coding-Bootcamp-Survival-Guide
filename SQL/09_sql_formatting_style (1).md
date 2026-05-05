# 09 — SQL Formatting and Style: Writing Queries Humans Can Read

## Big Idea

SQL formatting does not usually change what a query does.

But it absolutely changes how easy the query is to read, debug, review, maintain, and trust.

A database may not care whether your SQL is ugly.

Humans do.

And humans are the ones who have to fix it later.

For beginners, this lesson matters because SQL can quickly become visually confusing. Formatting gives your eyes a path through the query.

Good formatting is not decoration.

Good formatting is communication.

---

## Why Formatting Matters

Look at this query:

```sql
SELECT id, FirstName, LASTNAME,c.nAme FROM people p left JOIN cities AS c on c.id=p.cityid;
```

It might run.

But it is hard to read.

Now compare it to this:

```sql
SELECT p.person_id,
       p.first_name,
       p.last_name,
       c.name AS city_name
FROM person AS p
LEFT JOIN city AS c
       ON c.city_id = p.city_id;
```

Same general idea.

Very different readability.

The second version makes it easier to see:

```text
Which columns are selected
Which tables are used
How the tables connect
What aliases mean
Where each clause begins
```

### Beginner Translation

Bad formatting makes the reader untangle the query.

Good formatting lets the reader follow the structure.

---

## Formatting Is a Debugging Tool

SQL bugs often hide in visual clutter.

Formatting helps you catch things like:

```text
missing commas
wrong join condition
wrong AND/OR grouping
missing WHERE clause
accidental SELECT *
ambiguous column names
incorrect nesting
```

When the query is formatted cleanly, mistakes stand out.

When the query is crammed into one line, mistakes hide.

---

## The Core Rule: Be Consistent

There is not one universal SQL style used by everyone.

Different teams and companies may prefer different styles.

But the most important rule is:

```text
Pick a style and use it consistently.
```

A project with one consistent style is easier to read than a project where every developer formats SQL differently.

Consistency lowers mental friction.

---

## Recommended Beginner Style

For this training material, we will use this style:

```text
SQL keywords in uppercase
table and column names in lowercase
underscores between words
one major clause per line
one selected column per line when there are multiple columns
explicit JOIN syntax
meaningful aliases
semicolons at the end
no SELECT * in production examples
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
     ON o.customer_id = c.customer_id
WHERE c.state = 'MD'
ORDER BY o.order_date DESC;
```

---

## Capitalization

A common convention is:

```text
SQL keywords: uppercase
identifiers: lowercase
```

Example:

```sql
SELECT customer_id,
       first_name,
       last_name
FROM customer
WHERE state = 'MD';
```

Keywords:

```text
SELECT
FROM
WHERE
```

Identifiers:

```text
customer_id
first_name
last_name
customer
state
```

### Why This Helps

Uppercase keywords make the structure visible.

Lowercase identifiers make database objects distinct from SQL commands.

### Beginner Translation

Capitalization separates the language from the names.

---

## Use Underscores for Multi-Word Names

Prefer:

```text
first_name
last_name
order_date
customer_id
```

Avoid inconsistent styles like:

```text
FirstName
firstname
first-name
first name
FIRST_NAME
```

For SQL identifiers, spaces create problems and often require quoting.

So use underscores.

### Good

```sql
SELECT first_name,
       last_name,
       order_date
FROM order_record;
```

### Avoid

```sql
SELECT "First Name",
       "Last Name",
       "Order Date"
FROM "Order Record";
```

Quoted identifiers can become annoying and less portable.

### Beginner Translation

Underscores make names readable without needing special handling.

---

## Avoid Vague Names

Bad names:

```text
id
name
data
info
value
thing
misc
```

Better names:

```text
customer_id
product_name
order_date
shipping_addr
invoice_total
```

### Why `id` Can Be Confusing

In one table, `id` might seem fine.

But in a join, multiple tables may have an `id`.

```sql
SELECT id,
       name
FROM customer
JOIN order_record
     ON customer.id = order_record.id;
```

Which `id`?

Which `name`?

Better:

```sql
SELECT c.customer_id,
       c.first_name,
       o.order_id
FROM customer AS c
JOIN order_record AS o
     ON o.customer_id = c.customer_id;
```

### Beginner Translation

Names should explain themselves when read inside a query.

---

## Use Meaningful Table Aliases

Aliases shorten table names and make joins easier to read.

Good:

```sql
SELECT c.customer_id,
       c.first_name,
       o.order_id
FROM customer AS c
JOIN order_record AS o
     ON o.customer_id = c.customer_id;
```

Here:

```text
c = customer
o = order_record
```

Avoid aliases that tell the reader nothing:

```sql
SELECT x.customer_id,
       y.order_id
FROM customer AS x
JOIN order_record AS y
     ON y.customer_id = x.customer_id;
```

`x` and `y` do not carry meaning.

### Beginner Rule

Use aliases that remind you of the table.

```text
customer      → c
order_record  → o
order_line    → ol
product       → p
salesperson   → s
```

---

## Use AS for Aliases

Some SQL dialects allow aliases without `AS`.

But using `AS` makes the alias explicit.

Clear:

```sql
SELECT first_name AS given_name,
       last_name AS family_name
FROM customer AS c;
```

Less clear:

```sql
SELECT first_name given_name,
       last_name family_name
FROM customer c;
```

### Beginner Translation

`AS` says:

```text
I am giving this a temporary name.
```

That reduces guessing.

---

## Put Each Major Clause on Its Own Line

Bad:

```sql
SELECT customer_id, first_name, last_name FROM customer WHERE state = 'MD' ORDER BY last_name;
```

Better:

```sql
SELECT customer_id,
       first_name,
       last_name
FROM customer
WHERE state = 'MD'
ORDER BY last_name;
```

Major clauses include:

```text
SELECT
FROM
JOIN
ON
WHERE
GROUP BY
HAVING
ORDER BY
```

### Beginner Translation

Each clause is a section of the question.

Give each section its own line.

---

## Put Multiple Selected Columns on Separate Lines

For short queries, one line may be fine.

```sql
SELECT customer_id
FROM customer;
```

But when selecting multiple columns, use separate lines.

```sql
SELECT customer_id,
       first_name,
       last_name,
       city,
       state
FROM customer;
```

This makes it easier to:

```text
add columns
remove columns
comment out columns
spot missing commas
compare changes in git
```

---

## Commas: End of Line vs. Beginning of Line

Two common styles exist.

### Trailing Commas

```sql
SELECT customer_id,
       first_name,
       last_name,
       state
FROM customer;
```

### Leading Commas

```sql
SELECT customer_id
     , first_name
     , last_name
     , state
FROM customer;
```

Both styles exist.

For beginners, trailing commas are more familiar.

This training material will use trailing commas.

### Watch Out

Trailing commas can cause errors if you leave one before `FROM`.

Bad:

```sql
SELECT customer_id,
       first_name,
       last_name,
FROM customer;
```

Good:

```sql
SELECT customer_id,
       first_name,
       last_name
FROM customer;
```

---

## Align Columns for Readability

This style is easy to scan:

```sql
SELECT c.customer_id,
       c.first_name,
       c.last_name,
       c.email
FROM customer AS c;
```

Notice the selected columns line up.

This gives your eyes a vertical path.

### Beginner Translation

Alignment turns the query into a readable list.

---

## Indent JOIN Conditions

A join has two parts:

```text
the table being joined
the condition that explains how it connects
```

Format them clearly:

```sql
SELECT c.customer_id,
       c.first_name,
       o.order_id
FROM customer AS c
JOIN order_record AS o
     ON o.customer_id = c.customer_id;
```

For multiple join conditions:

```sql
SELECT c.customer_id,
       o.order_id
FROM customer AS c
JOIN order_record AS o
     ON o.customer_id = c.customer_id
    AND o.order_status = 'OPEN';
```

### Beginner Translation

Indent `ON` so it visually belongs to the join.

---

## Prefer Explicit JOIN Syntax

Avoid old comma-style joins:

```sql
SELECT c.customer_id,
       o.order_id
FROM customer AS c,
     order_record AS o
WHERE o.customer_id = c.customer_id;
```

Prefer explicit joins:

```sql
SELECT c.customer_id,
       o.order_id
FROM customer AS c
JOIN order_record AS o
     ON o.customer_id = c.customer_id;
```

### Why Explicit JOIN Is Better

It separates:

```text
table connection logic → JOIN / ON
row filtering logic    → WHERE
```

That makes the query easier to reason about.

### Beginner Translation

Use `JOIN ... ON` to show how tables connect.

Use `WHERE` to filter rows.

---

## Format WHERE Conditions Clearly

For one condition:

```sql
SELECT customer_id,
       first_name,
       last_name
FROM customer
WHERE state = 'MD';
```

For multiple conditions:

```sql
SELECT customer_id,
       first_name,
       last_name
FROM customer
WHERE state = 'MD'
  AND city = 'Crofton'
  AND zip_code = '21114';
```

For `OR` conditions:

```sql
SELECT customer_id,
       first_name,
       last_name
FROM customer
WHERE state = 'MD'
   OR state = 'VA'
   OR state = 'DC';
```

### Beginner Translation

Put conditions on separate lines so each rule is visible.

---

## Use Parentheses With Mixed AND/OR

Bad or unclear:

```sql
SELECT customer_id,
       first_name,
       last_name
FROM customer
WHERE state = 'MD'
  AND city = 'Crofton'
   OR city = 'Odenton';
```

Better:

```sql
SELECT customer_id,
       first_name,
       last_name
FROM customer
WHERE state = 'MD'
  AND (
       city = 'Crofton'
       OR city = 'Odenton'
  );
```

### Beginner Translation

When logic gets mixed, parentheses make your intent visible.

Do not make the reader guess.

---

## Prefer IN Over Repeated OR

Instead of:

```sql
SELECT customer_id,
       first_name,
       last_name
FROM customer
WHERE state = 'MD'
   OR state = 'VA'
   OR state = 'DC';
```

Prefer:

```sql
SELECT customer_id,
       first_name,
       last_name
FROM customer
WHERE state IN ('MD', 'VA', 'DC');
```

This is shorter and easier to scan.

### Beginner Translation

`IN` means:

```text
match anything in this list
```

---

## Prefer BETWEEN for Clear Ranges

Instead of:

```sql
SELECT product_id,
       product_name,
       price
FROM product
WHERE price >= 10.00
  AND price <= 50.00;
```

You can write:

```sql
SELECT product_id,
       product_name,
       price
FROM product
WHERE price BETWEEN 10.00 AND 50.00;
```

### Beginner Warning

Check your database behavior and your data type.

`BETWEEN` is commonly inclusive, meaning it includes the endpoints.

---

## Format CASE Expressions

A `CASE` expression can become hard to read if kept on one line.

Bad:

```sql
SELECT order_id, CASE WHEN total >= 100 THEN 'large' ELSE 'standard' END AS order_size FROM order_record;
```

Better:

```sql
SELECT order_id,
       CASE
           WHEN total >= 100 THEN 'large'
           ELSE 'standard'
       END AS order_size
FROM order_record;
```

### Beginner Translation

A `CASE` expression is a mini decision tree.

Format it like one.

---

## Format GROUP BY and HAVING

Example:

```sql
SELECT state,
       COUNT(*) AS customer_count
FROM customer
GROUP BY state
HAVING COUNT(*) > 1
ORDER BY customer_count DESC;
```

When grouping by multiple columns:

```sql
SELECT state,
       city,
       COUNT(*) AS customer_count
FROM customer
GROUP BY state,
         city
HAVING COUNT(*) > 1
ORDER BY state,
         city;
```

### Beginner Translation

Group columns should be visible as their own list.

---

## Format Subqueries

A subquery is a query inside another query.

Bad:

```sql
SELECT customer_id, first_name, last_name FROM customer WHERE customer_id IN (SELECT customer_id FROM order_record WHERE order_date >= '2026-01-01');
```

Better:

```sql
SELECT customer_id,
       first_name,
       last_name
FROM customer
WHERE customer_id IN (
    SELECT customer_id
    FROM order_record
    WHERE order_date >= '2026-01-01'
);
```

### Beginner Translation

A subquery is a nested question.

Indent it so the nesting is visible.

---

## Format INSERT Statements

For one row:

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

For multiple rows:

```sql
INSERT INTO customer (
    customer_id,
    first_name,
    last_name,
    state
)
VALUES
    ('C001', 'Jane', 'Smith', 'MD'),
    ('C002', 'Robert', 'Jones', 'VA'),
    ('C003', 'Maria', 'Garcia', 'DC');
```

### Beginner Rule

Always list the columns in an `INSERT`.

Avoid:

```sql
INSERT INTO customer
VALUES ('C001', 'Jane', 'Smith', 'MD');
```

Why?

Because this relies on table column order.

If the table changes, your insert can break or put values in the wrong place.

---

## Format UPDATE Statements

```sql
UPDATE customer
SET email = 'jane.smith@example.com',
    phone = '555-9999'
WHERE customer_id = 'C001';
```

### Beginner Warning

Be very careful with `UPDATE`.

Without a `WHERE` clause, you may update every row.

Dangerous:

```sql
UPDATE customer
SET state = 'MD';
```

This changes all customers.

Usually you want:

```sql
UPDATE customer
SET state = 'MD'
WHERE customer_id = 'C001';
```

---

## Format DELETE Statements

```sql
DELETE FROM customer
WHERE customer_id = 'C001';
```

### Beginner Warning

Be very careful with `DELETE`.

Without a `WHERE` clause, you may delete every row.

Dangerous:

```sql
DELETE FROM customer;
```

This deletes all rows from the table.

Formatting cannot save you by itself, but clear formatting makes danger easier to notice.

---

## Avoid SELECT * in Production Code

`SELECT *` means all columns.

It is useful for quick exploration:

```sql
SELECT *
FROM customer;
```

But in production, prefer listing columns:

```sql
SELECT customer_id,
       first_name,
       last_name,
       email
FROM customer;
```

### Why Avoid SELECT *

```text
It retrieves unnecessary data.
It can break code when table structure changes.
It makes queries less self-documenting.
It may expose columns you did not intend to use.
It makes performance harder to reason about.
```

### Beginner Translation

`SELECT *` is like saying:

```text
Bring me the whole filing cabinet.
```

Professional SQL usually says:

```text
Bring me these three folders.
```

---

## Select Only What You Need

Do not retrieve extra columns “just in case.”

Bad:

```sql
SELECT *
FROM customer;
```

Better:

```sql
SELECT customer_id,
       first_name,
       last_name
FROM customer;
```

This is clearer and often more efficient.

### Beginner Translation

Good SQL is specific.

Specific queries are easier to understand and maintain.

---

## Use Semicolons

End SQL statements with semicolons.

```sql
SELECT customer_id,
       first_name,
       last_name
FROM customer;
```

Some tools may not require it for single statements.

But using semicolons consistently avoids trouble, especially when running multiple statements.

### Beginner Translation

A semicolon tells SQL:

```text
This statement is finished.
```

---

## Comments

Comments explain why something is happening.

They should not explain the obvious.

Bad comment:

```sql
-- Select customer ID
SELECT customer_id
FROM customer;
```

That comment adds nothing.

Better comment:

```sql
/* Only active customers are included in the renewal campaign. */
SELECT customer_id,
       first_name,
       last_name
FROM customer
WHERE account_status = 'ACTIVE';
```

### Comment Types

Single-line comment:

```sql
-- This is a comment.
```

Block comment:

```sql
/*
This is a longer comment.
It can span multiple lines.
*/
```

### Beginner Translation

Use comments to explain intent, not syntax.

---

## Respect Data Types

Do not compare values as if they are a different type.

Example:

```sql
WHERE order_date >= DATE '2026-01-01'
```

is clearer than treating dates like random strings when your database supports date literals.

Also remember:

```text
Numbers are for math.
Text is for labels, names, codes, and identifiers.
Dates are for dates.
```

### Beginner Example

ZIP code should often be text, not a number.

Why?

```text
ZIP codes can start with zero.
You do not do math on ZIP codes.
```

---

## Avoid Implicit Conversions

An implicit conversion happens when the database guesses how to convert one type to another.

Example:

```sql
WHERE product_id = 123
```

But `product_id` is stored as text.

Better:

```sql
WHERE product_id = '123'
```

### Beginner Translation

Do not make the database guess.

Be clear about the type of value you are using.

---

## Keep Queries Succinct

Avoid unnecessary clutter.

Bad:

```sql
SELECT customer_id,
       first_name,
       last_name
FROM customer
WHERE 1 = 1
  AND state = 'MD';
```

Better:

```sql
SELECT customer_id,
       first_name,
       last_name
FROM customer
WHERE state = 'MD';
```

`WHERE 1 = 1` is sometimes used in generated SQL, but beginners should not use it as a habit in hand-written queries.

---

## Use Standard SQL When Possible

Different database systems have vendor-specific features.

Examples:

```text
Oracle
PostgreSQL
MySQL
SQL Server
SQLite
```

When possible, use standard SQL features that work across systems.

This makes your SQL more portable.

### Beginner Translation

Vendor-specific features are sometimes useful.

But do not use special tools when a standard tool does the job clearly.

---

## Full Example: Poorly Formatted to Clean

### Poor Version

```sql
select c.customer_id,c.first_name,c.last_name,o.order_id,o.order_date from customer c join order_record o on o.customer_id=c.customer_id where c.state='MD' and o.order_date>='2026-01-01' order by o.order_date desc;
```

### Clean Version

```sql
SELECT c.customer_id,
       c.first_name,
       c.last_name,
       o.order_id,
       o.order_date
FROM customer AS c
JOIN order_record AS o
     ON o.customer_id = c.customer_id
WHERE c.state = 'MD'
  AND o.order_date >= '2026-01-01'
ORDER BY o.order_date DESC;
```

### What Improved?

```text
Keywords are visible.
Columns are listed clearly.
Tables and aliases are clear.
Join condition is visible.
Filters are easy to scan.
Sort order is obvious.
```

---

## Full Example With GROUP BY

```sql
SELECT c.state,
       COUNT(o.order_id) AS order_count
FROM customer AS c
JOIN order_record AS o
     ON o.customer_id = c.customer_id
WHERE o.order_date >= '2026-01-01'
GROUP BY c.state
HAVING COUNT(o.order_id) > 10
ORDER BY order_count DESC;
```

This query asks:

```text
For orders since 2026-01-01,
count orders by customer state,
only show states with more than 10 orders,
sort largest order count first.
```

Because it is formatted, the question is visible.

---

## Style Checklist

Use this checklist when writing SQL:

```text
1. Are SQL keywords uppercase?
2. Are table and column names consistent?
3. Are multi-word names separated with underscores?
4. Are selected columns listed explicitly?
5. Is each major clause on its own line?
6. Are multiple columns listed one per line?
7. Are aliases meaningful?
8. Is AS used for aliases?
9. Are JOIN conditions written with ON?
10. Are WHERE conditions easy to scan?
11. Are mixed AND/OR conditions grouped with parentheses?
12. Are IN and BETWEEN used where they improve clarity?
13. Are NULL checks written with IS NULL or IS NOT NULL?
14. Does the statement end with a semicolon?
15. Are comments meaningful?
16. Are data types respected?
17. Is the query selecting only what it needs?
```

---

## Common Beginner Mistakes

### Mistake 1: Writing Everything on One Line

One-line SQL becomes painful quickly.

### Mistake 2: Random Capitalization

Bad:

```sql
Select FirstName FROM CUSTOMER where State='MD';
```

Pick a consistent style.

### Mistake 3: Vague Aliases

Avoid:

```sql
FROM customer AS x
JOIN order_record AS y
```

Prefer:

```sql
FROM customer AS c
JOIN order_record AS o
```

### Mistake 4: Using SELECT * Too Much

List the columns you need.

### Mistake 5: No Semicolon

Use semicolons consistently.

### Mistake 6: Comments That Repeat the Code

Bad:

```sql
-- Get customer ID
SELECT customer_id
FROM customer;
```

Better comments explain why.

### Mistake 7: WHERE Clause Hidden in a Long Line

Make filters visible.

This is especially important for `UPDATE` and `DELETE`.

---

## The “Road Signs” Analogy

Formatting is like road signs and lane markings.

The road still exists without them.

But driving becomes more dangerous.

SQL without formatting may still run.

But reading it becomes slower and riskier.

Good formatting gives the reader lanes, signs, and exits.

---

## Team Style Matters

When working alone, you may develop your own preferences.

When working with a team, consistency matters more than personal preference.

A team should decide:

```text
naming rules
capitalization
indentation
aliasing style
line length
comment style
JOIN formatting
INSERT/UPDATE/DELETE formatting
```

Then write it down and follow it.

### Beginner Translation

Style is not just personal taste.

In a team, style is a shared contract.

---

## Key Terms

| Term | Meaning |
|---|---|
| Formatting | Visual layout of code |
| Style guide | Agreed rules for writing code |
| Identifier | Name of a database object like table or column |
| Alias | Temporary name used in a query |
| Keyword | SQL command word like SELECT or WHERE |
| Clause | Section of a SQL statement |
| JOIN condition | Rule that connects tables |
| Comment | Text ignored by SQL, used to explain intent |
| Semicolon | Statement terminator |
| Implicit conversion | Database guesses how to convert a value type |

---

## Quick Check

Answer these in your own words:

1. Why does SQL formatting matter?
2. Why should SQL keywords often be uppercase?
3. Why are vague names like `id` and `name` risky?
4. Why should aliases be meaningful?
5. Why should major clauses be placed on separate lines?
6. Why should you avoid `SELECT *` in production code?
7. Why is explicit `JOIN ... ON` better than comma joins?
8. Why should mixed `AND` and `OR` logic use parentheses?
9. What should comments explain?
10. Why should `UPDATE` and `DELETE` statements be formatted carefully?

---

## Practice Exercise

Reformat this query:

```sql
select c.customer_id,c.first_name,c.last_name,o.order_id,o.order_date from customer c left join order_record o on o.customer_id=c.customer_id where c.state='MD' or c.state='VA' order by c.last_name,o.order_date desc
```

Suggested clean structure:

```sql
SELECT c.customer_id,
       c.first_name,
       c.last_name,
       o.order_id,
       o.order_date
FROM customer AS c
LEFT JOIN order_record AS o
       ON o.customer_id = c.customer_id
WHERE c.state IN ('MD', 'VA')
ORDER BY c.last_name,
         o.order_date DESC;
```

Now answer:

```text
1. What changed visually?
2. Why is IN cleaner than repeated OR here?
3. What does LEFT JOIN preserve?
4. Which table aliases were used?
5. Why is the ORDER BY easier to read now?
```

---

## Summary

SQL formatting is not about making code pretty.

It is about making code readable, reviewable, debuggable, and maintainable.

The beginner rules are:

```text
Use consistent naming.
Use uppercase SQL keywords.
Use lowercase identifiers.
Use underscores.
List columns clearly.
Avoid SELECT * in production code.
Use meaningful aliases.
Use explicit JOIN syntax.
Put major clauses on separate lines.
Make conditions visible.
Use comments for intent.
End statements with semicolons.
```

Good formatting lets another human see the structure of your question.

And in real software, that matters.

---

## Source Notes

This lesson was drafted from the uploaded SQL style and formatting materials, especially the readings on SQL formatting standards, naming conventions, aliases, indentation, whitespace, comments, SELECT formatting, JOIN formatting, avoiding SELECT *, semicolons, data type respect, and team coding standards.
