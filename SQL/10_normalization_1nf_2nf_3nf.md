# 10 — Normalization: 1NF, 2NF, and 3NF

## Big Idea

Normalization is the process of organizing database tables so that data is stored in the right place, with as little unnecessary repetition as possible.

The goal is not to make the design look fancy.

The goal is to prevent data problems.

Normalization helps avoid:

```text
repeated data
conflicting data
missing data problems
accidental data loss
painful updates
```

For beginners, the easiest way to understand normalization is this:

```text
Each fact should live in one clear place.
```

When the same fact is copied all over the database, the database becomes harder to trust.

---

## Why Normalization Exists

Imagine this table:

| order_id | order_date | customer_id | customer_name | customer_phone | product_name | product_price |
|---|---|---|---|---|---|---|
| O1001 | 2026-05-01 | C001 | Jane Smith | 555-1111 | Laptop | 999.99 |
| O1002 | 2026-05-02 | C001 | Jane Smith | 555-1111 | Mouse | 25.00 |
| O1003 | 2026-05-03 | C001 | Jane Smith | 555-1111 | Keyboard | 75.00 |

Jane Smith's name and phone number are repeated on every order.

That may not look bad with three rows.

But what about:

```text
100 orders?
10,000 orders?
1,000,000 orders?
```

Now imagine Jane changes her phone number.

Every row with Jane's old phone number has to be updated.

If one row is missed, the database disagrees with itself.

That is the problem normalization tries to prevent.

---

## The Core Problem: Repetition Creates Risk

Repeated data causes three major kinds of problems:

```text
insertion anomalies
deletion anomalies
modification anomalies
```

These are called update anomalies.

They are signs that too many different facts have been packed into one table.

---

## Insertion Anomaly

An **insertion anomaly** happens when you cannot add one fact without also adding an unrelated fact.

Example:

```text
You want to add a new customer.
But the table only stores customers inside orders.
So you cannot add the customer until they place an order.
```

Bad table:

| order_id | customer_id | customer_name | customer_phone |
|---|---|---|---|
| O1001 | C001 | Jane Smith | 555-1111 |

Where do you put a new customer who has not ordered yet?

You may be forced to insert fake order data or leave strange blanks.

That is bad design.

### Beginner Translation

Insertion anomaly means:

```text
I cannot store this fact until some other unrelated fact exists.
```

---

## Deletion Anomaly

A **deletion anomaly** happens when deleting one fact accidentally deletes another fact you wanted to keep.

Example:

```text
You delete the only order for a customer.
Now you also lose the only record of that customer.
```

Bad table:

| order_id | customer_id | customer_name | customer_phone |
|---|---|---|---|
| O1001 | C001 | Jane Smith | 555-1111 |

If order `O1001` is deleted, Jane Smith disappears too.

But maybe Jane is still a customer.

### Beginner Translation

Deletion anomaly means:

```text
I deleted one thing and accidentally lost another thing.
```

---

## Modification Anomaly

A **modification anomaly** happens when one fact is stored in many places and must be updated in all of them.

Example:

| order_id | customer_id | customer_name | customer_phone |
|---|---|---|---|
| O1001 | C001 | Jane Smith | 555-1111 |
| O1002 | C001 | Jane Smith | 555-1111 |
| O1003 | C001 | Jane Smith | 555-1111 |

Jane changes her phone number to:

```text
555-9999
```

Every row must be updated.

If one row is missed:

| order_id | customer_id | customer_name | customer_phone |
|---|---|---|---|
| O1001 | C001 | Jane Smith | 555-9999 |
| O1002 | C001 | Jane Smith | 555-1111 |
| O1003 | C001 | Jane Smith | 555-9999 |

Now the database contains conflicting data.

### Beginner Translation

Modification anomaly means:

```text
I changed a fact in one place, but old copies still exist somewhere else.
```

---

## The Normalization Mental Model

Normalization asks:

```text
What is this table really about?
Does every column describe that one thing?
Are we repeating facts?
Can one fact change without updating many rows?
Can we add facts independently?
Can we delete facts safely?
```

