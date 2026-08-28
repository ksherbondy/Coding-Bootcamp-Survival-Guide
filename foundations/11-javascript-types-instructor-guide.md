# Instructor Guide: JavaScript Types

## 1. Introduction

This guide supports instructors leading the **JavaScript Types** forum in a flipped classroom model. Students should arrive with pre-work familiarity on primitive values and objects. The live session should focus on runtime reasoning, coercion behavior, and debugging edge cases.

### Instructor Goals

- Establish an accurate model of JavaScript types and `typeof` behavior.
- Clarify assignment and passing semantics for primitives and objects.
- Coach students through equality and numeric edge-case debugging.

---

## 2. Learning Objectives

By the end of this session, students should be able to:

- Identify JavaScript's seven primitive types and distinguish them from objects.
- Explain `typeof` behavior, including `typeof null`.
- Compare assignment and parameter passing behavior for primitives versus objects.
- Apply mutability and immutability rules in practical code.
- Evaluate expressions with `==`, `===`, and `Object.is`.
- Debug `NaN`, coercion, and `Number`/`BigInt` edge cases.

---

## 3. Key Concepts

### 3.1 Primitive Types Versus Objects

JavaScript primitive types are:

- `string`
- `number`
- `bigint`
- `boolean`
- `undefined`
- `symbol`
- `null`

Everything else is an object value.

```js
const samples = [
  // Value             Expected typeof
  // ─────             ───────────────
  "hello",             // "string"
  42,                  // "number"
  42n,                 // "bigint"
  true,                // "boolean"
  undefined,           // "undefined"
  Symbol("id"),        // "symbol"
  null,                // "object"   (historic spec bug)
  {},                  // "object"
  [],                  // "object"   (use Array.isArray to distinguish)
  function () {},      // "function"
];

for (const value of samples) {
  console.log(value, "=>", typeof value);
}
```

### 3.2 `typeof` Caveats

`typeof` is useful but incomplete:

- `typeof null` is `"object"` due to historic behavior.
- Arrays also return `"object"`.
- Use `Array.isArray(value)` to distinguish arrays.

### 3.3 Assignment and Call-by-Sharing

JavaScript passes values. For object values, the value is a shared reference.

```js
function reassignPerson(person) {
  person = { name: "Reassigned" }; // local reassignment only
}

function mutatePerson(person) {
  person.name = "Mutated"; // mutates shared object
}

const original = { name: "Ada" };

reassignPerson(original);
console.log(original.name); // Ada

mutatePerson(original);
console.log(original.name); // Mutated
```

### 3.4 Mutability and `const`

- Primitives are immutable.
- Objects and arrays are mutable.
- `const` blocks rebinding, not internal mutation.

```js
const user = { name: "Sam" };
user.name = "Samira"; // allowed

const items = [1, 2, 3];
items.push(4); // allowed
```

### 3.5 Equality and Coercion

Default rule: use `===` unless coercion is explicitly intended.

```js
const checks = [
  ["'5' and 5", "5", 5],
  ["0 and false", 0, false],
  ["null and undefined", null, undefined],
  ["NaN and NaN", NaN, NaN],
  ["+0 and -0", +0, -0]
];

for (const [label, a, b] of checks) {
  console.log(label, {
    doubleEquals: a == b,
    tripleEquals: a === b,
    objectIs: Object.is(a, b)
  });
}
```

### 3.6 `NaN` and Numeric Edge Cases

```js
console.log(0 / 0); // NaN
console.log(NaN === NaN); // false
console.log(Number.isNaN(0 / 0)); // true

const totalCents = 1500n;
const feeCents = 25n;

console.log(totalCents + feeCents); // 1525n
```

---

## 4. Facilitation Guide — 60-Minute Session

### 4.1 Opening and Pre-Work Check — 5–10 Minutes

Ask:

> How many primitive types does JavaScript have?

Prompt checks:

- `typeof null`
- Object passing semantics
- Difference between `==` and `===`

Set the session flow:

**Type model → output prediction → edge-case debugging**

### 4.2 Concept Deep Dive — 15–20 Minutes

- Run the primitive matrix and collect output predictions before execution.
- Explain array, object, and function detection strategy.
- Walk through mutation versus reassignment behavior.
- Compare equality operators with deliberate test cases.

### 4.3 Live Coding — 20–25 Minutes

Build:

- A small value classifier utility.
- An object mutation/reassignment test harness.
- An equality comparison logger using `==`, `===`, and `Object.is`.
- `NaN` and `BigInt` checks, including discussion of runtime errors.

### 4.4 Debrief and Exit Check — 5–10 Minutes

Revisit misconceptions collected at the start of the session.

#### Exit Check

Ask:

- Why is `typeof` not enough for arrays?
- What does call-by-sharing mean in practice?
- When is `Object.is` useful over `===`?

---

## 5. Live Coding and Exercise Guide

### Exercise 1 — Foundational: Type Classifier

**Setup Prompt:**

> Classify mixed values as primitive or object and log type checks.

**Expected Reasoning:**

Separate category and detection method.

**Common Wrong Answers:**

Expecting `typeof []` to return `"array"`.

**Instructor Hint:**

Ask which additional check should follow `typeof`.

---

### Exercise 2 — Intermediate: Mutation Versus Reassignment

**Setup Prompt:**

> Predict function side effects on primitive and object arguments.

**Expected Reasoning:**

Reassignment is local; object mutation is shared.

**Common Wrong Answers:**

Using the blanket statement that objects are always "passed by reference."

**Instructor Hint:**

Ask what exact value is passed into the parameter.

---

### Exercise 3 — Advanced: Equality and Number Edge Cases

**Setup Prompt:**

> Predict outcomes for tricky comparisons and `NaN` checks.

**Expected Reasoning:**

Account for coercion and special same-value semantics.

**Common Wrong Answers:**

Assuming `NaN === NaN` is `true`.

**Instructor Hint:**

Require an explanation of the conversion step for each expression.

---

## 6. Common Pitfalls and Debug Scripts

### Pitfall A: `null` and `undefined` Confusion

```js
let a;
const b = null;

console.log(a == b, a === b); // true, false
```

### Pitfall B: `NaN` Comparison Bug

```js
const value = Number("hello");

console.log(value === NaN); // always false
```

### Pitfall C: Accidental Coercion

```js
console.log("5" + 1); // "51"
console.log("5" - 1); // 4
```

### Pitfall D: Shared Nested References

```js
const original = { scores: [1, 2] };
const shallowCopy = { ...original };

shallowCopy.scores.push(3);

console.log(original.scores); // [1, 2, 3]
```

---

## 7. Modern Updates for This Forum

- Use seven primitive types in all examples and prompts.
- Prefer precise **call-by-sharing** wording over "passed by reference" shorthand.
- Teach `===` as the default equality operator.
- Use `Number.isNaN()` for `NaN` checks.
- Include `Number`/`BigInt` mixing warnings in arithmetic examples.

---

## 8. Resources

- MDN: JavaScript Data Types and Data Structures
- MDN: `typeof` Operator
- MDN: Equality Comparisons and Sameness
- MDN: `Number.isNaN()`
- MDN: `BigInt`
