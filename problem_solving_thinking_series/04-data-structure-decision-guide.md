# 4. Data Structure Decision Guide

## Objective

Use the problem's required behavior to narrow data-structure choices.

---

# Decision Questions

## Do I need uniqueness?

Consider:

```text
Set
```

## Do I need key → value lookup?

Consider:

```text
Map
Object
```

## Does position/order matter?

Consider:

```text
Array
```

## Do I need most-recent-first behavior?

Consider:

```text
Stack
```

## Do I need first-arrived-first behavior?

Consider:

```text
Queue
```

## Is the data hierarchical?

Consider:

```text
Tree
```

## Is the problem about connections?

Consider:

```text
Graph
```

## Do I repeatedly need the smallest/largest priority?

Consider:

```text
Heap / Priority Queue
```

---

# Worked Example

## Problem

> Given a list of users, repeatedly retrieve a user by ID.

Initial idea:

```js
users.find(user => user.id === id);
```

That works.

But the word:

```text
repeatedly
```

should trigger another question:

> Am I repeatedly scanning the same collection?

Build an index:

```js
function indexUsers(users) {
  const byId = new Map();

  for (const user of users) {
    byId.set(user.id, user);
  }

  return byId;
}
```

Then:

```js
const user = byId.get(id);
```

The data structure changed the problem from repeated searching into lookup.

---

# Teaching Point

Do not ask:

> Which structure is "best"?

Ask:

> Which behavior do I need the structure to provide?
