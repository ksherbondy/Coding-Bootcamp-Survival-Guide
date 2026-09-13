# JavaScript Map

> `Map` is a built-in key/value data structure designed specifically for lookup and dynamic associations.

## Learning Objectives

You should be able to:

- create Maps
- add, read, update, and delete entries
- iterate Maps
- explain Map vs Object
- use non-string keys
- understand insertion order

---

# 1. Creating a Map

```js
const users = new Map();
```

Or initialize with key/value pairs:

```js
const users = new Map([
  ["ada", 1],
  ["grace", 2]
]);
```

Each entry is:

```text
[key, value]
```

---

# 2. Add or Update

```js
users.set("ada", 10);
```

If the key already exists, the value is updated.

---

# 3. Read

```js
users.get("ada");
```

If missing:

```js
users.get("missing");
```

returns:

```js
undefined
```

---

# 4. Check for a Key

```js
users.has("ada");
```

---

# 5. Delete

```js
users.delete("ada");
```

Remove everything:

```js
users.clear();
```

---

# 6. Size

```js
users.size;
```

Unlike arrays and strings, Maps use:

```text
size
```

not:

```text
length
```

---

# 7. Keys Can Be Any Value

Object keys are primarily strings or symbols.

Map keys can be:

```text
strings
numbers
objects
functions
symbols
other values
```

Example:

```js
const metadata = new Map();

const user = {
  name: "Ada"
};

metadata.set(user, {
  loggedIn: true
});
```

The object itself is the key.

---

# 8. Iteration

```js
for (const [key, value] of users) {
  console.log(key, value);
}
```

This works because Map iteration yields:

```text
[key, value]
```

pairs.

---

# 9. Map Methods

```text
set()
get()
has()
delete()
clear()
entries()
keys()
values()
forEach()
```

Property:

```text
size
```

Maps are iterable.

---

# 10. Insertion Order

Maps remember insertion order during iteration.

```js
const map = new Map();

map.set("first", 1);
map.set("second", 2);
map.set("third", 3);
```

Iteration follows that order.

---

# 11. Map vs Object

Use an object when modeling something with named properties:

```js
const user = {
  name: "Ada",
  age: 36
};
```

Use Map when the object itself is really a dynamic key/value data structure:

```js
const userScores = new Map();
```

Map provides explicit operations:

```text
set
get
has
delete
size
```

---

# 12. Frequency Counter With Map

```js
const colors = ["red", "blue", "red"];

const counts = new Map();

for (const color of colors) {
  counts.set(
    color,
    (counts.get(color) || 0) + 1
  );
}
```

---

# 13. Convert Map to Array

```js
[...map];
```

produces an array of:

```text
[key, value]
```

pairs.

---

# 14. Convert Map to Object

When keys are suitable property keys:

```js
Object.fromEntries(map);
```

Convert object to Map:

```js
new Map(Object.entries(object));
```

---

# 15. Key Identity

Objects used as keys compare by identity.

```js
const a = {};
const b = {};

map.set(a, "value");

map.get(a); // "value"
map.get(b); // undefined
```

Even though `a` and `b` look identical, they are different objects.

---

# Practice

1. What method adds an entry?
2. What method reads one?
3. What property tells you the number of entries?
4. What can Map use as keys that plain object property syntax cannot naturally use?
5. When is a plain object a better semantic fit?
