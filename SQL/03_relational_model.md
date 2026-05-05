# 03 — The Relational Model: Tables, Rows, Columns, and Rules

## Big Idea

The relational model is the foundation behind relational databases.

When beginners first hear “relational database,” they often think it means:

```text
A database where tables are related to each other.
```

That is partly true.

But the deeper idea is this:

```text
A relational database organizes data into relations,
and those relations follow rules.
```

In everyday database language, we usually say:

```text
table
row
column
```

In the formal relational model, the matching terms are:

```text
relation
tuple
attribute
```

The words are different because the relational model comes from mathematics.

But for beginners, we can start with the practical version:

```text
A table stores one kind of thing.
A row stores one record of that thing.
A column stores one detail about that thing.
```

---

## Why the Relational Model Matters

The relational model gives databases structure.

Instead of storing everything in one giant file, we divide information into tables that each represent one clear subject.

Example:

```text
customer
order
product
employee
course
student
```

Each table has columns.

Each row in the table represents one specific instance.

So a `customer` table might look like this:

| customer_id | first_name | last_name | state |
|---|---|---|---|
| C001 | Jane | Smith | MD |
| C002 | Robert | Jones | VA |
| C003 | Maria | Garcia | DC |

In plain English:

```text
The table is CUSTOMER.
Each row is one customer.
Each column is one detail about a customer.
```

That is the basic shape of relational thinking.

---

## Informal Terms vs. Formal Terms

You will see two sets of vocabulary.

Most developers say:

| Informal Database Term | Formal Relational Term |
|---|---|
| Table | Relation |
| Row | Tuple |
| Column | Attribute |

Both vocabularies describe similar ideas, but they are not always perfectly identical.

For beginners, the practical terms are easier.

But the formal terms matter because documentation, textbooks, database theory, and normalization often use them.

### Beginner Translation

```text
Table    = relation
Row      = tuple
Column   = attribute
```

Or even simpler:

```text
A relation is a table-shaped structure.
A tuple is one row in that structure.
An attribute is one named column/detail.
```

---

## What Is a Relation?

A **relation** is the formal version of a table.

It has two major parts:

```text
1. A heading
2. A body
```

The **heading** defines the attributes.

The **body** contains the tuples.

Example:

```text
customer(customer_id, first_name, last_name, state)
```

That heading says every customer record has these attributes:

```text
customer_id
first_name
last_name
state
```

The body contains the actual data:

| customer_id | first_name | last_name | state |
|---|---|---|---|
| C001 | Jane | Smith | MD |
| C002 | Robert | Jones | VA |

### Beginner Translation

The heading is the form layout.

The body is the stack of filled-out forms.

---

## What Is a Tuple?

A **tuple** is one row in a relation.

In a `customer` table, one tuple is one customer.

Example:

| customer_id | first_name | last_name | state |
|---|---|---|---|
| C001 | Jane | Smith | MD |

That row is one tuple.

### Beginner Translation

A tuple is one complete record.

Not one value.

Not one column.

One full row.

---

## What Is an Attribute?

An **attribute** is a named detail in a relation.

In this table:

| customer_id | first_name | last_name | state |
|---|---|---|---|

The attributes are:

```text
customer_id
first_name
last_name
state
```

Each attribute has a meaning.

Each attribute should store one kind of value.

### Beginner Translation

An attribute is a column with a job.

It answers one question about the row.

```text
What is the customer's ID?
What is the customer's first name?
What is the customer's last name?
What state does the customer live in?
```

---

## What Is a Domain?

A **domain** is the set of valid values for an attribute.

For example:

```text
state
```

might have a domain of valid two-letter state abbreviations:

```text
MD, VA, DC, PA, CA, TX, ...
```

An `age` column might have a domain like:

```text
whole numbers greater than or equal to 0
```

A `price` column might have a domain like:

```text
decimal numbers greater than or equal to 0
```

### Beginner Translation

A domain is the allowed value zone.

It tells the database:

```text
This kind of column is only supposed to hold this kind of value.
```

