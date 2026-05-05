# 05 — ERDs and Cardinality: Drawing the Database Before Building It

## Big Idea

An **Entity Relationship Diagram**, or **ERD**, is a picture of a database design.

Before we write `CREATE TABLE`, before we add foreign keys, and before we write SQL queries, an ERD helps us see the structure of the data.

An ERD shows:

```text
Entities
Attributes
Relationships
Cardinality
Participation
```

For beginners, the ERD is valuable because it turns database design into something visual.

Instead of trying to hold the whole system in your head, you can draw it.

---

## Why ERDs Matter

A database is supposed to model some part of the real world.

But real-world systems can get messy fast.

Example:

```text
Customers buy cars.
Salespeople help customers.
Cars belong to dealerships.
Customers may trade in cars.
A sale connects a customer, a car, and a salesperson.
```

That is a lot to track in plain English.

An ERD lets us turn the messy paragraph into a visual map.

```text
CUSTOMER → SALE ← CAR
              ↑
        SALESPERSON
```

This picture helps us ask:

```text
What things exist?
How are they connected?
How many of each thing can connect?
Which connections are required?
Where will the foreign keys go later?
```

The ERD is not the database yet.

It is the blueprint.

---

## ERD Mental Model

Think of an ERD like a wiring diagram.

Entities are the components.

Relationships are the wires.

Cardinality tells you how many connections are allowed.

Participation tells you whether the wire is required or optional.

If the wiring diagram is wrong, the system built from it will probably be wrong too.

---

## The Main ERD Symbols

Different ERD notations exist, but the basic ideas are consistent.

Common symbols include:

| Concept | Common Symbol |
|---|---|
| Entity | Rectangle |
| Attribute | Oval |
| Relationship | Diamond |
| Weak entity | Double rectangle |
| Multivalued attribute | Double oval |
| Derived attribute | Dashed oval |
| Key attribute | Underlined attribute |
| Cardinality | 1, M, N, crow's foot, or min/max notation |

Some tools use crow's foot notation instead of Chen notation.

The symbols may change, but the meaning stays the same.

---

## Entities in ERDs

An entity is usually drawn as a rectangle.

```text
+----------+
| CUSTOMER |
+----------+
```

The rectangle represents the category of thing we are tracking.

Examples:

```text
CUSTOMER
ORDER
PRODUCT
STUDENT
COURSE
BOOK
MEMBER
SALE
```

### Beginner Translation

A rectangle means:

```text
This is an important thing the database needs to remember.
```

---

## Attributes in ERDs

An attribute is often drawn as an oval connected to its entity.

```text
             (first_name)
                  |
+----------+  (last_name)
| CUSTOMER |------|
+----------+  (phone_number)
                  |
             (customer_id)
```

In many modern diagramming tools, attributes are listed inside the entity box instead:

```text
+----------------+
| CUSTOMER       |
+----------------+
| customer_id PK |
| first_name     |
| last_name      |
| phone_number   |
+----------------+
```

Both styles are trying to communicate the same thing.

### Beginner Translation

Attributes are the details we need to know about an entity.

---

## Key Attributes

A key attribute uniquely identifies an entity instance.

In Chen notation, the key attribute is often underlined.

Text version:

```text
CUSTOMER
- customer_id  ← key
- first_name
- last_name
```

In table-style ERDs, it might be shown like this:

```text
+----------------+
| CUSTOMER       |
+----------------+
| PK customer_id |
| first_name     |
| last_name      |
+----------------+
```

### Beginner Translation

The key is the unique label for each record.

It says:

```text
This customer, not just any customer.
```

---

## Relationships in ERDs

A relationship is often drawn as a diamond.

```text
+----------+       +-------+       +-------+
| CUSTOMER |-------| BUYS  |-------|  CAR  |
+----------+       +-------+       +-------+
```

In crow's foot notation, the relationship is usually shown as a line between entities with symbols at the ends.

```text
CUSTOMER ─────── SALE
```

The relationship name is often a verb or verb phrase:

```text
buys
places
contains
works_for
enrolls_in
checks_out
```

### Beginner Translation

A relationship tells us how two entities are connected.

---

## Relationship Sentences

Before drawing a line, write the relationship in plain English.

Example:

```text
Each CUSTOMER may place many ORDERs.
Each ORDER must be placed by one CUSTOMER.
```

This is better than simply saying:

```text
CUSTOMER is related to ORDER.
```

The sentence tells us more.

It gives us:

```text
the relationship
the direction
the quantity
whether it is optional or required
```

### Relationship Sentence Template

Use this pattern:

```text
Each [ENTITY A] may/must [relationship phrase] one or more [ENTITY B].
Each [ENTITY B] may/must [relationship phrase] one [ENTITY A].
```

Example:

```text
Each STUDENT may enroll in many COURSEs.
Each COURSE may have many STUDENTs.
```

Now we know this is many-to-many.

---

## Cardinality

**Cardinality** tells us how many instances of one entity can be associated with instances of another entity.

The three most common relationship cardinalities are:

```text
one-to-one
one-to-many
many-to-many
```

Cardinality answers:

```text
How many?
```

---

## One-to-One

A **one-to-one** relationship means one instance of Entity A connects to one instance of Entity B.

Example:

```text
Each PERSON has one PASSPORT_RECORD.
Each PASSPORT_RECORD belongs to one PERSON.
```

Diagram idea:

```text
PERSON 1 ─── 1 PASSPORT_RECORD
```

### When One-to-One Happens

One-to-one relationships are less common than one-to-many.

They can happen when:

```text
Security-sensitive details are separated.
Optional details are separated.
A table is split for performance or organization.
Two entities are conceptually different but share identity.
```

### Beginner Translation

One row over here matches one row over there.

---

## One-to-Many

A **one-to-many** relationship means one instance of Entity A can connect to many instances of Entity B.

Example:

```text
Each CUSTOMER may place many ORDERs.
Each ORDER must belong to one CUSTOMER.
```

Diagram idea:

```text
CUSTOMER 1 ─── many ORDER
```

This is probably the most common database relationship.

### Beginner Translation

One parent can have many children.

```text
One customer → many orders
One department → many employees
One author → many books
One dealership → many cars
```

---

## Many-to-Many

A **many-to-many** relationship means many instances on both sides can connect.

Example:

```text
Each STUDENT may enroll in many COURSEs.
Each COURSE may have many STUDENTs.
```

Diagram idea:

```text
STUDENT many ─── many COURSE
```

This is a warning sign in relational design because many-to-many relationships usually need to be resolved with a bridge entity.

---

## Resolving Many-to-Many Relationships

Relational databases do not usually store a many-to-many relationship directly.

Instead, we create a bridge entity.

Bad direct model:

```text
STUDENT many ─── many COURSE
```

Better model:

```text
STUDENT 1 ─── many ENROLLMENT many ─── 1 COURSE
```

Or:

```text
STUDENT → ENROLLMENT ← COURSE
```

The bridge entity records the connection.

### ENROLLMENT Example

```text
ENROLLMENT
- student_id
- course_id
- enrollment_date
- grade
```

Now we can store details about the relationship itself.

### Beginner Translation

The bridge is the receipt.

It proves the relationship happened and stores details about it.

---

## Participation

Cardinality tells us:

```text
How many?
```

Participation tells us:

```text
Is the relationship required?
```

There are two common participation types:

```text
optional
mandatory
```

---

## Optional Participation

Optional means an entity instance can exist without being connected.

Example:

```text
A CUSTOMER may place zero or more ORDERs.
```

A customer can be in the database before placing an order.

So customer participation in orders is optional.

Text notation:

```text
CUSTOMER 0..many ORDER
```

### Beginner Translation

Optional means:

```text
It can exist without that connection.
```

---

## Mandatory Participation

Mandatory means an entity instance must be connected.

Example:

```text
An ORDER must belong to one CUSTOMER.
```

An order without a customer would not make sense.

So order participation in customer is mandatory.

Text notation:

```text
ORDER 1..1 CUSTOMER
```

### Beginner Translation

Mandatory means:

```text
It cannot be valid without that connection.
```

---

## Minimum and Maximum Cardinality

Some ERD notation uses minimum and maximum values.

Example:

```text
(0, N)
```

Means:

```text
minimum = 0
maximum = many
```

So the relationship is optional and many.

Example:

```text
(1, 1)
```

Means:

```text
minimum = 1
maximum = 1
```

So the relationship is mandatory and exactly one.

Example:

```text
(1, N)
```

Means:

```text
minimum = 1
maximum = many
```

So the relationship is mandatory and many.

### Beginner Translation

Min/max cardinality answers two questions:

```text
Minimum: Does it have to have any?
Maximum: How many can it have?
```

---

## Common Cardinality Patterns

| Pattern | Meaning | Example |
|---|---|---|
| 0..1 | Optional, at most one | Employee may have one parking pass |
| 1..1 | Required, exactly one | Order must have one customer |
| 0..N | Optional, many allowed | Customer may have many orders |
| 1..N | Required, many allowed | Course must have at least one section |

---

## Crow's Foot Notation

Crow's foot notation is common in modern database tools.

A crow's foot symbol means “many.”

Text approximation:

```text
CUSTOMER |——< ORDER
```

This means:

```text
One customer can have many orders.
```

Another way to write it:

```text
CUSTOMER 1 ───< ORDER many
```

### Beginner Translation

The crow's foot points to the many side.

If the crow's foot is near `ORDER`, then there can be many orders.