A healthy table should usually be about one subject.

Examples:

```text
customer facts go in customer
order facts go in order_record
product facts go in product
line item facts go in order_line
```

---

## Normal Forms

Normalization is often taught in stages called **normal forms**.

Common beginner stages:

```text
First Normal Form  = 1NF
Second Normal Form = 2NF
Third Normal Form  = 3NF
```

There are higher normal forms, but 1NF, 2NF, and 3NF are the beginner foundation.

The progression is:

```text
1NF: Make values atomic. Remove repeating groups.
2NF: Remove partial dependency on part of a composite key.
3NF: Remove transitive dependency between non-key columns.
```

That sounds abstract.

We will unpack each one slowly.

---

## Functional Dependency

Before 2NF and 3NF make sense, we need one key idea:

```text
functional dependency
```

A **functional dependency** means one attribute determines another.

Example:

```text
customer_id → customer_name
```

This means:

```text
If you know the customer_id,
you can determine the customer_name.
```

Another example:

```text
zip_code → city, state
```

This means:

```text
If you know the zip_code,
you may be able to determine city and state.
```

### Beginner Translation

Functional dependency means:

```text
This value tells me that value.
```

Or:

```text
The left side determines the right side.
```

---

## Determinant and Dependent

In this dependency:

```text
customer_id → customer_name
```

`customer_id` is the **determinant**.

`customer_name` is the **dependent**.

Why?

Because `customer_id` determines `customer_name`.

### Beginner Translation

```text
determinant = the thing you know
dependent   = the thing it tells you
```

---

## First Normal Form: 1NF

A table is in **First Normal Form**, or **1NF**, when each cell contains a single atomic value and there are no repeating groups.

### Rule

```text
Each row-column intersection should contain one value.
No lists inside a cell.
No repeating groups of columns.
```

---

## 1NF Problem: Lists Inside a Cell

Bad table:

| customer_id | customer_name | phone_numbers |
|---|---|---|
| C001 | Jane Smith | 555-1111, 555-2222 |
| C002 | Robert Jones | 555-3333 |

The `phone_numbers` column contains a list.

That is not atomic.

### Why This Is a Problem

How do you search for one phone number?

How do you update only one phone number?

How do you label one as home and one as mobile?

How do you enforce phone number format?

The list makes everything harder.

---

## 1NF Fix: Move Repeating Values to Rows

Better:

### customer

| customer_id | customer_name |
|---|---|
| C001 | Jane Smith |
| C002 | Robert Jones |

### customer_phone

| customer_id | phone_number | phone_type |
|---|---|---|
| C001 | 555-1111 | home |
| C001 | 555-2222 | mobile |
| C002 | 555-3333 | home |

Now each cell has one value.

Each phone number gets its own row.

### Beginner Translation

1NF says:

```text
Do not hide a list inside a cell.
Give each value its own place.
```

---

## 1NF Problem: Repeating Groups of Columns

Bad table:

| customer_id | customer_name | phone_1 | phone_2 | phone_3 |
|---|---|---|---|---|
| C001 | Jane Smith | 555-1111 | 555-2222 | NULL |
| C002 | Robert Jones | 555-3333 | NULL | NULL |

This avoids a comma-separated list, but it creates repeating columns.

What happens if a customer has four phone numbers?

Add `phone_4`?

Then `phone_5`?

That design does not scale.

### 1NF Fix

Use a related table:

| customer_id | phone_number | phone_type |
|---|---|---|
| C001 | 555-1111 | home |
| C001 | 555-2222 | mobile |
| C002 | 555-3333 | home |

### Beginner Translation

Repeating columns are usually a sign that you need another table.

---

## First Normal Form Checklist

A table is likely in 1NF if:

```text
Each cell has one value.
There are no comma-separated lists.
There are no repeated column groups like phone_1, phone_2, phone_3.
Each row can be identified.
Each column stores one kind of fact.
```

---