This matters because not every value belongs everywhere.

```text
"banana" should not go in an age column.
-50 should not go in a price column.
"Maryland" might not belong in a state column if the system expects "MD".
```

---

## Data Types vs. Domains

A data type is the technical storage category.

Examples:

```text
INTEGER
VARCHAR
CHAR
DATE
DECIMAL
BOOLEAN
```

A domain is the business meaning and allowed range.

Example:

```text
Data type: CHAR(2)
Domain: valid U.S. state abbreviation
```

That means `MD` is valid, but `XX` might not be.

Both are two characters.

Only one makes sense.

### Beginner Translation

A data type asks:

```text
What kind of storage is this?
```

A domain asks:

```text
What values are allowed to mean something here?
```

---

## Degree and Cardinality

Two important words in the relational model are **degree** and **cardinality**.

These can be confusing because “cardinality” is also used when talking about relationships in ERDs.

In the relational model:

```text
Degree = number of attributes/columns in a relation
Cardinality = number of tuples/rows in a relation
```

Example:

| customer_id | first_name | last_name | state |
|---|---|---|---|
| C001 | Jane | Smith | MD |
| C002 | Robert | Jones | VA |
| C003 | Maria | Garcia | DC |

This table has:

```text
Degree: 4
Cardinality: 3
```

Why?

It has 4 columns and 3 rows.

### Beginner Translation

```text
Degree = how wide the table is
Cardinality = how tall the table is
```

---

## Schema vs. State

A database has both structure and current data.

The **schema** is the design.

The **state** is the current contents.

### Schema

```text
customer(customer_id, first_name, last_name, state)
```

This says what the table should look like.

### State

| customer_id | first_name | last_name | state |
|---|---|---|---|
| C001 | Jane | Smith | MD |
| C002 | Robert | Jones | VA |

This is the current data inside the table.

If we add another customer, the schema may stay the same, but the state changes.

### Beginner Translation

Schema is the blueprint.

State is what is currently built inside it.

---

## Relation vs. Table: Why the Difference Matters

In everyday SQL work, people often say “table.”

But in pure relational theory, a relation has stricter rules.

A true relation:

```text
Does not care about row order.
Does not care about column order.
Does not allow duplicate tuples.
Stores atomic values.
```

A practical SQL table may behave a little differently depending on the DBMS.

For example, SQL tables can sometimes allow duplicate rows unless you define keys or constraints.

### Beginner Translation

A table is the practical tool.

A relation is the clean mathematical ideal.

The closer your table design is to the relational ideal, the healthier your database usually is.

---

## Atomic Values

The relational model expects values to be **atomic**.

Atomic means the value should not be split into multiple repeating pieces inside one cell.

Bad example:

| customer_id | customer_name | phone_numbers |
|---|---|---|
| C001 | Jane Smith | 555-1111, 555-2222, 555-3333 |

The `phone_numbers` column contains multiple values jammed into one cell.

That makes searching, updating, and enforcing rules harder.

Better:

| customer_id | first_name | last_name |
|---|---|---|
| C001 | Jane | Smith |

And then a separate table:

| customer_id | phone_number |
|---|---|
| C001 | 555-1111 |
| C001 | 555-2222 |
| C001 | 555-3333 |

### Beginner Translation

One cell should hold one meaningful value.

Do not stuff a list into a column just because it fits as text.

---

## Keys

A **key** identifies rows.

The most common key is a **primary key**.

A primary key uniquely identifies one row in a table.

Example:

| customer_id | first_name | last_name |
|---|---|---|
| C001 | Jane | Smith |
| C002 | Jane | Smith |

Both customers have the same name.

But they have different customer IDs.

That is why names usually make poor primary keys.

### Primary Key

```text
customer_id
```

This is the column we choose to identify each customer.

### Beginner Translation

A primary key is the database’s exact label for a row.

It says:

```text
This one.
Not another row that looks similar.
This exact one.
```

---

## Candidate Keys

A **candidate key** is any minimal set of attributes that could uniquely identify a row.

