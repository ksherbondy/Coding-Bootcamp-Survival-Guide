# 12. Edge-Case Discovery

## Objective

Derive tests from the problem before running the official tests.

---

# Universal Edge-Case Checklist

Consider:

```text
empty input
one item
two items
zero
negative values
duplicates
all same
already sorted
reverse sorted
target absent
target first
target last
ties
very large values
```

Not every problem needs all of these.

The point is to ask.

---

# Example

Problem:

> Return the largest number in an array.

Possible tests:

```js
findMax([5]);            // one item
findMax([-5, -2, -9]);   // all negative
findMax([2, 2, 2]);      // duplicates
findMax([1, 100, 3]);    // max in middle
```

A common bad initialization:

```js
let max = 0;
```

fails for:

```js
[-5, -2, -9]
```

Better:

```js
let max = -Infinity;
```

or use the first array element if non-empty input is guaranteed.

---

# Derive Tests From Wording

If problem says:

```text
at least
```

test:

```text
below threshold
exact threshold
above threshold
```

If problem says:

```text
unique
```

test:

```text
no duplicates
one duplicate
all duplicates
```

Tests should come from the contract, not random guesses.
