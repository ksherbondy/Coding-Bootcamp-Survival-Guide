# 11. Boundary Thinking and Off-by-One Errors

## Objective

Treat boundary words as algorithmic information.

---

# Boundary Vocabulary

| Wording | Think |
|---|---|
| at least | `>=` |
| at most | `<=` |
| more than | `>` |
| less than | `<` |
| exactly | `===` |
| first | lower boundary |
| last | upper boundary |

---

# Example

Problem:

> Return the first index where the running total is at least 10.

"At least" means:

```js
total >= 10
```

not:

```js
total > 10
```

---

# Array Boundary

For an array:

```js
const values = ["a", "b", "c"];
```

Valid indexes:

```text
0
1
2
```

Length:

```text
3
```

Therefore:

```js
values.length - 1
```

is the last valid index.

---

# Common Loop Difference

```js
i < array.length
```

versus:

```js
i <= array.length
```

The second attempts to access one position beyond the array.

---

# Boundary Test Habit

Always test:

```text
just below
exactly equal
just above
```

If threshold is `10`, test:

```text
9
10
11
```
