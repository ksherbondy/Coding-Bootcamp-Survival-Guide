# 06 — ER Model to Tables: Turning the Diagram Into a Relational Schema

## Big Idea

An ERD is the blueprint.

Tables are the build.

Once the entities, attributes, relationships, and cardinalities make sense, the next step is converting the ER model into a relational database structure.

That means turning:

```text
Entities into tables
Attributes into columns
Keys into primary keys
Relationships into foreign keys
Many-to-many relationships into bridge tables
```

This is the point where the visual design starts becoming something SQL can create.

---

## The Core Mapping

The basic translation looks like this:

| ER Model Concept | Relational Database Concept |
|---|---|
| Entity | Table |
| Attribute | Column |
| Entity instance | Row |
| Key attribute | Primary key |
| Relationship | Foreign key or bridge table |
| Many-to-many relationship | Bridge table |
| Multivalued attribute | Separate related table |
| Composite attribute | Separate simple columns |

### Beginner Translation

The ERD is the drawing.

The relational schema is the table plan.

SQL is how we build the plan.

---

## Start With the Entity

Suppose the ERD has this entity:

```text
CUSTOMER
- customer_id
- first_name
- last_name
- phone_number
```

That becomes a table:

```sql
CREATE TABLE customer (
    customer_id  CHAR(8),
    first_name   VARCHAR(25),
    last_name    VARCHAR(25),
    phone_number VARCHAR(12)
);
```

The entity name becomes the table name.

The attributes become the columns.

---

## Naming: Entity Names vs. Table Names

In ERDs, entities are often written as singular uppercase names:

```text
CUSTOMER
ORDER
PRODUCT
```

In SQL, teams may choose different naming styles.

Common choices include:

```text
customer
order_record
product
```

or:

```text
customers
orders
products
```

There is not one universal rule everyone agrees on.

The important beginner rule is:

```text
Pick a naming style and stay consistent.
```

For this lesson, we will use:

```text
lowercase
underscores
mostly singular table names
```

Example:

```text
customer
salesperson
order_line
```

---

## Step 1: Create a Table for Each Strong Entity

A **strong entity** has its own identity.

Example ERD entities:

```text
CUSTOMER
CAR
SALESPERSON
DEALERSHIP
```

These become tables:

```text
customer
car
salesperson
dealership
```

Each table should have a primary key.

Example:

```sql
CREATE TABLE customer (
    customer_id CHAR(8),
    first_name  VARCHAR(25),
    last_name   VARCHAR(25),
    phone       VARCHAR(12)
);
```

But this is not complete yet.

We need to declare the key.

---

## Step 2: Convert Key Attributes Into Primary Keys

If the ERD says `customer_id` identifies a customer, then it becomes the primary key.

```sql
CREATE TABLE customer (
    customer_id CHAR(8) PRIMARY KEY,
    first_name  VARCHAR(25),
    last_name   VARCHAR(25),
    phone       VARCHAR(12)
);
```

### Beginner Translation

The primary key tells the database:

```text
No two customer rows can use the same customer_id.
This column identifies one exact customer.
```

---

## Step 3: Convert Simple Attributes Into Columns

Simple attributes become normal columns.

ERD:

```text
CUSTOMER
- customer_id
- first_name
- last_name
- state
```

Table:

```sql
CREATE TABLE customer (
    customer_id CHAR(8) PRIMARY KEY,
    first_name  VARCHAR(25),
    last_name   VARCHAR(25),
    state       CHAR(2)
);
```

Each attribute needs a data type.

---

## Step 4: Choose Data Types

Every column needs a data type.

Common examples:

| Kind of Data | Possible SQL Type |
|---|---|
| Short fixed code | CHAR |
| Variable text | VARCHAR |
| Whole number | INTEGER |
| Money/precise decimal | DECIMAL |
| Date | DATE |
| Date and time | TIMESTAMP |
| True/false | BOOLEAN |

Example:

