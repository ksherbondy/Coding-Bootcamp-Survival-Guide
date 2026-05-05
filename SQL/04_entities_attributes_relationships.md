# 04 — Entities, Attributes, and Relationships: Turning Real Life Into a Data Model

## Big Idea

Before a database becomes tables and SQL, it starts as a real-world situation.

A business has customers.

A school has students.

A library has books.

A hospital has patients.

A store has products and sales.

Database design begins by asking:

```text
What real-world things do we need to track?
What details do we need to know about them?
How do those things connect to each other?
```

Those three questions lead to the three core building blocks of early database design:

```text
Entities
Attributes
Relationships
```

For beginners, this is the first major abstraction.

We are taking a messy real-world situation and turning it into a clean structure a database can understand.

---

## The Core Pattern

A simple way to remember the process is:

```text
Nouns       → Entities
Descriptions → Attributes
Verbs       → Relationships
```

Example sentence:

```text
A customer buys a car.
```

Break it apart:

```text
customer → entity
car      → entity
buys     → relationship
```

Now add details:

```text
customer has first name, last name, phone number
car has VIN, make, model, color
```

Those details are attributes.

This is the heart of database modeling.

---

## What Is an Entity?

An **entity** is an important thing the system needs to store information about.

An entity can be:

```text
a person
a place
a thing
an event
a concept
```

Examples:

| Category | Example Entities |
|---|---|
| Person | customer, student, employee, patient |
| Place | store, classroom, dealership, warehouse |
| Thing | car, book, product, tool |
| Event | sale, appointment, enrollment, checkout |
| Concept | course, account, membership, policy |

### Beginner Translation

An entity is something worth making a folder for.

If the business says:

```text
We need to keep track of these.
```

then it may be an entity.

---

## Entity vs. Entity Instance

This part matters.

An **entity** is the category.

An **entity instance** is one specific example.

| Entity | Entity Instance |
|---|---|
| customer | Jane Smith |
| car | 2021 Honda Civic VIN 123 |
| student | Maria Garcia |
| course | Introduction to SQL |
| sale | Sale #1001 |

### Beginner Translation

The entity is the blank form.

The entity instance is one filled-out form.

```text
CUSTOMER = the kind of thing
Jane Smith = one actual customer
```

When this becomes a table:

```text
Entity          → table
Entity instance → row
```

---

## How to Find Entities

Start with a plain-English description of the system.

Example:

```text
A dealership sells vehicles to customers.
Salespeople help customers with purchases.
Each dealership has many salespeople.
Customers may trade in vehicles during a sale.
```

Look for important nouns:

```text
dealership
vehicle
customer
salesperson
purchase/sale
trade-in
```

Those nouns are candidates for entities.

### Candidate Entity Test

Ask:

```text
Do we need to store multiple examples of this?
Do we need to remember details about it?
Does it participate in relationships with other things?
Would the business care if we lost track of it?
```

If the answer is yes, it is probably an entity.

---

## What Is an Attribute?

An **attribute** is a detail that describes an entity.

Example:

```text
CUSTOMER
- customer_id
- first_name
- last_name
- phone_number
- email
```

Each bullet is an attribute.

For a car:

```text
CAR
- vin
- make
- model
- color
- mileage
- price
```

### Beginner Translation

If the entity is a form, attributes are the blanks on the form.

```text
First Name: _______
Last Name:  _______
Phone:      _______
```

When this becomes a table:

```text
Attribute → column
```

---

## Attribute vs. Value

An attribute is the type of detail.

A value is the actual data stored for one instance.

| Attribute | Value |
|---|---|
| first_name | Jane |
| last_name | Smith |
| state | MD |
| mileage | 45200 |

### Beginner Translation

The attribute is the question.

The value is the answer.

```text
Attribute: first_name
Value: Jane
```

---

## Simple Attributes

A **simple attribute** cannot be broken down into smaller useful parts for the database.

Examples:

```text
age
price
state
zip_code
vin
```

A value like `MD` for state is already atomic enough for most systems.

### Beginner Translation

A simple attribute is one clean piece of information.

---

## Composite Attributes

A **composite attribute** can be broken into smaller parts.

Example:

```text
name
```

could be broken into:

```text
first_name
middle_name
last_name
```

Another example:

```text
address
```

could be broken into:

```text
street
city
state
zip_code
```

### Why This Matters

If you store a full name as one field:

```text
Jane Marie Smith
```

then searching or sorting by last name becomes harder.

If you store it as separate attributes:

