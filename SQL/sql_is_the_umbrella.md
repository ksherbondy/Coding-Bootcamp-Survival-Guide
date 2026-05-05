# SQL Is the Umbrella

## Big Idea

When beginners first learn SQL, it can feel like every command is just thrown into one big pile:

```sql
SELECT
INSERT
UPDATE
DELETE
CREATE
ALTER
DROP
GRANT
REVOKE
COMMIT
ROLLBACK
```

That gets confusing fast.

The important thing to understand is that **SQL is the umbrella**.

Under that umbrella are different categories of commands. Each category has a different job.

```text
SQL
├── DDL  → Data Definition Language
│         CREATE, ALTER, DROP
│         Changes database structure
│
├── DML  → Data Manipulation Language
│         SELECT, INSERT, UPDATE, DELETE
│         Works with the data inside tables
│
├── DCL  → Data Control Language
│         GRANT, REVOKE
│         Controls permissions
│
└── TCL  → Transaction Control Language
          COMMIT, ROLLBACK, SAVEPOINT
          Controls transaction behavior
```

---

## The Big Picture

SQL is the full language used to communicate with a relational database.

But not every SQL command does the same kind of work.

Some commands build the structure. Some commands work with the data. Some commands control who has access. Some commands control whether changes are saved or undone.

That is why SQL is usually divided into smaller command families.

---

## DDL: Data Definition Language

**DDL** stands for **Data Definition Language**.

DDL commands change the **structure** of the database.

That means DDL is used to create, modify, or remove things like:

```text
tables
columns
constraints
indexes
schemas
views
```

Common DDL commands include:

```sql
CREATE
ALTER
DROP
```

Example:

```sql
CREATE TABLE student (
    student_id INT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name  VARCHAR(50)
);
```

This does not add student data yet.

It creates the table structure where student data can later live.

### Think of DDL as the Blueprint Tools

DDL is like building or remodeling the database house.

```text
CREATE = build something new
ALTER  = remodel something that exists
DROP   = demolish something
```

---

## DML: Data Manipulation Language

**DML** stands for **Data Manipulation Language**.

DML commands work with the **data inside the tables**.

Common DML commands include:

```sql
SELECT
INSERT
UPDATE
DELETE
```

Examples:

```sql
SELECT first_name,
       last_name
FROM student;
```

```sql
INSERT INTO student (
    student_id,
    first_name,
    last_name
)
VALUES (
    1,
    'John',
    'Doe'
);
```

```sql
UPDATE student
SET last_name = 'Smith'
WHERE student_id = 1;
```

```sql
DELETE FROM student
WHERE student_id = 1;
```

DML is the category most beginners spend the most time with at first because it handles everyday data work.

### Think of DML as the Table Data Tools

DML is like working with the forms inside the filing cabinet.

```text
SELECT = read the forms
INSERT = add a new form
UPDATE = edit an existing form
DELETE = remove a form
```

Important safety note:

```text
WHERE is the targeting system for UPDATE and DELETE.
```

Without `WHERE`, you can accidentally update or delete every row.

---

## DCL: Data Control Language

**DCL** stands for **Data Control Language**.

DCL commands control **permissions**.

They decide who is allowed to do what.

Common DCL commands include:

```sql
GRANT
REVOKE
```

Example:

```sql
GRANT SELECT
ON student
TO reporting_user;
```

This gives `reporting_user` permission to read from the `student` table.

Another example:

```sql
REVOKE SELECT
ON student
FROM reporting_user;
```

This removes that permission.

### Think of DCL as the Security Desk

DCL is about access control.

```text
GRANT  = give permission
REVOKE = take permission away
```

Beginners may not use DCL much at first, but it matters in real systems because not every user should be allowed to read, change, or delete every table.

---

## TCL: Transaction Control Language

**TCL** stands for **Transaction Control Language**.

TCL commands control **transactions**.

A transaction is a group of database actions that should be treated as one unit.

Common TCL commands include:

```sql
COMMIT
ROLLBACK
SAVEPOINT
```

Example:

```sql
BEGIN;

UPDATE account
SET balance = balance - 100
WHERE account_id = 1;

UPDATE account
SET balance = balance + 100
WHERE account_id = 2;

COMMIT;
```

This saves both updates.

If something goes wrong, you may use:

```sql
ROLLBACK;
```

That undoes the changes in the transaction.

### Think of TCL as the Save/Undo System

TCL is like deciding whether changes should become permanent.

```text
COMMIT    = save the changes
ROLLBACK  = undo the changes
SAVEPOINT = mark a checkpoint you can roll back to
```

This is especially important when multiple changes must succeed or fail together.

For example, transferring money between accounts should not only subtract from one account. It must also add to the other account. If one part fails, the whole transaction should be undone.

---

## Simple Memory Version

```text
SQL is the umbrella.

DDL builds the structure.
DML works with the data.
DCL controls access.
TCL controls save/undo behavior.
```

Or even shorter:

```text
DDL = Build
DML = Use
DCL = Allow
TCL = Save or Undo
```

---

## Command Family Cheat Sheet

| SQL Family | Full Name | Main Job | Common Commands | Think of it as... |
|---|---|---|---|---|
| DDL | Data Definition Language | Changes structure | `CREATE`, `ALTER`, `DROP` | Blueprint / construction tools |
| DML | Data Manipulation Language | Works with table data | `SELECT`, `INSERT`, `UPDATE`, `DELETE` | Add, read, edit, remove records |
| DCL | Data Control Language | Controls permissions | `GRANT`, `REVOKE` | Security desk |
| TCL | Transaction Control Language | Controls transactions | `COMMIT`, `ROLLBACK`, `SAVEPOINT` | Save / undo system |

---

## How to Choose the Right SQL Family

Ask what kind of work you are trying to do.

```text
Am I changing the database structure?
→ DDL

Am I reading or changing rows inside tables?
→ DML

Am I controlling who can access something?
→ DCL

Am I saving or undoing a group of changes?
→ TCL
```

---

## Beginner Examples by Goal

### Goal: Create a table

Use **DDL**.

```sql
CREATE TABLE student (
    student_id INT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name  VARCHAR(50)
);
```

### Goal: Add a student

Use **DML**.

```sql
INSERT INTO student (
    student_id,
    first_name,
    last_name
)
VALUES (
    1,
    'John',
    'Doe'
);
```

### Goal: Read student data

Use **DML**.

```sql
SELECT student_id,
       first_name,
       last_name
FROM student;
```

### Goal: Give a user permission to read student data

Use **DCL**.

```sql
GRANT SELECT
ON student
TO reporting_user;
```

### Goal: Save a group of changes

Use **TCL**.

```sql
COMMIT;
```

### Goal: Undo a group of changes

Use **TCL**.

```sql
ROLLBACK;
```

---

## Why This Matters

When you understand these categories, SQL stops feeling like a random list of commands.

Instead, you can organize the language by purpose.

```text
DDL answers: How do I build or change the structure?
DML answers: How do I read or change the data?
DCL answers: Who is allowed to do what?
TCL answers: Should these changes be saved or undone?
```

That is the real mental model.

SQL is the umbrella.

DDL, DML, DCL, and TCL are the tool families underneath it.

---

## Quick Check

Answer these in your own words:

1. What does it mean to say SQL is the umbrella?
2. Which SQL family changes database structure?
3. Which SQL family works with rows inside tables?
4. Which SQL family controls permissions?
5. Which SQL family controls saving and undoing transactions?
6. What is the difference between `DELETE` and `DROP TABLE`?
7. Why is `WHERE` important with `UPDATE` and `DELETE`?
8. What does `COMMIT` do?
9. What does `ROLLBACK` do?
10. Which family would `CREATE TABLE` belong to?

---

## Summary

SQL is the full language.

Under SQL are command families:

```text
DDL = Data Definition Language
DML = Data Manipulation Language
DCL = Data Control Language
TCL = Transaction Control Language
```

The survival guide memory version is:

```text
SQL is the umbrella.
DDL builds.
DML uses.
DCL allows.
TCL saves or undoes.
```

Once learners see that structure, the command list becomes much less confusing.
