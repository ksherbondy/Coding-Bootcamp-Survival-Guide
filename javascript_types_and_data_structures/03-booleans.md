# JavaScript Booleans

> A boolean represents one of two logical states: `true` or `false`.

## Learning Objectives

You should be able to:

- create boolean values
- use comparison and logical operators
- explain truthy and falsy values
- avoid common boolean comparison mistakes
- use short-circuit evaluation

---

# 1. Boolean Values

There are only two boolean primitive values:

```js
true;
false;
```

Example:

```js
const loggedIn = true;
const admin = false;
```

```js
typeof true;
```

returns:

```text
"boolean"
```

---

# 2. Comparisons Produce Booleans

```js
5 > 3;     // true
5 < 3;     // false
5 === 5;   // true
5 !== 4;   // true
```

Common comparison operators:

```text
=== strict equality
!== strict inequality
>   greater than
<   less than
>=  greater than or equal
<=  less than or equal
```

---

# 3. Prefer Strict Equality

```js
5 == "5";
```

returns:

```js
true
```

because loose equality performs coercion.

```js
5 === "5";
```

returns:

```js
false
```

because the types differ.

Prefer:

```js
===
!==
```

unless you deliberately need coercive equality.

---

# 4. Boolean Logic

AND:

```js
true && true;
```

OR:

```js
true || false;
```

NOT:

```js
!true;
```

Think:

```text
&& → both
|| → either
!  → opposite
```

---

# 5. Booleans in `if`

```js
const loggedIn = true;

if (loggedIn) {
  console.log("Welcome");
}
```

You do not need:

```js
if (loggedIn === true)
```

when the variable is already a boolean.

---

# 6. Truthy and Falsy

JavaScript allows non-boolean values in conditions.

Falsy values include:

```text
false
0
-0
0n
""
null
undefined
NaN
```

Most other values are truthy.

Examples:

```js
if ("hello") {
  // runs
}

if ([]) {
  // runs
}

if ({}) {
  // runs
}
```

Empty arrays and empty objects are truthy.

---

# 7. Convert to Boolean

```js
Boolean(value);
```

Examples:

```js
Boolean(0);        // false
Boolean(1);        // true
Boolean("");       // false
Boolean("hello");  // true
Boolean([]);       // true
```

Double NOT also converts:

```js
!!value;
```

but `Boolean(value)` is often clearer for beginners.

---

# 8. Short-Circuit Evaluation

With `&&`, JavaScript stops when it finds a falsy operand.

```js
loggedIn && showDashboard();
```

With `||`, JavaScript stops when it finds a truthy operand.

```js
const name = inputName || "Guest";
```

Be careful: these operators return operands, not necessarily literal booleans.

```js
"hello" && 42;
```

returns:

```text
42
```

---

# 9. `??` Is Different From `||`

```js
value || fallback;
```

uses the fallback for any falsy value.

```js
value ?? fallback;
```

uses the fallback only for:

```text
null
undefined
```

Example:

```js
0 || 10;  // 10
0 ?? 10;  // 0
```

---

# 10. Common Mistakes

Do not write:

```js
if (x === 1 || 2)
```

because `2` is independently truthy.

Correct:

```js
if (x === 1 || x === 2)
```

Also remember:

```js
[] === false
```

is not a good way to reason about truthiness.

Use the value directly in the condition when appropriate.

---

# Quick Reference

```js
Boolean(value);

=== !==
> < >= <=

&&
||
!

??
```

---

# Practice

1. What are the two boolean values?
2. What is the difference between `==` and `===`?
3. Is `[]` truthy or falsy?
4. Is `""` truthy or falsy?
5. What is the difference between `||` and `??`?