```text
first_name  = Jane
middle_name = Marie
last_name   = Smith
```

then the database can work with each part clearly.

### Beginner Translation

Composite attributes are bundles.

Sometimes the bundle should be unpacked before it becomes a table.

---

## Single-Valued Attributes

A **single-valued attribute** has one value for each entity instance.

Example:

```text
A car has one VIN.
A student has one student ID.
A sale has one sale date.
```

### Beginner Translation

One row gets one value for that attribute.

---

## Multi-Valued Attributes

A **multi-valued attribute** can have more than one value for one entity instance.

Example:

```text
A customer may have multiple phone numbers.
A student may have multiple email addresses.
A product may belong to multiple categories.
```

Bad design:

| customer_id | customer_name | phone_numbers |
|---|---|---|
| C001 | Jane Smith | 555-1111, 555-2222 |

Better design:

```text
CUSTOMER
- customer_id
- first_name
- last_name

CUSTOMER_PHONE
- customer_id
- phone_number
- phone_type
```

### Beginner Translation

If one cell needs a list, that is a warning sign.

A list often wants to become its own related table later.

---

## Derived Attributes

A **derived attribute** can be calculated from other data.

Example:

```text
age
```

can be calculated from:

```text
date_of_birth
```

Another example:

```text
order_total
```

can be calculated from:

```text
quantity × unit_price
```

### Should Derived Attributes Be Stored?

Sometimes yes, sometimes no.

For beginners, the safe rule is:

```text
If it can be reliably calculated from stored data,
do not rush to store it separately.
```

Why?

Because stored derived values can become inconsistent.

Example:

```text
date_of_birth = 2000-01-01
age = 21
```

Eventually the age becomes wrong unless it is updated.

### Beginner Translation

A derived attribute is an answer the database can calculate.

Do not store the calculator result unless you have a good reason.

---

## Key Attributes

A **key attribute** uniquely identifies an entity instance.

Example:

```text
CUSTOMER
- customer_id
```

```text
CAR
- vin
```

The key matters because names and descriptions are often not unique.

Two customers can have the same name.

Two products can have similar descriptions.

Two people can live at the same address.

A key gives the database one exact way to identify one exact record.

### Beginner Translation

A key attribute is the label that says:

```text
This one. Exactly this one.
```

---

## Natural Keys and Artificial Keys

A **natural key** already exists in the real world.

Example:

```text
VIN for a vehicle
ISBN for a book edition
email address in some systems
```

An **artificial key** is created by the system.

Example:

```text
customer_id
student_id
order_id
sale_id
```

### Which Is Better?

It depends.

Natural keys can be useful, but they may change, be mistyped, or have exceptions.

Artificial keys are often simpler because the system controls them.

### Beginner Translation

A natural key comes from the world.

An artificial key is assigned by the database.

---

## What Is a Relationship?

A **relationship** describes how entities are connected.

Example:

```text
A customer places an order.
A student enrolls in a course.
An employee works for a department.
A book is checked out by a member.
```

Relationships are often found by looking for verbs or verb phrases.

| Sentence | Entity 1 | Relationship | Entity 2 |
|---|---|---|---|
| Customer places order | customer | places | order |
| Student enrolls in course | student | enrolls in | course |
| Employee works for department | employee | works for | department |
| Member checks out book | member | checks out | book |

### Beginner Translation

A relationship is the action or connection between things.

---

## Relationship Sentences

A good way to test relationships is to write them as paired sentences.

Example:

```text
Each customer may place many orders.
Each order must belong to one customer.
```

This pair tells us the relationship direction and quantity.

Another example:

```text
Each department may have many employees.
Each employee works for one department.
```

These sentences are powerful because they force you to think clearly.

---

## Why Relationship Sentences Matter

Beginners often draw lines between entities too quickly.

But a line without a rule is vague.

Instead of only saying:

```text
CUSTOMER is related to ORDER
```

say:

```text
Each CUSTOMER may place zero or more ORDERs.
Each ORDER must be placed by exactly one CUSTOMER.
```

Now the relationship has meaning.

This helps later when creating foreign keys.

---

## Cardinality: How Many?

**Cardinality** describes how many instances of one entity can be associated with instances of another.

The major types are:

```text
one-to-one
one-to-many
many-to-many
```

---

## One-to-One Relationships

A **one-to-one** relationship means one instance of Entity A relates to one instance of Entity B.

Example:

```text
Each person has one passport record.
Each passport record belongs to one person.
```

This might be modeled as:

