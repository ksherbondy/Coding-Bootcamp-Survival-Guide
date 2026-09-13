# JavaScript `null` and `undefined`

> Both represent absence, but they usually communicate different kinds of absence.

## Learning Objectives

You should be able to:

- explain `undefined`
- explain `null`
- compare them safely
- understand `typeof null`
- use optional chaining and nullish coalescing

---

# 1. `undefined`

A variable that has been declared but not assigned has the value:

```js
undefined
```

Example:

```js
let score;

console.log(score);
```

Output:

```text
undefined
```

Missing object properties also return `undefined`:

```js
const user = {};

user.name;
```

returns:

```js
undefined
```

---

# 2. `null`

`null` is usually used intentionally to mean:

```text
no value
empty
nothing here
not currently assigned
```

Example:

```js
const selectedUser = null;
```

That communicates deliberate absence.

---

# 3. Typical Mental Model

```text
undefined
→ JavaScript often means "not provided / not assigned / not found"

null
→ programmer often means "intentionally empty"
```

This is a convention, not a law enforced by the language.

---

# 4. `typeof`

```js
typeof undefined;
```

returns:

```text
"undefined"
```

But:

```js
typeof null;
```

returns:

```text
"object"
```

This is a long-standing historical quirk in JavaScript.

`null` is still a primitive value.

---

# 5. Equality

```js
null === undefined;
```

is:

```js
false
```

because they are different primitive values.

Loose equality:

```js
null == undefined;
```

returns:

```js
true
```

This is one of the unusual cases where some programmers deliberately use `==`, but beginners are usually better served by explicit checks.

---

# 6. Checking Explicitly

```js
value === null;
value === undefined;
```

Or:

```js
typeof value === "undefined";
```

---

# 7. Nullish Coalescing

```js
const name = userName ?? "Guest";
```

The fallback is used only if `userName` is:

```text
null
undefined
```

Unlike `||`, values such as:

```text
0
false
""
```

are preserved.

---

# 8. Optional Chaining

Instead of:

```js
user.address.city
```

which can throw if `address` is missing:

```js
user.address?.city;
```

returns:

```js
undefined
```

if the chain cannot continue.

---

# 9. Default Parameters

```js
function greet(name = "Guest") {
  return `Hello ${name}`;
}
```

The default applies when the argument is omitted or `undefined`.

```js
greet(undefined);
```

uses `"Guest"`.

But:

```js
greet(null);
```

passes `null` explicitly.

---

# Common Mistakes

Assuming:

```js
typeof null === "null"
```

It does not.

Using `||` when `0`, `false`, or `""` are legitimate values.

Confusing a missing property with a property intentionally set to `null`.

---

# Practice

1. What does an unassigned variable contain?
2. Why might a programmer deliberately use `null`?
3. What does `typeof null` return?
4. What is the difference between `||` and `??`?
5. What does optional chaining protect against?