Example:

```text
student_id
email
```

If both are unique, either could be a candidate key.

The database designer chooses one as the primary key.

### Beginner Translation

Candidate keys are possible row identifiers.

The primary key is the one we officially choose.

---

## Composite Keys

A **composite key** uses more than one column.

Example:

| student_id | course_id | grade |
|---|---|---|
| S001 | C101 | A |
| S001 | C205 | B |
| S002 | C101 | A |

A single `student_id` is not enough to identify a grade row because one student can take many courses.

A single `course_id` is not enough because many students can take the same course.

Together, they identify the row:

```text
student_id + course_id
```

That is a composite key.

### Beginner Translation

A composite key is a lock that needs two keys turned together.

One column alone is not enough.

---

## Foreign Keys

A **foreign key** is a column that points to a primary key in another table.

Example:

```text
customer.customer_id
order.customer_id
```

The customer table owns the customer ID.

The order table uses that customer ID to say which customer placed the order.

### Customer Table

| customer_id | first_name | last_name |
|---|---|---|
| C001 | Jane | Smith |
| C002 | Robert | Jones |

### Order Table

| order_id | customer_id | order_date |
|---|---|---|
| O1001 | C001 | 2026-05-01 |
| O1002 | C001 | 2026-05-02 |
| O1003 | C002 | 2026-05-03 |

Here, `order.customer_id` is a foreign key.

It connects each order back to a real customer.

### Beginner Translation

A foreign key is a pointer with rules.

It says:

```text
This value must match something real in another table.
```

---

## Integrity Constraints

An **integrity constraint** is a rule that protects the correctness of the database.

Common constraints include:

| Constraint | What It Protects |
|---|---|
| PRIMARY KEY | Each row must be uniquely identifiable |
| FOREIGN KEY | References must point to real rows |
| NOT NULL | A column must have a value |
| UNIQUE | No duplicate values allowed |
| CHECK | Values must satisfy a rule |
| DEFAULT | Use a fallback value if none is provided |

Example:

```sql
CREATE TABLE customer (
    customer_id CHAR(8) PRIMARY KEY,
    first_name  VARCHAR(25) NOT NULL,
    last_name   VARCHAR(25) NOT NULL,
    state       CHAR(2)
);
```

This table says:

```text
Each customer must have a unique customer_id.
Each customer must have a first_name.
Each customer must have a last_name.
```

### Beginner Translation

Constraints are guardrails.

They stop bad data before it gets stored.

---

## Nulls

A **NULL** means the absence of a value.

It does not mean zero.

It does not mean an empty string.

It does not mean false.

It means:

```text
No value is stored here.
```

NULL can mean different real-world things:

```text
unknown
not provided
not applicable
missing
```

Example:

| customer_id | first_name | middle_name |
|---|---|---|
| C001 | Jane | NULL |

This does not mean Jane’s middle name is literally “NULL.”

It means the database does not have a middle name value stored.

### Beginner Warning

NULLs require special handling in SQL.

You usually do not test for NULL like this:

```sql
WHERE middle_name = NULL
```

You test like this:

```sql
WHERE middle_name IS NULL
```

### Beginner Translation

NULL is the database saying:

```text
There is no value here to compare.
```

---

## Relational Operators

The relational model is not just about structure.

It also includes operations for working with data.

SQL gives us practical ways to perform those operations.

Examples:

```sql
SELECT
FROM
WHERE
JOIN
GROUP BY
ORDER BY
```

These let us ask questions like:

```text
Which customers live in Maryland?
Which orders belong to customer C001?
How many orders were placed this month?
Which products have never been ordered?
```

The relational model gives us the structure.

SQL gives us a language for working with that structure.

---

## Example: A Tiny Relational Design

Let’s model customers and orders.

### customer

| customer_id | first_name | last_name |
|---|---|---|
| C001 | Jane | Smith |
| C002 | Robert | Jones |

### order