```text
PERSON 1 — 1 PASSPORT
```

### Beginner Translation

One thing connects to one thing.

---

## One-to-Many Relationships

A **one-to-many** relationship means one instance of Entity A can relate to many instances of Entity B.

Example:

```text
One customer can place many orders.
Each order belongs to one customer.
```

This is very common.

```text
CUSTOMER 1 — many ORDER
```

### Beginner Translation

One parent can have many children.

When this becomes tables, the foreign key usually goes on the “many” side.

```text
order.customer_id
```

---

## Many-to-Many Relationships

A **many-to-many** relationship means many instances on both sides can connect.

Example:

```text
A student can enroll in many courses.
A course can have many students.
```

This is not stored directly as a simple foreign key.

Instead, we create a bridge entity.

```text
STUDENT → ENROLLMENT ← COURSE
```

The bridge records the connection.

### Beginner Translation

A many-to-many relationship usually needs a receipt, log, or sign-up record between the two things.

---

## Bridge Entities

A **bridge entity** resolves a many-to-many relationship.

Example:

```text
STUDENT
COURSE
```

Many-to-many:

```text
A student can take many courses.
A course can have many students.
```

Bridge:

```text
ENROLLMENT
```

Now we have:

```text
One student can have many enrollments.
One course can have many enrollments.
Each enrollment connects one student to one course.
```

The bridge can also store details about the relationship:

```text
enrollment_date
grade
status
```

### Beginner Translation

The bridge entity is the event or record that proves the connection happened.

---

## Optional vs. Mandatory Participation

Relationships also have participation rules.

### Optional

A customer may place an order.

That means a customer can exist with no orders yet.

```text
CUSTOMER optional participation in ORDER
```

### Mandatory

An order must belong to a customer.

That means an order cannot exist without a customer.

```text
ORDER mandatory participation in CUSTOMER
```

### Beginner Translation

Optional means:

```text
Can exist without the relationship.
```

Mandatory means:

```text
Must have the relationship to be valid.
```

---

## Entity or Attribute?

One of the hardest beginner questions is:

```text
Should this be an entity or an attribute?
```

Example:

```text
phone number
```

If each customer only has one phone number, it might be an attribute.

```text
CUSTOMER.phone_number
```

But if each customer can have many phone numbers, and each phone number has details like type, preferred status, or verification date, then it may need its own entity/table.

```text
CUSTOMER_PHONE
```

### Entity Test

Make it an entity if:

```text
You need to store multiple of it.
It has its own attributes.
It participates in relationships.
It has its own lifecycle.
The business talks about it as a thing.
```

Keep it as an attribute if:

```text
It is just one simple detail about another entity.
```

---

## Entity or Relationship?

Sometimes a concept feels like a relationship but needs to become an entity.

Example:

```text
Customer buys product.
```

The relationship is “buys.”

But in a real store, the purchase itself has details:

```text
sale_date
quantity
price_paid
payment_method
receipt_number
```

So `SALE` becomes an entity.

```text
CUSTOMER → SALE ← PRODUCT
```

### Beginner Translation

If the relationship needs details, it may deserve to become an entity.

---

## A Full Example: Library System

Plain English requirements:

```text
A library has members.
Members check out books.
Each book has a title, author, and ISBN.
A member can check out many books.
A book can be checked out many times over its lifetime.
The system must track checkout date and return date.
```

### Step 1: Find Entities

Important nouns:

```text
library
member
book
checkout
author
```

For a simple version, we might choose:

```text
MEMBER
BOOK
CHECKOUT
```

### Step 2: Find Attributes

```text
MEMBER
- member_id
- first_name
- last_name
- phone_number
- email

BOOK
- book_id
- isbn
- title
- author_name

CHECKOUT
- checkout_id
- member_id
- book_id
- checkout_date
- return_date
```

### Step 3: Find Relationships

```text
Each MEMBER may have many CHECKOUTs.
Each CHECKOUT belongs to one MEMBER.

Each BOOK may appear in many CHECKOUTs.
Each CHECKOUT refers to one BOOK.
```

### Step 4: Notice the Bridge

`CHECKOUT` connects `MEMBER` and `BOOK`.

It also stores relationship details:

```text
checkout_date
return_date
```

So checkout is not just a line.

It is an event entity.

---

## A Full Example: School System

Plain English requirements:

```text
Students enroll in courses.
Each student has a student ID, name, and email.
Each course has a course ID, course name, and credit count.
A student can enroll in many courses.
A course can have many students.
The system tracks enrollment date and grade.
```

