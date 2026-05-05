# Database Foundations: How Real-World Information Becomes Tables

## Big Idea

A database is not just a place where information is stored. A database is a structured way to represent something from the real world so that a computer can store it, search it, protect it, and update it without everything turning into a mess.

A simple contact list, a school gradebook, a store inventory, a banking system, and a vehicle sales website are all examples of real-world problems that can be modeled with a database.

So the first lesson is this:

> A database is not born as tables.  
> A database starts as a real-world problem.

Before we write SQL, before we create tables, before we worry about primary keys, we need to ask:

**What real-world things are we trying to remember?**

---

## Step 1: Start With the Real World

Imagine we are building a database for a small car dealership.

Before thinking about tables, columns, or SQL, we describe the business in plain English:

> The dealership sells cars to customers. Salespeople help customers buy vehicles. Each vehicle has information like VIN, make, model, color, and price. Each customer has a name, address, and phone number.

That paragraph is where database design begins.

Not with code.

Not with tables.

With the story.

A good database design starts by listing what data you need and what you want to do with it later. At this stage, avoid thinking in tables and columns too early. Instead, ask:

**What do I need to know?**

That is important for beginners because jumping straight to tables too early can cause bad design.

---

## Step 2: Find the Nouns — These Become Entities

In database design, the important “things” in the real-world problem are called **entities**.

A simple way to find entities is to look for important nouns.

From the dealership story:

- car
- customer
- salesperson
- dealership
- sale

These are probably entities because they are things the business needs to track.

An entity is not always a physical object. It can be a person, place, thing, event, or concept.

### Beginner Translation

An **entity** is a category of thing.

Not one specific customer.

Not “Bob Smith.”

But the idea of a customer.

So:

| Real-world example | Entity |
|---|---|
| Bob Smith | CUSTOMER |
| 2020 Honda Civic | CAR |
| Jane the salesperson | SALESPERSON |
| The act of buying a car | SALE |

The entity is the blueprint.  
The individual example is one record that will eventually go into the database.

---

## Step 3: Find the Details — These Become Attributes

Once we know the entities, we ask:

**What do we need to know about each one?**

Those details are called **attributes**.

For example:

### CUSTOMER

A customer might have:

- customer ID
- first name
- last name
- street address
- city
- state
- zip code
- phone number

### CAR

A car might have:

- VIN
- make
- model
- color
- mileage
- suggested retail price

### Beginner Translation

An **attribute** is a detail about an entity.

If the entity is like a folder label, the attributes are the blanks on the form inside the folder.

For a customer form:

```text
Customer ID: _______
First Name:  _______
Last Name:   _______
Phone:       _______
```

Later, those attributes usually become columns in a table.

---

## Step 4: Find the Verbs — These Become Relationships

Entities do not just sit alone. They interact.

Customers **buy** cars.  
Salespeople **help** customers.  
Dealerships **employ** salespeople.  
Cars **are sold in** sales.

These connections are called **relationships**.

A useful grammar analogy is:

> Nouns become entities.  
> Descriptions become attributes.  
> Verbs become relationships.

---

## Step 5: Ask “How Many?” — Cardinality

Once we know two things are related, we ask:

**How many of one thing can connect to how many of the other thing?**

This is called **cardinality**.

For example:

### One-to-Many

One customer can make many purchases.

```text
One CUSTOMER → many SALES
```

But each sale belongs to one customer.

```text
One SALE → one CUSTOMER
```

Together, that gives us a **one-to-many** relationship.

### Many-to-Many

A customer can buy many cars.

A car can be sold more than once over time if it is traded in and resold.

So at first, we might say:

```text
CUSTOMER ↔ CAR
```

That is many-to-many.

But relational databases do not handle many-to-many relationships cleanly by directly connecting both tables. When we have a many-to-many relationship, we usually add a bridge entity.

So instead of this:

```text
CUSTOMER ↔ CAR
```

We create this:

```text
CUSTOMER → SALE ← CAR
```

The `SALE` entity becomes the record of the transaction.

### Beginner Translation

A bridge table is like a receipt.

The customer is one thing.  
The car is another thing.  
The sale is the event that connects them.

---

## Step 6: Draw the ERD

An **Entity Relationship Diagram**, or **ERD**, is a picture of the database design.

It helps us see:

- the entities
- the attributes
- the relationships
- the cardinality

In ER diagrams, entities are often shown as rectangles, attributes as ovals, and relationships as diamonds, depending on the notation being used.

For beginners, the ERD is useful because it turns database design into something visual.

Instead of staring at code, you can look at the structure.

---

## Step 7: Turn the ERD Into Tables