```sql
CREATE TABLE car (
    vin      VARCHAR(20) PRIMARY KEY,
    make     VARCHAR(25),
    model    VARCHAR(25),
    color    VARCHAR(20),
    mileage  INTEGER,
    price    DECIMAL(10, 2)
);
```

### Beginner Warning

Do not automatically make every number-looking value numeric.

Phone numbers, ZIP codes, SSNs, IDs, and VINs are usually not math values.

You do not add them, subtract them, average them, or multiply them.

They are identifiers or codes, so they are often stored as character/text values.

Example:

```sql
zip_code CHAR(5)
phone    VARCHAR(12)
```

---

## Step 5: Break Composite Attributes Into Simple Columns

A composite attribute is made of smaller parts.

Example:

```text
name
```

Could become:

```text
first_name
middle_name
last_name
```

Another example:

```text
address
```

Could become:

```text
street
city
state
zip_code
```

Bad table design:

```sql
CREATE TABLE customer (
    customer_id CHAR(8) PRIMARY KEY,
    name        VARCHAR(100),
    address     VARCHAR(200)
);
```

Better table design:

```sql
CREATE TABLE customer (
    customer_id CHAR(8) PRIMARY KEY,
    first_name  VARCHAR(25),
    last_name   VARCHAR(25),
    street      VARCHAR(50),
    city        VARCHAR(30),
    state       CHAR(2),
    zip_code    CHAR(5)
);
```

### Beginner Translation

Do not store a bundle if the database needs to search, sort, filter, validate, or update the pieces separately.

---

## Step 6: Handle Multivalued Attributes With a New Table

A multivalued attribute can have more than one value for the same entity instance.

Example:

```text
A customer can have multiple phone numbers.
```

Bad design:

```sql
CREATE TABLE customer (
    customer_id   CHAR(8) PRIMARY KEY,
    first_name    VARCHAR(25),
    last_name     VARCHAR(25),
    phone_numbers VARCHAR(100)
);
```

Possible bad data:

```text
555-1111, 555-2222, 555-3333
```

Better design:

```sql
CREATE TABLE customer (
    customer_id CHAR(8) PRIMARY KEY,
    first_name  VARCHAR(25),
    last_name   VARCHAR(25)
);

CREATE TABLE customer_phone (
    customer_id  CHAR(8),
    phone_number VARCHAR(12),
    phone_type   VARCHAR(10),
    PRIMARY KEY (customer_id, phone_number),
    FOREIGN KEY (customer_id) REFERENCES customer(customer_id)
);
```

### Beginner Translation

If one attribute needs a list, it probably wants to become a related table.

---

## Step 7: Convert One-to-Many Relationships

This is one of the biggest rules in database design.

For a one-to-many relationship:

```text
CUSTOMER 1 ─── many ORDER
```

The foreign key goes on the many side.

So `order` gets `customer_id`.

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

### Why the Foreign Key Goes on the Many Side

One customer can have many orders.

If we tried putting `order_id` in the customer table, we would need multiple order IDs in one customer row.

Bad:

```text
customer_id | order_ids
C001        | O001, O002, O003
```

That violates clean relational design.

Instead, each order stores the one customer it belongs to.

Good:

```text
order_id | customer_id
O001     | C001
O002     | C001
O003     | C001
```

### Beginner Rule

```text
For one-to-many:
put the primary key from the one side
into the table on the many side
as a foreign key.
```

---

## Step 8: Convert One-to-One Relationships

For one-to-one relationships, you have design choices.

Example:

```text
PERSON 1 ─── 1 PASSPORT_RECORD
```

You could put the foreign key in either table, but usually it goes where the relationship is more optional or more dependent.

Example:

```sql
CREATE TABLE person (
    person_id  CHAR(8) PRIMARY KEY,
    first_name VARCHAR(25),
    last_name  VARCHAR(25)
);

CREATE TABLE passport_record (
    passport_id     CHAR(9) PRIMARY KEY,
    person_id       CHAR(8) UNIQUE NOT NULL,
    expiration_date DATE,
    FOREIGN KEY (person_id) REFERENCES person(person_id)
);
```

