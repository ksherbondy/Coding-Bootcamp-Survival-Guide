# JavaScript BigInt

> `BigInt` represents integers larger than JavaScript's safe `Number` integer range.

## Learning Objectives

You should be able to:

- create `BigInt` values
- explain why `BigInt` exists
- understand its arithmetic restrictions
- know when not to use it

---

# 1. Why BigInt Exists

Normal JavaScript numbers cannot represent every integer exactly forever.

The safe range ends at:

```js
Number.MAX_SAFE_INTEGER;
```

For larger exact integers, use:

```js
BigInt
```

---

# 2. Creating BigInts

Add `n`:

```js
const huge = 9007199254740993n;
```

Or:

```js
const huge = BigInt("9007199254740993");
```

Type:

```js
typeof huge;
```

returns:

```text
"bigint"
```

---

# 3. Arithmetic

```js
10n + 5n;  // 15n
10n - 5n;  // 5n
10n * 5n;  // 50n
10n / 3n;  // 3n
10n % 3n;  // 1n
2n ** 8n;  // 256n
```

BigInt division truncates fractional results because BigInt represents integers.

---

# 4. Do Not Mix Number and BigInt Directly

This throws:

```js
10n + 5;
```

Convert deliberately:

```js
10n + BigInt(5);
```

or, if safe:

```js
Number(10n) + 5;
```

---

# 5. Comparisons

```js
10n === 10;
```

is:

```js
false
```

because the types differ.

Relational comparisons can compare them in many cases:

```js
10n > 5;
```

---

# 6. BigInt Is for Integers

You cannot write:

```js
1.5n
```

BigInt does not represent floating-point decimal values.

---

# 7. JSON Caveat

Standard `JSON.stringify()` does not serialize BigInt values automatically.

```js
JSON.stringify({ value: 10n });
```

throws unless you provide your own conversion strategy.

---

# 8. When to Use BigInt

Useful for:

```text
very large integer IDs
cryptographic / number-theory work
exact integer counters beyond safe Number range
large integer algorithms
```

Do not use BigInt merely because a value is "large" if ordinary `Number` is sufficient.

---

# Practice

1. Why does BigInt exist?
2. What does the `n` suffix mean?
3. Can you add `10n + 5` directly?
4. What happens to `10n / 3n`?
5. Can BigInt represent `1.5`?
