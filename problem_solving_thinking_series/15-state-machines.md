# 15. State Machines and Legal Transitions

## Objective

Think in terms of valid states and transitions instead of scattered booleans.

---

# Example: Order Status

Possible states:

```text
created
paid
shipped
delivered
cancelled
```

Legal transitions might be:

```text
created → paid
created → cancelled
paid → shipped
paid → cancelled
shipped → delivered
```

Not every transition should exist.

For example:

```text
delivered → paid
```

makes no sense.

---

# Weak Representation

```js
order.isPaid = true;
order.isCancelled = true;
order.isDelivered = true;
```

These booleans can create contradictory combinations.

---

# Clearer State

```js
order.status = "paid";
```

Then transitions can be checked explicitly.

```js
function ship(order) {
  if (order.status !== "paid") {
    throw new Error("Order must be paid before shipping");
  }

  order.status = "shipped";
}
```

---

# Mental Model

```text
current state
+
event
=
next state
```

Examples:

```text
loggedOut + login success → loggedIn
playing + pause → paused
connected + failure → disconnected
```

State-machine thinking is useful in UI, games, networking, workflows, and embedded systems.