Once the ERD makes sense, we can turn it into relational tables.

The basic mapping is:

| ERD Concept | Relational Database Concept |
|---|---|
| Entity | Table |
| Attribute | Column |
| Entity instance | Row |
| Relationship | Foreign key or bridge table |

So this:

```text
CUSTOMER
- customer_id
- first_name
- last_name
- phone
```

Becomes this:

```sql
CREATE TABLE customer (
    customer_id CHAR(8),
    first_name  VARCHAR(25),
    last_name   VARCHAR(25),
    phone       VARCHAR(12)
);
```

At this point, the design has crossed from concept into structure.

---

## Step 8: Add Keys

A **key** is how the database identifies a row.

A customer might have a `customer_id`.

A car might have a `vin`.

A sale might have a `sale_id`, or it might use a combination of values depending on the design.

A primary key uniquely identifies one row in a table.

### Beginner Translation

A primary key is the database’s way of saying:

> “This row. Not a row like it. This exact row.”

Names are not usually good primary keys because two people can have the same name.

Phone numbers can change.

Addresses can change.

So we often create an ID.

---

## Step 9: Connect Tables With Foreign Keys

A **foreign key** is a column that points to a primary key in another table.

For example:

```text
customer.customer_id
sale.customer_id
```

The `customer_id` in `customer` is the primary key.

The `customer_id` in `sale` is the foreign key.

It says:

> “This sale belongs to this customer.”

### One-to-Many Rule

If one customer can have many sales:

```text
CUSTOMER 1 → many SALE
```

Then the foreign key goes in `sale`.

```sql
CREATE TABLE sale (
    sale_id     CHAR(8),
    customer_id CHAR(8)
);
```

Why?

Because each sale needs to know which customer it belongs to.

---

## Step 10: Query the Data With SQL

After the tables exist and contain data, SQL lets us ask questions.

A basic query looks like this:

```sql
SELECT customer_id,
       first_name,
       last_name
FROM customer;
```

The `SELECT` part says what columns we want.

The `FROM` part says what table we are using.

### Beginner Translation

SQL is not magic.

A `SELECT` query is basically asking:

```text
What do you want?
Where should I get it from?
Are there any conditions?
How should I sort it?
```

That becomes:

```sql
SELECT ...
FROM ...
WHERE ...
ORDER BY ...;
```

---

## Step 11: Format SQL Like You Expect Someone Else to Read It

SQL formatting does not usually change what the query does.

But it changes how easy the query is to read, debug, and maintain.

For example, this is hard to read:

```sql
SELECT id, FirstName, LASTNAME,c.nAme FROM people p left JOIN cities AS c on c.id=p.cityid;
```

This is easier:

```sql
SELECT p.person_id,
       p.first_name,
       p.last_name,
       c.name AS city_name
FROM person AS p
LEFT JOIN city AS c
       ON p.city_id = c.city_id;
```

### Beginner Translation

Formatting is not decoration.

Formatting is part of communication.

Bad formatting makes the reader work harder.

Good formatting says:

> “Here is the structure. You can trust your eyes.”

---

## Step 12: Normalize the Design

Normalization is the process of organizing tables so that data is not repeated unnecessarily.

Repeated data causes problems.

For example, imagine one giant `orders` table:

| order_id | customer_id | first_name | last_name | city | state |
|---|---|---|---|---|---|
| 1 | C001 | John | Smith | Adelphi | MD |
| 2 | C001 | John | Smith | Adelphi | MD |
| 3 | C001 | John | Smith | Adelphi | MD |

Every order repeats the customer’s name and location.

That can cause three problems:

1. **Insertion anomaly** — you cannot add a customer until they place an order.
2. **Deletion anomaly** — deleting the last order might accidentally delete the only copy of the customer data.
3. **Modification anomaly** — if the customer moves, you have to update multiple rows.

So we split the design:

```text
CUSTOMER
- customer_id
- first_name
- last_name
- city
- state

ORDER
- order_id
- customer_id
- order_date
```

Now customer information lives in one place.

Orders point to the customer.

That is the point of relational design.

---

# Final Mental Model

A beginner should walk away with this chain:

```text
Real-world problem
        ↓
Important nouns
        ↓
Entities
        ↓
Details about entities
        ↓
Attributes
        ↓
Verbs between entities
        ↓
Relationships
        ↓
How many?
        ↓
Cardinality
        ↓
ERD
        ↓
Tables, columns, keys
        ↓
SQL
        ↓
Clean formatting
        ↓
Normalization
```

Or even simpler:

> We listen to the real world, find its nouns, describe them, connect them, draw them, table them, query them, and clean them up.

That is the whole database design journey.