| order_id | customer_id | order_date |
|---|---|---|
| O1001 | C001 | 2026-05-01 |
| O1002 | C001 | 2026-05-02 |
| O1003 | C002 | 2026-05-03 |

This design says:

```text
One customer can have many orders.
Each order belongs to one customer.
```

The relationship is created through `customer_id`.

A query can connect the tables:

```sql
SELECT c.customer_id,
       c.first_name,
       c.last_name,
       o.order_id,
       o.order_date
FROM customer AS c
JOIN order AS o
     ON o.customer_id = c.customer_id;
```

The database can now answer connected questions.

That is the power of relational structure.

---

## Common Beginner Mistakes

### Mistake 1: One Giant Table

Beginners often try to store everything in one table.

That causes repetition and update problems.

Better design separates subjects.

```text
customer facts go in customer
order facts go in order
product facts go in product
```

### Mistake 2: No Primary Key

Without a primary key, the database may not have a clean way to identify one exact row.

Every table should have a clear identifier.

### Mistake 3: Storing Lists in One Column

Avoid this:

```text
phone_numbers = "555-1111, 555-2222"
```

Use a related table instead.

### Mistake 4: Confusing Row and Column

A column is a type of detail.

A row is one complete record.

### Mistake 5: Ignoring Data Types

Do not store numbers as text unless there is a good reason.

But also do not store phone numbers as numbers if you will never do math on them.

A phone number is an identifier-like value, not a calculation value.

---

## The “Form” Analogy

A table is like a stack of identical forms.

The form template is the schema.

The blanks on the form are columns.

Each completed form is a row.

The rules printed on the form are constraints.

The filing number at the top is the primary key.

A reference to another form is a foreign key.

That gives us:

```text
Schema      = blank form template
Column      = field on the form
Row         = completed form
Primary key = unique form number
Foreign key = reference to another form
Constraint  = rule for filling out the form
```

---

## Key Terms

| Term | Meaning |
|---|---|
| Relational model | Theory behind relational databases |
| Relation | Formal term similar to table |
| Tuple | Formal term similar to row |
| Attribute | Formal term similar to column |
| Domain | Set of valid values for an attribute |
| Degree | Number of attributes/columns |
| Cardinality | Number of tuples/rows |
| Schema | Database structure/design |
| State | Current contents/data |
| Primary key | Chosen unique row identifier |
| Candidate key | Possible unique row identifier |
| Composite key | Key made from multiple columns |
| Foreign key | Column that references a key in another table |
| Constraint | Rule that protects data integrity |
| NULL | Absence of a value |

---

## Quick Check

Answer these in your own words:

1. What is the difference between a table and a row?
2. What is the formal relational word for column?
3. What is a domain?
4. What does degree mean?
5. What does cardinality mean in the relational model?
6. Why does every table need a key?
7. What is the difference between a primary key and a foreign key?
8. Why is NULL not the same as zero?

---

## Practice Exercise

Design a tiny relational structure for a school.

Create three tables:

```text
student
course
enrollment
```

For each table, list:

```text
1. The columns
2. The primary key
3. Any foreign keys
```

Starter idea:

```text
student
- student_id
- first_name
- last_name

course
- course_id
- course_name

enrollment
- student_id
- course_id
- enrollment_date
```

Questions:

```text
1. What is the primary key of student?
2. What is the primary key of course?
3. Should enrollment have a composite key?
4. Which columns in enrollment are foreign keys?
5. What relationship does enrollment represent?
```

---

## Summary

The relational model gives databases a clean structure.

It teaches us to think in tables, rows, columns, keys, domains, and constraints.

The main beginner chain is:

```text
Relation  → table
Tuple     → row
Attribute → column
Domain    → valid values
Key       → exact row identity
Constraint → rule protecting correctness
```

The goal is not just to store data.

The goal is to store data in a way that is clear, connected, reliable, and easy to question.

That is relational thinking.

---

## Source Notes

This lesson was drafted from the uploaded relational database materials, especially the readings on the relational model, database terminology, domains, keys, constraints, relations, tuples, attributes, schema, state, and relational integrity.
