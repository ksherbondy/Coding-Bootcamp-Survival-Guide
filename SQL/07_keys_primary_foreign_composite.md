# 07 — Keys: Primary, Foreign, Composite, Candidate, and Surrogate

## Big Idea

Keys are how a relational database knows exactly which row is which.

Without keys, a database can store data, but it cannot reliably identify, connect, protect, or enforce relationships between rows.

A key answers questions like:

```text
Which customer?
Which order?
Which product?
Which student?
Which exact row?
```

Keys are also how tables connect.

A primary key identifies a row in its own table.

A foreign key points to a row in another table.

Together, they create the backbone of relational database design.

---

## Why Keys Matter

Imagine a `customer` table:

| first_name | last_name | phone |
|---|---|---|
| John | Smith | 555-1111 |
| John | Smith | 555-2222 |
| John | Smith | 555-3333 |

Which John Smith placed order `O1001`?

Names are not enough.

Phone numbers can change.

Addresses can change.

People can share names.

A database needs a stable way to identify one exact row.

So we add a key:

| customer_id | first_name | last_name | phone |
|---|---|---|---|
| C001 | John | Smith | 555-1111 |
| C002 | John | Smith | 555-2222 |
| C003 | John | Smith | 555-3333 |

Now we can say:

```text
Order O1001 belongs to customer C002.
```

No guessing.

---

## Key Mental Model

A key is like a tracking number.

If you mail a package, you do not identify it by saying:

```text
the brown box going to Virginia
```

There may be many brown boxes going to Virginia.

You use the tracking number.

A primary key is the tracking number for a row.

---

## What Is a Primary Key?

A **primary key** is the chosen column, or group of columns, that uniquely identifies each row in a table.

Example:

```sql
CREATE TABLE customer (
    customer_id CHAR(8) PRIMARY KEY,
    first_name  VARCHAR(25),
    last_name   VARCHAR(25)
);
```

Here, `customer_id` is the primary key.

That means:

```text
Every customer must have a customer_id.
No two customers can have the same customer_id.
The customer_id identifies one exact customer row.
```

### Primary Key Rules

A primary key should be:

```text
Unique
Not null
Stable
Minimal
Clear
```

### Beginner Translation

The primary key says:

```text
This row has one official identity.
```

---

## Primary Key Example

Table:

| customer_id | first_name | last_name |
|---|---|---|
| C001 | Jane | Smith |
| C002 | Robert | Jones |
| C003 | Maria | Garcia |

Primary key:

```text
customer_id
```

Why?

Because each value appears once.

```text
C001 → Jane Smith
C002 → Robert Jones
C003 → Maria Garcia
```

---

## What Does Unique Mean?

**Unique** means no duplicate values are allowed for the key.

Bad primary key data:

| customer_id | first_name |
|---|---|
| C001 | Jane |
| C001 | Robert |

This is invalid because `C001` appears twice.

The database would not know which row `C001` means.

---

## What Does NOT NULL Mean?

A primary key cannot be NULL.

Bad primary key data:

| customer_id | first_name |
|---|---|
| C001 | Jane |
| NULL | Robert |

This is invalid because the second row has no identity.

### Beginner Translation

A primary key cannot say:

```text
I do not know who this row is.
```

---

## What Does Stable Mean?

A good primary key should not change often.

Bad primary key candidate:

```text
phone_number
```

Why?

People change phone numbers.

Better:

```text
customer_id
```

The system controls it and can keep it stable.

### Beginner Translation

A primary key should be more like a serial number than a nickname.

---

## What Does Minimal Mean?

A key should not contain unnecessary columns.

Suppose `student_id` alone uniquely identifies a student.

Then this is unnecessary:

```text
student_id + first_name + last_name
```

The extra columns are not needed.

A minimal key uses only what is required to identify the row.

---

## Candidate Keys

A **candidate key** is any minimal column or group of columns that could uniquely identify a row.

Example:

```text
STUDENT
- student_id
- school_email
- first_name
- last_name
```

If both `student_id` and `school_email` are unique, then both are candidate keys.

```text
student_id   → candidate key
school_email → candidate key
```

The database designer chooses one to become the primary key.

### Beginner Translation

Candidate keys are possible official IDs.

The primary key is the one we choose.

---

## Primary Key vs. Candidate Key

| Concept | Meaning |
|---|---|
| Candidate key | Could uniquely identify a row |
| Primary key | The candidate key chosen as the official row identifier |

Example:

```text
Candidate keys:
- student_id
- school_email

Chosen primary key:
- student_id
```