## Second Normal Form: 2NF

A table is in **Second Normal Form**, or **2NF**, when:

```text
It is already in 1NF.
Every non-key column depends on the whole primary key.
```

2NF matters most when a table has a **composite primary key**.

A composite key is a key made of more than one column.

Example:

```text
student_id + course_id
```

---

## 2NF Problem: Partial Dependency

A **partial dependency** happens when a non-key column depends on only part of a composite key.

Example table:

| student_id | course_id | student_name | course_name | grade |
|---|---|---|---|---|
| S001 | C101 | Jane Smith | SQL Basics | A |
| S001 | C205 | Jane Smith | Web Dev | B |
| S002 | C101 | Robert Jones | SQL Basics | A |

Primary key:

```text
student_id + course_id
```

This combination identifies one enrollment.

But look at the columns:

```text
student_name depends only on student_id
course_name depends only on course_id
grade depends on student_id + course_id
```

That means:

```text
student_id → student_name
course_id → course_name
student_id + course_id → grade
```

`student_name` and `course_name` do not depend on the whole key.

They depend on part of it.

That violates 2NF.

---

## Why Partial Dependency Is a Problem

Jane Smith appears more than once.

SQL Basics appears more than once.

If Jane changes her name, multiple rows must be updated.

If the course name changes, multiple rows must be updated.

This causes modification anomalies.

---

## 2NF Fix: Split Into Separate Tables

Better design:

### student

| student_id | student_name |
|---|---|
| S001 | Jane Smith |
| S002 | Robert Jones |

### course

| course_id | course_name |
|---|---|
| C101 | SQL Basics |
| C205 | Web Dev |

### enrollment

| student_id | course_id | grade |
|---|---|---|
| S001 | C101 | A |
| S001 | C205 | B |
| S002 | C101 | A |

Now:

```text
student_name depends on student_id.
course_name depends on course_id.
grade depends on student_id + course_id.
```

Each fact lives in the correct table.

### Beginner Translation

2NF says:

```text
If the key has multiple parts,
every non-key column must need all parts of the key.
```

---

## 2NF Is Automatic With Single-Column Primary Keys?

Often, yes.

If a table has a single-column primary key, there cannot be a partial dependency on part of a composite key because there is no composite key.

But that does not mean the table is automatically well-designed.

It may still violate 3NF.

### Beginner Translation

2NF mainly catches problems in tables with combined keys.

---

## Second Normal Form Checklist

A table is likely in 2NF if:

```text
It is already in 1NF.
If the primary key is composite, every non-key column depends on the whole key.
No non-key column depends on only part of the composite key.
Facts about one entity are not repeated in a relationship table.
```

Ask:

```text
Does this column describe the whole row,
or only one part of the key?
```

---

## Third Normal Form: 3NF

A table is in **Third Normal Form**, or **3NF**, when:

```text
It is already in 2NF.
No non-key column depends on another non-key column.
```

This is about removing **transitive dependencies**.

---

## 3NF Problem: Transitive Dependency

A **transitive dependency** happens when:

```text
primary key → non-key column → another non-key column
```

Example table:

| customer_id | customer_name | zip_code | city | state |
|---|---|---|---|---|
| C001 | Jane Smith | 21114 | Crofton | MD |
| C002 | Robert Jones | 21114 | Crofton | MD |
| C003 | Maria Garcia | 21113 | Odenton | MD |

Primary key:

```text
customer_id
```

Dependencies:

```text
customer_id → customer_name, zip_code
zip_code → city, state
```

That means:

```text
customer_id → zip_code → city, state
```

`city` and `state` depend on `zip_code`, not directly on `customer_id`.

That is a transitive dependency.

---

## Why Transitive Dependency Is a Problem

Crofton and MD are repeated for every customer in ZIP code `21114`.

If a ZIP code mapping changes or is corrected, many rows must be updated.

If one row says:

```text
21114, Crofton, MD
```

and another says:

```text
21114, Croftin, MD
```

now the database disagrees.

---