---

## How ERDs Become Foreign Keys

ERDs help us know where foreign keys go later.

For a one-to-many relationship:

```text
CUSTOMER 1 ─── many ORDER
```

The foreign key goes on the many side.

```text
ORDER.customer_id
```

Why?

Because each order needs to point back to the customer it belongs to.

### Beginner Rule

```text
In a one-to-many relationship,
put the foreign key on the many side.
```

Examples:

| Relationship | Foreign Key Goes In |
|---|---|
| One customer has many orders | order.customer_id |
| One department has many employees | employee.department_id |
| One author has many books | book.author_id |
| One dealership has many salespeople | salesperson.dealership_id |

---

## How ERDs Handle Many-to-Many Foreign Keys

For a many-to-many relationship:

```text
STUDENT many ─── many COURSE
```

Create a bridge table:

```text
ENROLLMENT
```

The bridge table gets foreign keys to both parent tables:

```text
ENROLLMENT.student_id
ENROLLMENT.course_id
```

Then the model becomes:

```text
STUDENT 1 ─── many ENROLLMENT many ─── 1 COURSE
```

### Beginner Rule

```text
Many-to-many becomes a bridge.
The bridge stores foreign keys to both sides.
```

---

## Weak Entities

A **weak entity** depends on another entity for its identity.

Example:

```text
ORDER
ORDER_LINE
```

An order line might not make sense without the order.

```text
ORDER_LINE
- order_id
- line_number
- product_id
- quantity
```

The `line_number` might only be unique inside one order.

So the full identity could be:

```text
order_id + line_number
```

### Beginner Translation

A weak entity is like a child record that needs its parent to be identified.

---

## Recursive Relationships

A **recursive relationship** happens when an entity relates to itself.

Example:

```text
An employee manages other employees.
```

Both sides are `EMPLOYEE`.

```text
EMPLOYEE ─── manages ─── EMPLOYEE
```

But the roles are different:

```text
manager
subordinate
```

### Example

```text
EMPLOYEE
- employee_id
- first_name
- last_name
- manager_id
```

Here, `manager_id` points back to another `employee_id`.

### Beginner Translation

A recursive relationship is when a row points to another row in the same table.

---

## Ternary Relationships

A **ternary relationship** involves three entities at once.

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

The relationship is not just supplier-to-part or part-to-project.

It involves all three together.

### Beginner Warning

Ternary relationships can be tricky.

Do not break them into separate binary relationships unless the meaning still stays correct.

### Beginner Translation

A ternary relationship is a three-way fact.

The relationship only makes sense when all three pieces are known.

---

## ERD Example: Customer Orders

Requirements:

```text
Customers place orders.
Each order belongs to one customer.
Each order contains one or more products.
Each product can appear in many orders.
The system must track quantity and price at the time of order.
```

### Entities

```text
CUSTOMER
ORDER
PRODUCT
ORDER_LINE
```

### Relationships

```text
CUSTOMER 1 ─── many ORDER
ORDER 1 ─── many ORDER_LINE
PRODUCT 1 ─── many ORDER_LINE
```

### Why ORDER_LINE Exists

At first, we may think:

```text
ORDER many ─── many PRODUCT
```

But that many-to-many relationship needs a bridge.

The bridge is `ORDER_LINE`.

It stores:

```text
order_id
product_id
quantity
unit_price
```

### Text ERD

```text
CUSTOMER
- customer_id
- first_name
- last_name

ORDER
- order_id
- customer_id
- order_date

PRODUCT
- product_id
- product_name
- current_price

ORDER_LINE
- order_id
- product_id
- quantity
- unit_price
```

Diagram:

```text
CUSTOMER 1 ───< ORDER 1 ───< ORDER_LINE >─── 1 PRODUCT
```

### Beginner Translation

A customer places an order.

An order has line items.

Each line item points to a product.

That is how real receipts work.

---

## ERD Example: School Enrollment

Requirements:

```text
Students enroll in courses.
Each student can enroll in many courses.
Each course can have many students.
The system tracks enrollment date and grade.
```

### Entities

```text
STUDENT
COURSE
ENROLLMENT
```

### Relationships

```text
STUDENT 1 ─── many ENROLLMENT
COURSE 1 ─── many ENROLLMENT
```

### Diagram

```text
STUDENT 1 ───< ENROLLMENT >─── 1 COURSE
```

### Why ENROLLMENT Exists

Because this is many-to-many:

```text
student ↔ course
```

The enrollment record proves:

```text
This student signed up for this course.
```

It also stores:

```text
enrollment_date
grade
status
```

---

## ERD Example: Employee Management

Requirements:

```text
Employees work in departments.
Each department can have many employees.
Each employee works in one department.
Some employees manage other employees.
```

### Entities

