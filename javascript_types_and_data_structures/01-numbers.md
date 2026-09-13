# JavaScript Numbers

> JavaScript's `number` type represents both integers and floating-point values.

## Learning Objectives

By the end of this lesson, you should be able to:

- create numbers
- perform arithmetic
- explain the difference between integers and floating-point values
- explain why JavaScript has floating-point precision issues
- use `Number` and `Math` methods
- understand `NaN` and `Infinity`
- identify safe integer limits
- explain why bitwise operations behave differently from normal number operations

---

# 1. Creating Numbers

```js
const age = 40;
const price = 19.99;
const negative = -12;
const scientific = 1.5e6;
```

JavaScript does not have separate everyday types such as:

```text
int
float
double
```

for normal numeric values.

They are all:

```js
typeof 42;      // "number"
typeof 19.99;   // "number"
```

---

# 2. Basic Arithmetic

```js
5 + 2;   // 7
5 - 2;   // 3
5 * 2;   // 10
5 / 2;   // 2.5
5 % 2;   // 1
5 ** 2;  // 25
```

`%` is the remainder operator.

```js
10 % 2 === 0;
```

is a common way to test whether a number is even.

---

# 3. Operator Precedence

```js
2 + 3 * 4;
```

returns:

```text
14
```

because multiplication happens before addition.

Use parentheses when you want the grouping to be explicit:

```js
(2 + 3) * 4;
```

returns:

```text
20
```

---

# 4. Floating-Point Representation

JavaScript `number` values use IEEE-754 double-precision floating-point representation.

That means many decimal fractions cannot be represented exactly in binary.

```js
0.1 + 0.2;
```

produces approximately:

```text
0.30000000000000004
```

This is not unique to JavaScript. It is a consequence of binary floating-point representation.

---

# 5. Comparing Floating-Point Results

Avoid assuming decimal calculations are always exact.

Instead of:

```js
0.1 + 0.2 === 0.3;
```

you can compare with a tolerance:

```js
Math.abs((0.1 + 0.2) - 0.3) < Number.EPSILON;
```

---

# 6. Safe Integers

JavaScript can exactly represent integers only through a certain range.

```js
Number.MAX_SAFE_INTEGER;
Number.MIN_SAFE_INTEGER;
```

The maximum safe integer is:

```text
9007199254740991
```

You can test:

```js
Number.isSafeInteger(value);
```

For integers beyond that range, consider `BigInt`.

---

# 7. `NaN`

`NaN` means:

```text
Not-a-Number
```

but ironically:

```js
typeof NaN;
```

returns:

```text
"number"
```

Example:

```js
Number("hello");
```

returns:

```js
NaN
```

Prefer:

```js
Number.isNaN(value);
```

to test for it.

---

# 8. Infinity

JavaScript also has:

```js
Infinity;
-Infinity;
```

Example:

```js
1 / 0;   // Infinity
```

---

# 9. Useful `Number` Methods

```js
Number.isInteger(value);
Number.isSafeInteger(value);
Number.isNaN(value);
Number.isFinite(value);
Number.parseInt(value);
Number.parseFloat(value);
```

Examples:

```js
Number.isInteger(10);      // true
Number.isInteger(10.5);    // false

Number.parseInt("42px");   // 42
Number.parseFloat("3.14"); // 3.14
```

---

# 10. Converting Values to Numbers

```js
Number("42");      // 42
Number("3.14");    // 3.14
Number("");        // 0
Number("hello");   // NaN
```

Unary plus also converts:

```js
+"42"; // 42
```

but `Number()` is often clearer for beginners.

---

# 11. `Math`

`Math` is a built-in object containing numeric functions and constants.

Common examples:

```js
Math.round(4.6);  // 5
Math.floor(4.9);  // 4
Math.ceil(4.1);   // 5
Math.trunc(4.9);  // 4

Math.abs(-10);    // 10
Math.sqrt(81);    // 9
Math.pow(2, 8);   // 256

Math.min(4, 2, 9);
Math.max(4, 2, 9);

Math.random();
```

---

# 12. Powers and Roots

```js
2 ** 8;     // 256
81 ** 0.5;  // 9
```

So:

```js
Math.sqrt(81);
```

and:

```js
81 ** 0.5;
```

both calculate the square root.

---

# 13. Rounding

```js
Math.round(4.5); // 5
Math.floor(4.9); // 4
Math.ceil(4.1);  // 5
Math.trunc(4.9); // 4
```

Think:

```text
round → nearest integer
floor → down
ceil  → up
trunc → remove fractional part
```

---

# 14. Numeric Strings Are Still Strings

```js
const x = "10";
```

This is not a number.

```js
typeof x; // "string"
```

Therefore:

```js
"10" + 5;
```

returns:

```text
"105"
```

because `+` can perform string concatenation.

---

# 15. Bitwise Operations

JavaScript bitwise operators include:

```text
&
|
^
~
<<
>>
>>>
```

A major caveat:

> Normal JavaScript numbers are floating-point values, but most bitwise operators convert values to 32-bit integers before operating.

Example:

```js
5 << 1;
```

shifts the bits left once:

```text
5  → 0101
10 → 1010
```

Bitwise operations are useful for:

```text
flags
masks
powers of two
low-level representations
compact state
```

but should not be treated as general replacements for normal arithmetic.

---

# 16. Common Mistakes

```js
0.1 + 0.2 === 0.3;
```

Do not assume floating-point decimals are exact.

```js
NaN === NaN;
```

returns `false`.

Use:

```js
Number.isNaN(value);
```

Do not assume huge integers stay exact after `Number.MAX_SAFE_INTEGER`.

---

# 17. Quick Reference

```js
Number(value);

Number.isInteger(value);
Number.isSafeInteger(value);
Number.isNaN(value);
Number.isFinite(value);

Number.parseInt(value);
Number.parseFloat(value);

Math.round(value);
Math.floor(value);
Math.ceil(value);
Math.trunc(value);
Math.abs(value);
Math.sqrt(value);
Math.min(...values);
Math.max(...values);
Math.random();
```

---

# Practice

1. What does `typeof 3.14` return?
2. Why can `0.1 + 0.2` be surprising?
3. What is the difference between `Math.floor()` and `Math.trunc()`?
4. What does `Number.isInteger()` test?
5. Why might `BigInt` be necessary?
6. Why should bitwise operators be treated differently from ordinary arithmetic?