The other candidate key can still be protected with a `UNIQUE` constraint.

```sql
CREATE TABLE student (
    student_id   CHAR(8) PRIMARY KEY,
    school_email VARCHAR(100) UNIQUE,
    first_name   VARCHAR(25),
    last_name    VARCHAR(25)
);
```

---

## Alternate Keys

An **alternate key** is a candidate key that was not chosen as the primary key.

Example:

```text
student_id is chosen as primary key.
school_email is still unique.
```

Then `school_email` is an alternate key.

### Beginner Translation

An alternate key is the backup unique identifier.

It is not the main ID, but it still must be unique.

---

## Natural Keys

A **natural key** is a key that already exists in the real world.

Examples:

```text
VIN for a vehicle
ISBN for a book edition
email address for an account in some systems
serial number for a device
```

Example:

```sql
CREATE TABLE car (
    vin   VARCHAR(20) PRIMARY KEY,
    make  VARCHAR(25),
    model VARCHAR(25)
);
```

Here, `vin` is a natural key.

It already exists before the database creates it.

### Pros

Natural keys can be meaningful.

```text
A VIN identifies a real vehicle.
An ISBN identifies a book edition.
```

### Cons

Natural keys can be:

```text
long
mistyped
changed
reused in edge cases
sensitive
controlled by outside systems
```

### Beginner Translation

A natural key comes from the world, not from your database.

---

## Surrogate Keys

A **surrogate key** is an artificial key created by the database or application.

Examples:

```text
customer_id
order_id
student_id
product_id
```

Example:

```sql
CREATE TABLE customer (
    customer_id INTEGER PRIMARY KEY,
    first_name  VARCHAR(25),
    last_name   VARCHAR(25)
);
```

The database might auto-generate `customer_id`.

### Pros

Surrogate keys are often:

```text
short
stable
simple
system-controlled
not meaningful outside the database
```

### Cons

Surrogate keys do not carry real-world meaning.

`customer_id = 42` tells you nothing by itself.

### Beginner Translation

A surrogate key is a database-made name tag.

---

## Natural Key vs. Surrogate Key

| Type | Comes From | Example | Strength | Risk |
|---|---|---|---|---|
| Natural key | Real world | VIN, ISBN | Meaningful | May change or have exceptions |
| Surrogate key | System | customer_id | Stable and simple | No real-world meaning |

### Practical Beginner Rule

Use surrogate keys often for internal table identity.

Use natural keys carefully when they are truly stable, unique, and trusted.

You can also use both:

```sql
CREATE TABLE car (
    car_id INTEGER PRIMARY KEY,
    vin    VARCHAR(20) UNIQUE NOT NULL,
    make   VARCHAR(25),
    model  VARCHAR(25)
);
```

Here:

```text
car_id = surrogate primary key
vin    = natural alternate key
```

---

## Composite Keys

A **composite key** is a key made from more than one column.

Example:

```text
ENROLLMENT
- student_id
- course_id
- enrollment_date
- grade
```

A student can enroll in many courses.

A course can have many students.

So `student_id` alone is not unique.

`course_id` alone is not unique.

But together:

```text
student_id + course_id
```

may uniquely identify one enrollment row.

```sql
CREATE TABLE enrollment (
    student_id CHAR(8),
    course_id  CHAR(8),
    grade      CHAR(2),
    PRIMARY KEY (student_id, course_id)
);
```

### Beginner Translation

A composite key is a combination lock.

One column alone is not enough.

The combination identifies the row.

---

## Composite Key Example

| student_id | course_id | grade |
|---|---|---|
| S001 | C101 | A |
| S001 | C205 | B |
| S002 | C101 | A |

`student_id` repeats.

`course_id` repeats.

But the pair does not repeat:

```text
S001 + C101
S001 + C205
S002 + C101
```

Each pair identifies one row.

---

## When Composite Keys Are Common

Composite keys commonly appear in:

```text
bridge tables
line item tables
relationship tables
weak entity tables
history tables
```

Examples:

```text
enrollment(student_id, course_id)
order_line(order_id, line_number)
employee_skill(employee_id, skill_id)
project_assignment(employee_id, project_id)
```

---

## What Is a Foreign Key?

A **foreign key** is a column, or group of columns, that refers to a key in another table.

Example:

```text
customer.customer_id
order_record.customer_id
```

In `customer`, `customer_id` is the primary key.

In `order_record`, `customer_id` is a foreign key.