```text
DEPARTMENT
EMPLOYEE
```

### Relationships

```text
DEPARTMENT 1 ─── many EMPLOYEE
EMPLOYEE 1 ─── many EMPLOYEE
```

The second relationship is recursive.

### Text ERD

```text
DEPARTMENT
- department_id
- department_name

EMPLOYEE
- employee_id
- department_id
- manager_id
- first_name
- last_name
```

`department_id` points to `DEPARTMENT`.

`manager_id` points to another `EMPLOYEE`.

---

## Common Beginner Mistakes

### Mistake 1: Drawing Lines Without Meaning

A line between two entities should have a relationship sentence.

Bad:

```text
CUSTOMER connected to ORDER
```

Better:

```text
Each CUSTOMER may place many ORDERs.
Each ORDER must belong to one CUSTOMER.
```

### Mistake 2: Forgetting Cardinality

A relationship without cardinality is incomplete.

You need to know whether it is:

```text
one-to-one
one-to-many
many-to-many
```

### Mistake 3: Ignoring Optional vs. Mandatory

Ask:

```text
Can this exist without that?
```

Example:

```text
Can a customer exist without an order? Yes.
Can an order exist without a customer? Usually no.
```

### Mistake 4: Keeping Many-to-Many Directly

Many-to-many relationships usually need a bridge.

### Mistake 5: Putting the Foreign Key on the Wrong Side

For one-to-many:

```text
Foreign key goes on the many side.
```

---

## ERD Checklist

When reviewing an ERD, ask:

```text
1. Does every entity represent one clear kind of thing?
2. Does every entity have a key?
3. Are the relationships named or explainable?
4. Does every relationship have cardinality?
5. Is participation optional or mandatory where needed?
6. Have many-to-many relationships been resolved with bridge entities?
7. Are multivalued attributes turned into related entities/tables?
8. Are weak entities clearly dependent on their parent?
9. Are recursive relationships labeled with roles?
10. Can the ERD be explained in plain English?
```

If you cannot explain the ERD in plain English, the design probably needs more work.

---

## The “Map Before the Road” Analogy

Building tables without an ERD is like pouring concrete before drawing the road map.

You might get somewhere.

But you may also create dead ends, loops, missing bridges, or intersections that do not make sense.

An ERD gives you the map first.

Then the tables become much easier to build.

---

## Key Terms

| Term | Meaning |
|---|---|
| ERD | Entity Relationship Diagram |
| Entity | Thing the database tracks |
| Attribute | Detail about an entity |
| Relationship | Connection between entities |
| Cardinality | How many instances can connect |
| Participation | Whether a relationship is optional or required |
| One-to-one | One instance connects to one instance |
| One-to-many | One instance connects to many instances |
| Many-to-many | Many instances connect to many instances |
| Bridge entity | Entity that resolves a many-to-many relationship |
| Weak entity | Entity that depends on another entity for identity |
| Recursive relationship | Entity relates to itself |
| Ternary relationship | Relationship involving three entities |
| Crow's foot | ERD notation symbol for “many” |

---

## Quick Check

Answer these in your own words:

1. What is the purpose of an ERD?
2. What does cardinality tell us?
3. What is the difference between cardinality and participation?
4. What is a one-to-many relationship?
5. Why do many-to-many relationships need bridge entities?
6. Where does the foreign key go in a one-to-many relationship?
7. What is a recursive relationship?
8. What is a weak entity?
9. Why should relationship sentences be written before drawing lines?
10. What does a crow's foot represent?

---

## Practice Exercise

Read this requirement:

```text
A music school tracks instructors, students, and lessons.
Each instructor can teach many lessons.
Each student can take many lessons.
Each lesson has a date, start time, end time, and room.
A lesson is taught by one instructor and attended by one student.
```

Answer:

```text
1. What are the entities?
2. What are the attributes?
3. What relationships exist?
4. What is the cardinality of each relationship?
5. Which relationships are optional?
6. Which relationships are mandatory?
7. Where would the foreign keys go?
```

Possible starting point:

```text
INSTRUCTOR 1 ───< LESSON >─── 1 STUDENT
```

---

## Summary

An ERD is the visual blueprint of a database.

It helps us see entities, attributes, relationships, cardinality, and participation before writing SQL.

The beginner chain is:

```text
Entity = thing
Attribute = detail
Relationship = connection
Cardinality = how many
Participation = required or optional
Bridge = resolves many-to-many
Foreign key = table-level connection created later
```

A good ERD should tell a clear story.

If the story is clear, the tables become much easier to build.

---

## Source Notes

This lesson was drafted from the uploaded ERD and database design materials, especially the readings on ER diagram symbols, relationship sentences, cardinality, weak entities, recursive relationships, ternary relationships, bridge entities, and ERD-to-table design rules.