The `UNIQUE` constraint on `person_id` keeps it one-to-one.

Without `UNIQUE`, many passport records could point to the same person.

### Beginner Translation

A one-to-one foreign key needs uniqueness on the foreign key column if you want to enforce “only one.”

---

## Step 9: Convert Many-to-Many Relationships

Many-to-many relationships require a bridge table.

Example:

```text
STUDENT many ─── many COURSE
```

Convert it to:

```text
STUDENT 1 ─── many ENROLLMENT many ─── 1 COURSE
```

Tables:

```sql
CREATE TABLE student (
    student_id CHAR(8) PRIMARY KEY,
    first_name VARCHAR(25),
    last_name  VARCHAR(25)
);

CREATE TABLE course (
    course_id   CHAR(8) PRIMARY KEY,
    course_name VARCHAR(50),
    credits     INTEGER
);

CREATE TABLE enrollment (
    student_id      CHAR(8),
    course_id       CHAR(8),
    enrollment_date DATE,
    grade           CHAR(2),
    PRIMARY KEY (student_id, course_id),
    FOREIGN KEY (student_id) REFERENCES student(student_id),
    FOREIGN KEY (course_id) REFERENCES course(course_id)
);
```

### Why the Bridge Table Works

One student can appear in many enrollment rows.

One course can appear in many enrollment rows.

Each enrollment row connects one student to one course.

### Beginner Translation

The bridge table is the handshake record.

It says:

```text
This student is connected to this course.
```

And it can store details about that connection:

```text
enrollment_date
grade
status
```

---

## Step 10: Convert Relationship Attributes

Sometimes the relationship itself has attributes.

Example:

```text
Student enrolls in course.
```

The relationship might have:

```text
enrollment_date
grade
status
```

Those attributes belong on the bridge table:

```sql
CREATE TABLE enrollment (
    student_id      CHAR(8),
    course_id       CHAR(8),
    enrollment_date DATE,
    grade           CHAR(2),
    status          VARCHAR(12),
    PRIMARY KEY (student_id, course_id),
    FOREIGN KEY (student_id) REFERENCES student(student_id),
    FOREIGN KEY (course_id) REFERENCES course(course_id)
);
```

### Beginner Translation

If the connection has details, put those details on the table that represents the connection.

---

## Step 11: Convert Weak Entities

A weak entity depends on another entity for identity.

Example:

```text
ORDER
ORDER_LINE
```

An order line cannot exist without an order.

The line number may only be unique inside one order.

So the key could be:

```text
order_id + line_number
```

Tables:

```sql
CREATE TABLE order_record (
    order_id   CHAR(8) PRIMARY KEY,
    order_date DATE
);

CREATE TABLE order_line (
    order_id    CHAR(8),
    line_number INTEGER,
    product_id  CHAR(8),
    quantity    INTEGER,
    unit_price  DECIMAL(10, 2),
    PRIMARY KEY (order_id, line_number),
    FOREIGN KEY (order_id) REFERENCES order_record(order_id)
);
```

### Beginner Translation

A weak entity borrows part of its identity from its parent.

`line_number = 1` is not unique by itself.

But `order_id + line_number` is unique.

---

## Step 12: Convert Recursive Relationships

A recursive relationship is when a table relates to itself.

Example:

```text
An employee may manage other employees.
```

Table:

```sql
CREATE TABLE employee (
    employee_id CHAR(8) PRIMARY KEY,
    manager_id  CHAR(8),
    first_name  VARCHAR(25),
    last_name   VARCHAR(25),
    FOREIGN KEY (manager_id) REFERENCES employee(employee_id)
);
```

Here, `manager_id` points back to `employee_id` in the same table.

### Beginner Translation

A recursive foreign key says:

```text
This employee reports to another employee.
```

---

## Step 13: Convert Ternary Relationships Carefully

A ternary relationship involves three entities at once.

Example:

```text
A supplier supplies a part to a project.
```

Entities:

```text
SUPPLIER
PART
PROJECT
```

Bridge table:

```sql
CREATE TABLE supply (
    supplier_id CHAR(8),
    part_id     CHAR(8),
    project_id  CHAR(8),
    quantity    INTEGER,
    PRIMARY KEY (supplier_id, part_id, project_id),
    FOREIGN KEY (supplier_id) REFERENCES supplier(supplier_id),
    FOREIGN KEY (part_id) REFERENCES part(part_id),
    FOREIGN KEY (project_id) REFERENCES project(project_id)
);
```

### Beginner Warning

Do not automatically split a three-way fact into three two-way facts.

Sometimes the meaning depends on all three entities together.

### Beginner Translation

A ternary relationship is a three-part receipt.

You need all three IDs to know what happened.

---

## Full Example: Vehicle Sales

Plain English:

```text
Customers buy cars.
Salespeople help with sales.
A car can be sold more than once over time.
Each sale connects one customer, one car, and one salesperson.
```

### ERD Entities

```text
CUSTOMER
CAR
SALESPERSON
SALE
```

### Tables

```sql
CREATE TABLE customer (
    customer_id CHAR(8) PRIMARY KEY,
    first_name  VARCHAR(25),
    last_name   VARCHAR(25),
    phone       VARCHAR(12)
);

CREATE TABLE car (
    vin     VARCHAR(20) PRIMARY KEY,
    make    VARCHAR(25),
    model   VARCHAR(25),
    color   VARCHAR(20),
    mileage INTEGER
);

CREATE TABLE salesperson (
    salesperson_id CHAR(8) PRIMARY KEY,
    first_name     VARCHAR(25),
    last_name      VARCHAR(25),
    phone          VARCHAR(12)
);

CREATE TABLE sale (
    sale_id        CHAR(8) PRIMARY KEY,
    customer_id    CHAR(8) NOT NULL,
    vin            VARCHAR(20) NOT NULL,
    salesperson_id CHAR(8) NOT NULL,
    sale_date      DATE,
    sale_price     DECIMAL(10, 2),
    FOREIGN KEY (customer_id) REFERENCES customer(customer_id),
    FOREIGN KEY (vin) REFERENCES car(vin),
    FOREIGN KEY (salesperson_id) REFERENCES salesperson(salesperson_id)
);
```

### What Happened?

`SALE` became the connection point.

It stores foreign keys to:

```text
customer
car
salesperson
```

And it stores details about the sale:

```text
sale_date
sale_price
```

### Beginner Translation

A sale is the event record.

It answers:

```text
Who bought it?
What did they buy?
Who sold it?
When did it happen?
For how much?
```

---

## Full Example: Library Checkout

ERD idea:

```text
MEMBER 1 ─── many CHECKOUT many ─── 1 BOOK
```

Tables:

```sql
CREATE TABLE member (
    member_id  CHAR(8) PRIMARY KEY,
    first_name VARCHAR(25),
    last_name  VARCHAR(25),
    email      VARCHAR(50)
);

CREATE TABLE book (
    book_id CHAR(8) PRIMARY KEY,
    isbn    VARCHAR(20),
    title   VARCHAR(100),
    author  VARCHAR(100)
);

CREATE TABLE checkout (
    checkout_id   CHAR(8) PRIMARY KEY,
    member_id     CHAR(8) NOT NULL,
    book_id       CHAR(8) NOT NULL,
    checkout_date DATE NOT NULL,
    return_date   DATE,
    FOREIGN KEY (member_id) REFERENCES member(member_id),
    FOREIGN KEY (book_id) REFERENCES book(book_id)
);
```

### Why Not Just Put member_id in book?

Because a book can be checked out many times over its lifetime.

The checkout event matters.

A `checkout` table preserves the history.

---

## Full Example: Order Lines

ERD idea:

```text
CUSTOMER 1 ─── many ORDER
ORDER 1 ─── many ORDER_LINE
PRODUCT 1 ─── many ORDER_LINE
```

Tables:

```sql
CREATE TABLE customer (
    customer_id CHAR(8) PRIMARY KEY,
    first_name  VARCHAR(25),
    last_name   VARCHAR(25)
);

CREATE TABLE product (
    product_id   CHAR(8) PRIMARY KEY,
    product_name VARCHAR(50),
    current_price DECIMAL(10, 2)
);

CREATE TABLE order_record (
    order_id    CHAR(8) PRIMARY KEY,
    customer_id CHAR(8) NOT NULL,
    order_date  DATE,
    FOREIGN KEY (customer_id) REFERENCES customer(customer_id)
);

CREATE TABLE order_line (
    order_id    CHAR(8),
    line_number INTEGER,
    product_id  CHAR(8) NOT NULL,
    quantity    INTEGER,
    unit_price  DECIMAL(10, 2),
    PRIMARY KEY (order_id, line_number),
    FOREIGN KEY (order_id) REFERENCES order_record(order_id),
    FOREIGN KEY (product_id) REFERENCES product(product_id)
);
```

### Why Store unit_price on order_line?

The product’s current price can change.

The order line should preserve the price at the time of purchase.

This is a good example of storing a historical fact where it belongs.

---

## The Conversion Checklist

When converting an ERD to tables, use this checklist:

```text
1. Create a table for each strong entity.
2. Convert simple attributes into columns.
3. Break composite attributes into simple columns.
4. Move multivalued attributes into separate related tables.
5. Choose a primary key for each table.
6. Convert one-to-many relationships with a foreign key on the many side.
7. Convert one-to-one relationships with a foreign key plus UNIQUE when needed.
8. Convert many-to-many relationships into bridge tables.
9. Put relationship attributes on the bridge table.
10. Convert weak entities using the parent key plus partial key if needed.
11. Convert recursive relationships with a self-referencing foreign key.
12. Choose appropriate data types.
13. Add NOT NULL where participation is mandatory.
14. Add foreign key constraints to protect referential integrity.
```

---

## Common Beginner Mistakes

### Mistake 1: Making Tables Before Understanding Relationships

If you do not know the relationships, you will not know where the foreign keys belong.

### Mistake 2: Leaving Many-to-Many Relationships Unresolved

Bad:

```text
student table has course_ids = "C101, C102, C205"
```

Good:

```text
enrollment table has one row per student-course connection
```

### Mistake 3: Putting the Foreign Key on the One Side

For one-to-many relationships, the foreign key belongs on the many side.

### Mistake 4: Storing Composite Data as One Big String

Bad:

```text
address = "100 Main St, Baltimore, MD 21201"
```

Better:

```text
street
city
state
zip_code
```

### Mistake 5: Forgetting Constraints

A database without constraints relies too much on application code to prevent bad data.

Use constraints to protect the database itself.

### Mistake 6: Using SELECT-Friendly Names but Bad Schema Names

Good names should be readable in SQL later.

Avoid vague names like:

```text
id
name
data
thing
misc
```

Prefer clear names:

```text
customer_id
first_name
order_date
product_name
```

---

## How Mandatory Participation Becomes NOT NULL

In an ERD, mandatory participation means the relationship is required.

Example:

```text
Each ORDER must belong to one CUSTOMER.
```

That means `customer_id` in `order_record` should usually be `NOT NULL`.

```sql
CREATE TABLE order_record (
    order_id    CHAR(8) PRIMARY KEY,
    customer_id CHAR(8) NOT NULL,
    order_date  DATE,
    FOREIGN KEY (customer_id) REFERENCES customer(customer_id)
);
```

### Beginner Translation

Mandatory in the ERD often becomes `NOT NULL` in the table.

---

## How Optional Participation Allows NULL

Example:

```text
An employee may have a manager.
```

Top-level employees may not have a manager.

So `manager_id` can allow NULL:

```sql
CREATE TABLE employee (
    employee_id CHAR(8) PRIMARY KEY,
    manager_id  CHAR(8),
    first_name  VARCHAR(25),
    last_name   VARCHAR(25),
    FOREIGN KEY (manager_id) REFERENCES employee(employee_id)
);
```

### Beginner Translation

Optional in the ERD may become nullable in the table.