```sql
CREATE TABLE customer (
    customer_id CHAR(8) PRIMARY KEY,
    first_name  VARCHAR(25),
    last_name   VARCHAR(25)
);

CREATE TABLE order_record (
    order_id    CHAR(8) PRIMARY KEY,
    customer_id CHAR(8) NOT NULL,
    order_date  DATE,
    FOREIGN KEY (customer_id) REFERENCES customer(customer_id)
);
```

### Beginner Translation

A foreign key is a reference.

It says:

```text
This order belongs to this customer.
```

---

## Primary Key vs. Foreign Key

| Key Type | Job |
|---|---|
| Primary key | Identifies a row in its own table |
| Foreign key | Points to a row in another table |

Example:

```text
customer.customer_id      = primary key
order_record.customer_id  = foreign key
```

Same value.

Different job.

---

## Foreign Key Example

### customer

| customer_id | first_name |
|---|---|
| C001 | Jane |
| C002 | Robert |

### order_record

| order_id | customer_id |
|---|---|
| O1001 | C001 |
| O1002 | C001 |
| O1003 | C002 |

This means:

```text
O1001 belongs to Jane.
O1002 belongs to Jane.
O1003 belongs to Robert.
```

The foreign key creates the relationship.

---

## Referential Integrity

**Referential integrity** means foreign keys must point to real rows.

Bad order:

| order_id | customer_id |
|---|---|
| O1004 | C999 |

If there is no customer `C999`, then this order points to nothing.

That should be rejected.

The foreign key protects the database from orphan records.

### Beginner Translation

Referential integrity says:

```text
If you point to something, it must exist.
```

---

## Orphan Records

An **orphan record** is a child row that points to a missing parent row.

Example:

```text
order_record.customer_id = C999
```

But no customer exists with:

```text
customer.customer_id = C999
```

The order is now orphaned.

Foreign keys help prevent this.

---

## Parent and Child Tables

In a one-to-many relationship:

```text
CUSTOMER 1 ─── many ORDER
```

The `customer` table is the parent.

The `order_record` table is the child.

The child table contains the foreign key.

```text
order_record.customer_id
```

### Beginner Translation

The child points back to the parent.

---

## Foreign Keys in One-to-Many Relationships

Rule:

```text
The foreign key goes on the many side.
```

Example:

```text
One department has many employees.
```

Table design:

```sql
CREATE TABLE department (
    department_id CHAR(8) PRIMARY KEY,
    department_name VARCHAR(50)
);

CREATE TABLE employee (
    employee_id   CHAR(8) PRIMARY KEY,
    department_id CHAR(8) NOT NULL,
    first_name    VARCHAR(25),
    last_name     VARCHAR(25),
    FOREIGN KEY (department_id) REFERENCES department(department_id)
);
```

Why?

Because each employee works for one department.

So each employee row stores the department ID.

---

## Foreign Keys in Many-to-Many Relationships

Many-to-many relationships need a bridge table.

Example:

```text
STUDENT many ─── many COURSE
```

Bridge:

```text
ENROLLMENT
```

```sql
CREATE TABLE enrollment (
    student_id CHAR(8),
    course_id  CHAR(8),
    grade      CHAR(2),
    PRIMARY KEY (student_id, course_id),
    FOREIGN KEY (student_id) REFERENCES student(student_id),
    FOREIGN KEY (course_id) REFERENCES course(course_id)
);
```

The bridge table has two foreign keys.

Often, those same columns also form the composite primary key.

### Beginner Translation

A bridge table usually says:

```text
This row connects one thing from table A to one thing from table B.
```

---

## Foreign Keys Can Reference Composite Keys

If a parent table has a composite primary key, a child table may need a composite foreign key.

Example:

```text
ORDER_LINE primary key:
order_id + line_number
```

Another table might reference that exact line:

```sql
FOREIGN KEY (order_id, line_number)
REFERENCES order_line(order_id, line_number)
```

### Beginner Translation

If the parent row needs two values to identify it, the child needs both values to point to it.

---

## Unique Constraints

A `UNIQUE` constraint prevents duplicate values in a column or group of columns.

Example:

```sql
CREATE TABLE user_account (
    user_id INTEGER PRIMARY KEY,
    email   VARCHAR(100) UNIQUE NOT NULL
);
```

Here:

```text
user_id = primary key
email   = unique alternate key
```

### Primary Key vs. Unique

| Constraint | Allows NULL? | Main Identity? | Duplicates? |
|---|---|---|---|
| PRIMARY KEY | No | Yes | No |
| UNIQUE | Depends on DBMS/rules | No | No |

### Beginner Translation

