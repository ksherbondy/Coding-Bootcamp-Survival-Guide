# 16. Make Invalid States Hard to Represent

## Objective

Design data so incorrect combinations are impossible or obvious.

This is a deeper design principle:

> Do not only check for bad behavior. Structure the program so bad behavior has fewer places to exist.

---

# Example: Multiple Booleans

```js
const order = {
  isPaid: true,
  isShipped: false,
  isCancelled: true
};
```

Can an order be both paid and cancelled?

Maybe.

Maybe not.

The representation does not tell us.

---

# Better Representation

```js
const order = {
  status: "cancelled"
};
```

One state at a time.

---

# Example: Validated Values

Instead of passing arbitrary values everywhere:

```js
function sendEmail(address) {
}
```

validate at the boundary:

```js
function createEmailAddress(value) {
  if (!value.includes("@")) {
    throw new Error("Invalid email");
  }

  return value;
}
```

Now downstream code can rely on stronger assumptions.

---

# Data Structures Can Guarantee Behavior

```text
Set → uniqueness
Stack → LIFO
Queue → FIFO
Map → key/value relationship
```

The structure itself can enforce part of the program's contract.