## 3NF Fix: Split the Dependent Fact

Better design:

### customer

| customer_id | customer_name | zip_code |
|---|---|---|
| C001 | Jane Smith | 21114 |
| C002 | Robert Jones | 21114 |
| C003 | Maria Garcia | 21113 |

### zip_code

| zip_code | city | state |
|---|---|---|
| 21114 | Crofton | MD |
| 21113 | Odenton | MD |

Now:

```text
customer_id → customer_name, zip_code
zip_code → city, state
```

The ZIP code facts live in the ZIP code table.

### Beginner Translation

3NF says:

```text
Non-key columns should describe the key,
not each other.
```

---

## The Classic 3NF Phrase

A common phrase for 3NF is:

```text
Every non-key attribute must depend on the key,
the whole key,
and nothing but the key.
```

Break it down:

```text
the key        → 1NF/identity idea
the whole key  → 2NF
nothing but the key → 3NF
```

### Beginner Translation

Every column should answer a question about the row's identity.

If a column answers a question about another non-key column, it probably belongs somewhere else.

---

## Normalization Walkthrough: Bad Orders Table

Start with this table:

| order_id | order_date | customer_id | customer_name | customer_phone | product_id | product_name | unit_price | quantity |
|---|---|---|---|---|---|---|---|---|
| O1001 | 2026-05-01 | C001 | Jane Smith | 555-1111 | P10 | Laptop | 999.99 | 1 |
| O1001 | 2026-05-01 | C001 | Jane Smith | 555-1111 | P20 | Mouse | 25.00 | 2 |
| O1002 | 2026-05-02 | C001 | Jane Smith | 555-1111 | P30 | Keyboard | 75.00 | 1 |

What is wrong?

```text
Customer data repeats.
Product data repeats.
Order data repeats.
Line item data is mixed with order data.
```

---

## Step 1: Identify the Real Subjects

The table contains facts about:

```text
customers
orders
products
order lines
```

That suggests separate tables:

```text
customer
order_record
product
order_line
```

---

## Step 2: Customer Table

Customer facts:

```text
customer_id
customer_name
customer_phone
```

Table:

| customer_id | customer_name | customer_phone |
|---|---|---|
| C001 | Jane Smith | 555-1111 |

---

## Step 3: Product Table

Product facts:

```text
product_id
product_name
unit_price
```

Table:

| product_id | product_name | unit_price |
|---|---|---|
| P10 | Laptop | 999.99 |
| P20 | Mouse | 25.00 |
| P30 | Keyboard | 75.00 |

---

## Step 4: Order Table

Order facts:

```text
order_id
order_date
customer_id
```

Table:

| order_id | order_date | customer_id |
|---|---|---|
| O1001 | 2026-05-01 | C001 |
| O1002 | 2026-05-02 | C001 |

The order points to the customer.

---

## Step 5: Order Line Table

Line item facts:

```text
order_id
line_number
product_id
quantity
unit_price_at_sale
```

Table:

| order_id | line_number | product_id | quantity | unit_price_at_sale |
|---|---|---|---|---|
| O1001 | 1 | P10 | 1 | 999.99 |
| O1001 | 2 | P20 | 2 | 25.00 |
| O1002 | 1 | P30 | 1 | 75.00 |

Why store `unit_price_at_sale` here?

Because product prices may change later.

The order line preserves the historical price charged at the time of sale.

---

## Final Normalized Design

```text
customer
- customer_id
- customer_name
- customer_phone

product
- product_id
- product_name
- current_price

order_record
- order_id
- order_date
- customer_id

order_line
- order_id
- line_number
- product_id
- quantity
- unit_price_at_sale
```

Now each table has a clearer purpose.

```text
customer stores customer facts
product stores product facts
order_record stores order header facts
order_line stores item-level order facts
```

---

## Normalization and Foreign Keys

Normalization often creates more tables.

Foreign keys reconnect them.

Example:

```text
order_record.customer_id → customer.customer_id
order_line.order_id → order_record.order_id
order_line.product_id → product.product_id
```

