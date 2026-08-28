# 13. Trace Before You Code

## Objective

Make state changes visible before debugging them in your head.

---

# Trace Table

For code such as:

```js
let total = 0;

for (const number of [2, 4, 6]) {
  total += number;
}
```

Trace:

| Iteration | number | total before | total after |
|---:|---:|---:|---:|
| 1 | 2 | 0 | 2 |
| 2 | 4 | 2 | 6 |
| 3 | 6 | 6 | 12 |

---

# Why Trace?

Tracing helps answer:

```text
What changes?
What stays the same?
When does the condition become true?
What information is carried forward?
```

---

# Example: Two Pointers

```js
const numbers = [1, 2, 4, 6, 8, 10];
const target = 14;

let left = 0;
let right = numbers.length - 1;
```

Trace:

| left | right | values | sum | decision |
|---:|---:|---|---:|---|
| 0 | 5 | 1 + 10 | 11 | move left |
| 1 | 5 | 2 + 10 | 12 | move left |
| 2 | 5 | 4 + 10 | 14 | found |

The algorithm becomes easier to understand once movement is visible.
