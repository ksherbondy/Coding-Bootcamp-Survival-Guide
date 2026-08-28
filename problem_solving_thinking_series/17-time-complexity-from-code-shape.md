# 17. Time Complexity From Code Shape

## Objective

Develop rough complexity instincts by looking at how work grows.

---

# One Full Pass

```js
for (const value of values) {
}
```

Often:

```text
O(n)
```

---

# Nested Full Passes

```js
for (const a of values) {
  for (const b of values) {
  }
}
```

Often:

```text
O(n²)
```

---

# Repeatedly Halving

```text
n
n/2
n/4
n/8
...
```

Often:

```text
O(log n)
```

---

# Sorting

General comparison sorting is commonly:

```text
O(n log n)
```

---

# Important Warning

Visible source is not the whole runtime.

This:

```js
values.sort()
```

is one visible function call, but sorting still performs internal work.

Similarly:

```js
Math.log2(x)
```

is one visible JavaScript call, but the runtime still performs lower-level work.

Do not confuse:

```text
few lines of code
```

with:

```text
little computational work
```

---

# Better Question

Instead of counting lines, ask:

> How does the amount of work change as the input grows?