### Beginner Translation

Normalization separates facts.

Foreign keys preserve relationships.

---

## Does Normalization Always Mean More Tables?

Often, yes.

But more tables are not automatically bad.

The goal is not “more tables.”

The goal is:

```text
one clear place for each fact
```

Sometimes one table is enough.

Sometimes splitting a table makes the design clearer and safer.

---

## Can a Database Be Too Normalized?

Yes.

In real systems, designers sometimes denormalize for performance or reporting.

**Denormalization** means intentionally storing repeated data.

But beginners should understand normalization first.

Why?

Because denormalization should be a deliberate engineering choice, not an accident.

### Beginner Translation

First learn how to avoid repetition.

Later, learn when repetition is worth the tradeoff.

---

## Normalization vs. Query Simplicity

A normalized design may require joins.

Example:

```sql
SELECT o.order_id,
       o.order_date,
       c.customer_name,
       ol.product_id,
       p.product_name,
       ol.quantity
FROM order_record AS o
JOIN customer AS c
     ON c.customer_id = o.customer_id
JOIN order_line AS ol
     ON ol.order_id = o.order_id
JOIN product AS p
     ON p.product_id = ol.product_id;
```

This query is longer than selecting from one giant table.

But the data is safer.

### Beginner Translation

Normalization can make queries longer, but it makes stored facts cleaner.

---

## Normalization and Real-World Judgment

Normalization is not just a mechanical checklist.

You also need to understand the business meaning.

Example:

Should `unit_price` be stored in `product` or `order_line`?

Answer:

```text
Both may be needed, but they mean different things.
```

`product.current_price` means:

```text
What is the price now?
```

`order_line.unit_price_at_sale` means:

```text
What price was charged on this order?
```

Those are different facts.

So storing both can be correct.

### Beginner Translation

Normalization does not mean deleting useful history.

It means putting each fact where it belongs.

---

## 1NF vs. 2NF vs. 3NF Summary

| Normal Form | Main Question | Problem It Fixes |
|---|---|---|
| 1NF | Are values atomic? | Lists/repeating groups |
| 2NF | Does every non-key column depend on the whole key? | Partial dependencies |
| 3NF | Does every non-key column depend only on the key? | Transitive dependencies |

---

## Quick Examples

### Violates 1NF

```text
customer(phone_numbers)
C001: 555-1111, 555-2222
```

Fix:

```text
customer_phone(customer_id, phone_number)
```

---

### Violates 2NF

```text
enrollment(student_id, course_id, student_name, course_name, grade)
```

Key:

```text
student_id + course_id
```

Problem:

```text
student_name depends only on student_id
course_name depends only on course_id
```

Fix:

```text
student(student_id, student_name)
course(course_id, course_name)
enrollment(student_id, course_id, grade)
```

---

### Violates 3NF

```text
customer(customer_id, customer_name, zip_code, city, state)
```

Problem:

```text
zip_code → city, state
```

Fix:

```text
customer(customer_id, customer_name, zip_code)
zip_code(zip_code, city, state)
```

---

## Common Beginner Mistakes

### Mistake 1: Thinking Normalization Is Just Splitting Tables

Normalization is not random splitting.

It is splitting based on dependencies and meaning.

### Mistake 2: Leaving Lists in Cells

Comma-separated values inside one column are a major warning sign.

### Mistake 3: Ignoring Composite Keys

2NF only makes sense if you understand composite keys.

### Mistake 4: Keeping Lookup Facts in the Wrong Table

If one non-key column determines another non-key column, consider separating it.

### Mistake 5: Removing Historical Facts

Do not remove facts that describe the event at the time it happened.

Example:

```text
unit_price_at_sale
```

belongs on the order line even if product has a current price.

### Mistake 6: Believing Normalized Means Perfect

Normalization helps, but good design still needs business understanding, constraints, indexes, and testing.

---

## Normalization Checklist

Use this checklist:

