# 9. Loop and Traversal Selection

## Objective

Choose repetition based on intent, not habit.

---

# `for...of`

Use when you want values:

```js
for (const value of values) {
}
```

Good for:

```text
scan every item
accumulate
validate
search
```

---

# Indexed `for`

Use when position matters:

```js
for (let i = 0; i < values.length; i++) {
}
```

Good for:

```text
compare neighbors
access previous/next
skip positions
two indexes
```

---

# `while`

Use when repetition depends on a changing condition rather than a known collection length.

```js
while (condition) {
}
```

Good for:

```text
keep searching until...
repeat until state changes...
move pointers until they meet...
```

---

# `map`

Signal:

```text
transform every element
```

---

# `filter`

Signal:

```text
keep all elements that match
```

---

# `reduce`

Signal:

```text
combine many values into one result
```

---

# `some`

Signal:

```text
does any value match?
```

---

# `every`

Signal:

```text
do all values match?
```

---

# Example

Problem:

> Determine whether every score is at least 70.

The word:

```text
every
```

suggests:

```js
function allPassed(scores) {
  return scores.every(score => score >= 70);
}
```

The choice communicates intent.