### Entities

```text
STUDENT
COURSE
ENROLLMENT
```

### Attributes

```text
STUDENT
- student_id
- first_name
- last_name
- email

COURSE
- course_id
- course_name
- credits

ENROLLMENT
- student_id
- course_id
- enrollment_date
- grade
```

### Relationships

```text
Each STUDENT may have many ENROLLMENTs.
Each ENROLLMENT belongs to one STUDENT.

Each COURSE may have many ENROLLMENTs.
Each ENROLLMENT belongs to one COURSE.
```

### Beginner Translation

Enrollment is the sign-up record.

It proves that a specific student is connected to a specific course.

---

## Common Beginner Mistakes

### Mistake 1: Turning Every Noun Into an Entity

Not every noun should become an entity.

Example:

```text
customer name
```

Name is probably an attribute, not an entity.

### Mistake 2: Hiding Multiple Values in One Attribute

Bad:

```text
customer_phone_numbers
```

with values like:

```text
555-1111, 555-2222
```

Better:

```text
CUSTOMER_PHONE
```

### Mistake 3: Skipping Relationship Sentences

If you cannot explain the relationship in plain English, the design probably is not clear yet.

### Mistake 4: Forgetting the Bridge Entity

Many-to-many relationships almost always need a bridge.

### Mistake 5: Confusing an Entity With One Example

`CUSTOMER` is an entity.

`Jane Smith` is an instance.

---

## The “Folder and Forms” Analogy

Imagine an office filing cabinet.

An entity is a folder category:

```text
CUSTOMER
CAR
SALE
```

Attributes are the blanks on the forms inside the folder:

```text
first_name
last_name
phone_number
```

Entity instances are completed forms:

```text
Jane Smith's customer record
```

Relationships are cross-references between forms:

```text
This sale form points to Jane Smith's customer form.
This sale form points to this car form.
```

Bridge entities are special forms that record an event or connection:

```text
SALE
ENROLLMENT
CHECKOUT
```

They answer:

```text
Who was connected?
What were they connected to?
When did it happen?
What details belong to that connection?
```

---

## Key Terms

| Term | Meaning |
|---|---|
| Entity | Important thing the system tracks |
| Entity instance | One specific example of an entity |
| Attribute | Detail that describes an entity |
| Value | Actual data stored in an attribute |
| Simple attribute | Attribute that cannot usefully be divided further |
| Composite attribute | Attribute made of smaller parts |
| Single-valued attribute | Has one value per entity instance |
| Multi-valued attribute | Can have multiple values per entity instance |
| Derived attribute | Can be calculated from other data |
| Key attribute | Uniquely identifies an entity instance |
| Relationship | Association between entities |
| Cardinality | How many instances can connect |
| Bridge entity | Entity used to resolve many-to-many relationships |
| Optional participation | Entity can exist without the relationship |
| Mandatory participation | Entity must have the relationship |

---

## Quick Check

Answer these in your own words:

1. What is an entity?
2. What is the difference between an entity and an entity instance?
3. What is an attribute?
4. What is the difference between an attribute and a value?
5. Why might a phone number become its own related table?
6. What is a relationship?
7. What is a many-to-many relationship?
8. Why do many-to-many relationships usually need bridge entities?
9. What is the difference between optional and mandatory participation?
10. How can verbs help identify relationships?

---

## Practice Exercise

Read this mini requirement:

```text
A gym tracks members and classes.
Members can sign up for many classes.
Each class can have many members.
Each class has a name, date, start time, and instructor.
The gym wants to know when each member signed up for each class.
```

Answer:

```text
1. What are the entities?
2. What are the attributes for each entity?
3. What relationships exist?
4. Is there a many-to-many relationship?
5. What bridge entity would you create?
6. What details belong on the bridge entity?
```

Possible starting point:

```text
MEMBER
CLASS
CLASS_SIGNUP
```

---

## Summary

Entities, attributes, and relationships are the first building blocks of database design.

The beginner pattern is:

```text
Nouns become entities.
Details become attributes.
Verbs become relationships.
How many becomes cardinality.
Many-to-many becomes a bridge entity.
```

The goal is to translate real-world language into a clean data model.

Before we write SQL, we need to know what the world looks like.

Database design begins by listening carefully.

---

## Source Notes

This lesson was drafted from the uploaded database design and ER model materials, especially the readings on entities, attributes, relationships, relationship sentences, cardinality, keys, composite attributes, multivalued attributes, derived attributes, and bridge entities.