Primary key is the official row identity.

Unique means “no duplicates allowed.”

---

## Choosing Good Keys

A good key should be:

```text
Unique
Stable
Minimal
Not null
Simple
Hard to mistype
Not overloaded with meaning
```

### Avoid Bad Keys

Avoid using values that can change:

```text
phone_number
email address, unless the system treats it carefully
last_name
street_address
job_title
```

Avoid vague key names:

```text
id
```

Prefer clearer names:

```text
customer_id
order_id
student_id
product_id
```

---

## The Problem With Meaningful IDs

Sometimes IDs contain meaning:

```text
MD-2026-0001
```

This might mean:

```text
Maryland customer created in 2026, sequence 0001
```

That looks useful.

But meaningful IDs can become painful if meaning changes.

What if the customer moves?

What if the year was entered wrong?

What if the company expands?

### Beginner Rule

Be careful with smart IDs.

A key should identify.

It should not try to tell the whole story.

---

## Cascading Actions

Foreign keys can define what happens when a parent row is deleted or updated.

Common actions:

```text
RESTRICT / NO ACTION
CASCADE
SET NULL
SET DEFAULT
```

### RESTRICT / NO ACTION

Do not allow deleting a parent if children still reference it.

Example:

```text
Cannot delete customer C001 while orders still point to C001.
```

### CASCADE

Automatically apply the action to child rows.

Example:

```text
Delete customer C001 and automatically delete their orders.
```

This can be dangerous.

### SET NULL

If the parent is deleted, set the child foreign key to NULL.

Example:

```text
Delete a manager and set employee.manager_id to NULL.
```

### Beginner Warning

Cascades are powerful.

Use them carefully because they can delete or change many rows.

---

## Foreign Key Optionality

A foreign key can be required or optional.

### Required Foreign Key

```sql
customer_id CHAR(8) NOT NULL
```

Means:

```text
This row must belong to a customer.
```

### Optional Foreign Key

```sql
manager_id CHAR(8)
```

Means:

```text
This row may or may not point to a manager.
```

Example:

```text
Top-level employee may not have a manager.
```

### Beginner Translation

`NOT NULL` often means the relationship is mandatory.

Nullable foreign key often means the relationship is optional.

---

## Example: Complete Customer and Order Keys

```sql
CREATE TABLE customer (
    customer_id CHAR(8) PRIMARY KEY,
    first_name  VARCHAR(25) NOT NULL,
    last_name   VARCHAR(25) NOT NULL,
    email       VARCHAR(100) UNIQUE
);

CREATE TABLE order_record (
    order_id    CHAR(8) PRIMARY KEY,
    customer_id CHAR(8) NOT NULL,
    order_date  DATE NOT NULL,
    FOREIGN KEY (customer_id) REFERENCES customer(customer_id)
);
```

This design says:

```text
Each customer has one official ID.
Each order has one official ID.
Each order must point to a real customer.
Email, if used, cannot be duplicated.
```

---

## Example: Complete Student, Course, Enrollment Keys

```sql
CREATE TABLE student (
    student_id CHAR(8) PRIMARY KEY,
    first_name VARCHAR(25) NOT NULL,
    last_name  VARCHAR(25) NOT NULL
);

CREATE TABLE course (
    course_id   CHAR(8) PRIMARY KEY,
    course_name VARCHAR(50) NOT NULL
);

CREATE TABLE enrollment (
    student_id      CHAR(8),
    course_id       CHAR(8),
    enrollment_date DATE NOT NULL,
    grade           CHAR(2),
    PRIMARY KEY (student_id, course_id),
    FOREIGN KEY (student_id) REFERENCES student(student_id),
    FOREIGN KEY (course_id) REFERENCES course(course_id)
);
```

This design says:

```text
A student can enroll in many courses.
A course can have many students.
The enrollment row connects one student to one course.
The same student cannot enroll in the same course twice under this key design.
```

---

## Example: Order and Order Line Keys

```sql
CREATE TABLE order_record (
    order_id   CHAR(8) PRIMARY KEY,
    order_date DATE NOT NULL
);

CREATE TABLE order_line (
    order_id    CHAR(8),
    line_number INTEGER,
    product_id  CHAR(8) NOT NULL,
    quantity    INTEGER NOT NULL,
    unit_price  DECIMAL(10, 2) NOT NULL,
    PRIMARY KEY (order_id, line_number),
    FOREIGN KEY (order_id) REFERENCES order_record(order_id)
);
```

This design says:

```text
Each order has many lines.
Each line is identified by order_id plus line_number.
line_number can repeat across different orders.
line_number cannot repeat within the same order.
```

---

## Common Beginner Mistakes

### Mistake 1: No Primary Key

Every table should have a clear way to identify rows.

Without it, updates and relationships become risky.

### Mistake 2: Using Name as a Primary Key

Names are rarely unique and can change.

Bad:

```text
primary key = first_name + last_name
```

Better:

```text
customer_id
```

### Mistake 3: Thinking Foreign Keys Create Data Automatically

A foreign key does not create the parent row.

It only checks that the parent row exists.

You must insert the parent before the child.

### Mistake 4: Forgetting Composite Keys in Bridge Tables

A bridge table often needs a composite key or its own surrogate key plus a uniqueness rule.

### Mistake 5: Using `id` Everywhere

`id` becomes confusing in joins.

Prefer:

```text
customer_id
order_id
product_id
```

### Mistake 6: Letting Foreign Keys Point to Nothing

That creates orphan records.

Use foreign key constraints.

---

## Key Design Checklist

When designing keys, ask:

```text
1. Does every table have a primary key?
2. Is the primary key unique?
3. Is the primary key NOT NULL?
4. Is the primary key stable?
5. Is the primary key minimal?
6. Are natural keys protected with UNIQUE where needed?
7. Are foreign keys placed on the correct side of relationships?
8. Do all foreign keys reference real parent keys?
9. Are optional relationships allowed to be NULL?
10. Are mandatory relationships marked NOT NULL?
11. Do bridge tables have composite keys or proper uniqueness rules?
12. Are cascade actions intentional and safe?
```

---

## The “Passport and Address Book” Analogy

A primary key is like a passport number for a row.

It uniquely identifies the row.

A foreign key is like writing someone’s passport number in another record to show a connection.

Example:

```text
Customer passport: C001
Order form says: customer_id = C001
```

Now the order is connected to that exact customer.

Not just “Jane.”

Not just “the customer from Maryland.”

The exact customer.

---

## Key Terms

| Term | Meaning |
|---|---|
| Key | Attribute or attributes used to identify rows |
| Primary key | Chosen official row identifier |
| Candidate key | Possible unique row identifier |
| Alternate key | Candidate key not chosen as primary |
| Natural key | Real-world identifier |
| Surrogate key | System-created identifier |
| Composite key | Key made of multiple columns |
| Foreign key | Column or columns that reference another table |
| Referential integrity | Rule that references must point to valid rows |
| Orphan record | Child row with no valid parent |
| Parent table | Table being referenced |
| Child table | Table containing the foreign key |
| UNIQUE | Constraint preventing duplicate values |
| CASCADE | Action that automatically applies changes to child rows |

---

## Quick Check

Answer these in your own words:

1. What is the job of a primary key?
2. Why is a name usually a bad primary key?
3. What is a candidate key?
4. What is an alternate key?
5. What is the difference between a natural key and a surrogate key?
6. What is a composite key?
7. What is the job of a foreign key?
8. What does referential integrity mean?
9. What is an orphan record?
10. Why do bridge tables often use composite keys?

---

## Practice Exercise

Design keys for this system:

```text
A repair shop tracks customers, vehicles, and service appointments.
A customer can own many vehicles.
A vehicle can have many service appointments.
Each appointment belongs to one vehicle.
```

Answer:

```text
1. What tables do you need?
2. What is the primary key for each table?
3. What foreign keys are needed?
4. Which relationships are one-to-many?
5. Which foreign keys should be NOT NULL?
6. Would VIN be a natural key, alternate key, or primary key?
```

Possible starting point:

```text
customer
- customer_id PK

vehicle
- vehicle_id PK
- vin UNIQUE
- customer_id FK

service_appointment
- appointment_id PK
- vehicle_id FK
```

---

## Summary

Keys are the identity and connection system of a relational database.

The core ideas are:

```text
Primary key = identifies a row in its own table
Foreign key = points to a row in another table
Candidate key = could identify a row
Alternate key = candidate key not chosen
Natural key = real-world identifier
Surrogate key = system-created identifier
Composite key = multiple columns working together
```

A database without good keys is like a filing cabinet with missing labels and broken cross-references.

Good keys make data exact, connected, and trustworthy.

---

## Source Notes

This lesson was drafted from the uploaded database design, relational model, ERD, ER-to-table, normalization, and SQL materials, especially the readings on primary keys, foreign keys, composite keys, candidate keys, relation keys, referential integrity, entity constraints, weak entities, and bridge tables.
