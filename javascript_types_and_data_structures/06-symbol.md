# JavaScript Symbol

> A `Symbol` is a unique primitive value often used as a property key.

## Learning Objectives

You should be able to:

- create symbols
- explain uniqueness
- use symbols as object keys
- understand why symbols exist
- recognize well-known symbols

---

# 1. Creating a Symbol

```js
const id = Symbol();
```

You can provide a description:

```js
const id = Symbol("id");
```

The description is for debugging/readability.

---

# 2. Symbols Are Unique

```js
Symbol("id") === Symbol("id");
```

returns:

```js
false
```

Even though the descriptions match, the symbols are different values.

---

# 3. Type

```js
typeof Symbol("id");
```

returns:

```text
"symbol"
```

---

# 4. Symbols as Object Keys

```js
const id = Symbol("id");

const user = {
  name: "Ada",
  [id]: 12345
};
```

Access:

```js
user[id];
```

The symbol key does not collide with a normal string property named `"id"`.

---

# 5. Why Use Symbols?

Symbols are useful when you want:

```text
unique property keys
metadata unlikely to collide with normal keys
protocol hooks used by JavaScript itself
```

---

# 6. Symbol-Keyed Properties and Enumeration

Symbol properties are not returned by:

```js
Object.keys(object);
```

Retrieve them with:

```js
Object.getOwnPropertySymbols(object);
```

---

# 7. Global Symbol Registry

```js
const a = Symbol.for("shared");
const b = Symbol.for("shared");

a === b;
```

returns:

```js
true
```

`Symbol.for()` uses a global registry.

Retrieve the key:

```js
Symbol.keyFor(a);
```

---

# 8. Well-Known Symbols

JavaScript defines special symbols used by language protocols.

Examples:

```text
Symbol.iterator
Symbol.asyncIterator
Symbol.toPrimitive
Symbol.toStringTag
Symbol.hasInstance
Symbol.match
Symbol.replace
Symbol.search
Symbol.split
Symbol.species
```

Example idea:

```js
object[Symbol.iterator]
```

can define how an object participates in iteration.

---

# 9. Symbols Are Advanced but Important

Most beginner programs do not need custom symbols.

You should still know they exist because they help explain:

```text
iteration
built-in protocols
special object behavior
unique keys
```

---

# Practice

1. Are two calls to `Symbol("id")` equal?
2. Can a symbol be used as an object key?
3. Does `Object.keys()` return symbol keys?
4. What does `Symbol.for()` do differently?