---

## Relationship-to-Table Decision Guide

Use this guide:

| ERD Situation | Table Design |
|---|---|
| Strong entity | Create a table |
| Simple attribute | Create a column |
| Composite attribute | Split into simple columns |
| Multivalued attribute | Create a related table |
| One-to-many relationship | Add foreign key to many side |
| One-to-one relationship | Add foreign key with UNIQUE if needed |
| Many-to-many relationship | Create bridge table |
| Relationship has attributes | Put attributes on bridge/relationship table |
| Weak entity | Include parent key in child table |
| Recursive relationship | Add self-referencing foreign key |
| Ternary relationship | Create table with foreign keys to all participants |

---

## The “Blueprint to Building” Analogy

The ERD is like an architectural blueprint.

The tables are the actual rooms and walls.

The primary keys are room numbers.

The foreign keys are doors between rooms.

The constraints are building codes.

If the blueprint says two rooms must connect, the table design needs a foreign key.

If the blueprint says something is required, the table design may need `NOT NULL`.

If the blueprint says many things connect to many things, the table design needs a hallway or bridge table.

---

## Key Terms

| Term | Meaning |
|---|---|
| Relational schema | The table structure of a database |
| Table | Storage structure created from an entity |
| Column | Storage field created from an attribute |
| Primary key | Column or columns that uniquely identify a row |
| Foreign key | Column that references a primary key in another table |
| Bridge table | Table that resolves a many-to-many relationship |
| Composite attribute | Attribute that should be split into smaller columns |
| Multivalued attribute | Attribute that should become a related table |
| Weak entity | Entity dependent on another for identity |
| Recursive relationship | Relationship where a table references itself |
| NOT NULL | Constraint requiring a value |
| UNIQUE | Constraint preventing duplicate values |
| Referential integrity | Rule that foreign keys must point to valid rows |

---

## Quick Check

Answer these in your own words:

1. What does an entity become in a relational database?
2. What does an attribute become?
3. What does a key attribute become?
4. Where does the foreign key go in a one-to-many relationship?
5. Why do many-to-many relationships need bridge tables?
6. What should happen to a composite attribute like address?
7. What should happen to a multivalued attribute like phone numbers?
8. How does mandatory participation often show up in SQL?
9. What is a weak entity?
10. What is a self-referencing foreign key?

---

## Practice Exercise

Convert this mini ERD into table definitions.

```text
AUTHOR
- author_id
- first_name
- last_name

BOOK
- book_id
- title
- publication_year

Each AUTHOR may write many BOOKs.
Each BOOK must be written by one AUTHOR.
```

Questions:

```text
1. What tables do you need?
2. What are the primary keys?
3. Which table gets the foreign key?
4. Should the foreign key allow NULL?
5. Write the CREATE TABLE statements.
```

Starter answer shape:

```sql
CREATE TABLE author (
    author_id CHAR(8) PRIMARY KEY,
    first_name VARCHAR(25),
    last_name VARCHAR(25)
);

CREATE TABLE book (
    book_id CHAR(8) PRIMARY KEY,
    author_id CHAR(8) NOT NULL,
    title VARCHAR(100),
    publication_year INTEGER,
    FOREIGN KEY (author_id) REFERENCES author(author_id)
);
```

---

## Summary

Converting an ERD to tables is the bridge between database design and SQL implementation.

The key pattern is:

```text
Entity → Table
Attribute → Column
Key attribute → Primary key
One-to-many → Foreign key on many side
Many-to-many → Bridge table
Relationship details → Columns on bridge table
Mandatory relationship → NOT NULL
Optional relationship → Nullable foreign key
```

A good relational schema should preserve the meaning of the ERD.

The diagram tells the story.

The tables enforce the story.

---

## Source Notes

This lesson was drafted from the uploaded ERD-to-relational-model, database design, SQL DDL, and table specification materials, especially the readings on converting entities to tables, attributes to columns, primary keys, foreign keys, bridge entities, many-to-many resolution, weak entities, recursive relationships, and physical table design.
