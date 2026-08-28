# 14. Invariants: What Must Always Be True?

## Objective

Learn to reason about correctness by identifying facts that remain true throughout an algorithm.

An **invariant** is something that should remain true while the algorithm runs.

---

# Example: Running Maximum

```js
let max = -Infinity;

for (const number of numbers) {
  if (number > max) {
    max = number;
  }
}
```

Invariant:

> After each iteration, `max` is the largest value seen so far.

That statement explains why the algorithm works.

---

# Example: Duplicate Detection

```js
const seen = new Set();

for (const value of values) {
  if (seen.has(value)) return true;
  seen.add(value);
}
```

Invariant:

> `seen` contains every previously processed value.

Therefore:

```js
seen.has(value)
```

correctly answers:

> Have I encountered this value before?

---

# Example: Binary Search

Invariant:

> If the target exists, it must still be inside the current `[left, right]` search region.

Every comparison removes a region that cannot contain the target.

---

# Why This Matters

Instead of asking:

> "I think this code works?"

ask:

> "What fact is true after every step, and does that fact prove the result?"