```text
1. What is this table about?
2. What is the primary key?
3. Does each cell contain one atomic value?
4. Are there repeating groups of columns?
5. Does every non-key column depend on the key?
6. If the key is composite, does every non-key column depend on the whole key?
7. Do any non-key columns depend on other non-key columns?
8. Is the same fact repeated in many rows?
9. Can I insert one kind of fact without fake unrelated data?
10. Can I delete one fact without accidentally deleting another?
11. Can I update one fact in one place?
12. Are foreign keys reconnecting separated facts correctly?
```

---

## The “One Fact, One Home” Analogy

Think of each fact as a tool in a workshop.

If the same tool is copied into ten drawers, you may not know which one is correct.

If one breaks or changes, you have to update all ten.

Normalization gives each fact a home.

Then other tables point to that home when they need it.

```text
customer phone lives with customer
product name lives with product
order date lives with order
quantity lives with order line
```

---

## Key Terms

| Term | Meaning |
|---|---|
| Normalization | Organizing tables to reduce redundancy and anomalies |
| Redundancy | Repeating the same fact unnecessarily |
| Update anomaly | Problem caused by redundant or poorly placed data |
| Insertion anomaly | Cannot add a fact without unrelated data |
| Deletion anomaly | Deleting one fact accidentally removes another |
| Modification anomaly | Must update repeated copies of the same fact |
| Functional dependency | One attribute determines another |
| Determinant | Attribute that determines another |
| Dependent | Attribute being determined |
| 1NF | Atomic values, no repeating groups |
| 2NF | No partial dependency on a composite key |
| 3NF | No transitive dependency between non-key columns |
| Partial dependency | Column depends on part of a composite key |
| Transitive dependency | Non-key column depends on another non-key column |
| Denormalization | Intentional repetition for performance or convenience |

---

## Quick Check

Answer these in your own words:

1. What is normalization?
2. Why is repeated data dangerous?
3. What is an insertion anomaly?
4. What is a deletion anomaly?
5. What is a modification anomaly?
6. What does 1NF require?
7. What is a functional dependency?
8. What is a partial dependency?
9. Why does 2NF matter most with composite keys?
10. What is a transitive dependency?
11. What does 3NF remove?
12. Why might `unit_price_at_sale` belong in `order_line`?

---

## Practice Exercise

Normalize this table to 3NF:

| student_id | student_name | course_id | course_name | instructor_id | instructor_name | grade |
|---|---|---|---|---|---|---|
| S001 | Jane Smith | C101 | SQL Basics | I10 | Dr. Brown | A |
| S001 | Jane Smith | C205 | Web Dev | I20 | Prof. Green | B |
| S002 | Robert Jones | C101 | SQL Basics | I10 | Dr. Brown | A |

Assume:

```text
student_id determines student_name
course_id determines course_name and instructor_id
instructor_id determines instructor_name
student_id + course_id determines grade
```

Questions:

```text
1. What is the composite key in the original table?
2. Which columns depend only on student_id?
3. Which columns depend only on course_id?
4. Which column depends on instructor_id?
5. What tables would you create?
6. What are the primary keys?
7. What foreign keys are needed?
```

Possible final tables:

```text
student(student_id, student_name)

instructor(instructor_id, instructor_name)

course(course_id, course_name, instructor_id)

enrollment(student_id, course_id, grade)
```

---

## Summary

Normalization helps keep database facts clean, reliable, and easy to maintain.

The beginner path is:

```text
1NF: no lists, no repeating groups
2NF: every non-key column depends on the whole key
3NF: non-key columns do not depend on other non-key columns
```

The deeper idea is:

```text
Put each fact in the place where it belongs.
Store it once when possible.
Reconnect facts with keys.
```

A normalized database is not just organized.

It is harder to corrupt by accident.

---

## Source Notes

This lesson was drafted from the uploaded normalization and relational database materials, especially the readings on functional dependencies, update anomalies, insertion anomalies, deletion anomalies, modification anomalies, first normal form, second normal form, third normal form, composite keys, partial dependencies, and transitive dependencies.
