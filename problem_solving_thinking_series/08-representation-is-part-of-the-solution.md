# 8. Representation Is Part of the Solution

## Objective

Recognize that changing the shape of the data can make a hard problem easy.

---

# Example: Duplicate Detection

Original representation:

```js
[4, 2, 7, 4]
```

Question:

> Has a value appeared before?

As an array, membership requires searching.

As a Set:

```js
new Set([4, 2, 7])
```

membership becomes the structure's job.

---

# Example: User Lookup

Array:

```js
[
  { id: 10, name: "A" },
  { id: 25, name: "B" }
]
```

Repeated question:

> Give me user 25.

Transform into:

```text
10 → user A
25 → user B
```

That representation naturally suggests:

```js
Map
```

---

# Example: Number → Binary

Problem:

> Which power-of-two range is this integer in?

Sometimes the number's binary representation already contains the information.

```js
(8).toString(2); // "1000"
```

The representation exposes bit length.

---

# Core Question

When a solution feels awkward, ask:

> Is my data in the best shape for the question I am asking?

Possible transformations:

```text
Array → Set
Array → Map
string → characters
records → index by ID
graph edges → adjacency list
number → binary
nested data → flattened data
```

The algorithm may become simpler once the representation matches the problem.
