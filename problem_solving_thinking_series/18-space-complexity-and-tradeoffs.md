# 18. Space Complexity and Tradeoffs

## Objective

Understand the common tradeoff between remembering information and recomputing it.

---

# Example: Duplicate Search

Nested-loop approach:

```js
function hasDuplicate(values) {
  for (let i = 0; i < values.length; i++) {
    for (let j = i + 1; j < values.length; j++) {
      if (values[i] === values[j]) return true;
    }
  }

  return false;
}
```

Uses little extra memory.

But repeats comparisons.

Set approach:

```js
function hasDuplicate(values) {
  const seen = new Set();

  for (const value of values) {
    if (seen.has(value)) return true;
    seen.add(value);
  }

  return false;
}
```

Uses extra memory to remember seen values.

That memory reduces repeated searching.

---

# Core Tradeoff

```text
recompute
vs
remember
```

Common examples:

```text
Map/Set indexes
memoization
caches
prefix sums
lookup tables
```

Optimization often means intentionally spending memory to save time.
