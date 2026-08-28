# 20. Why This Solution? Comparative Reasoning

## Objective

Build judgment by comparing multiple correct approaches.

Students should practice answering:

> Why would I choose this solution instead of the others?

---

# Example: Remove Duplicates

## Solution A

```js
function unique(values) {
  const result = [];

  for (const value of values) {
    if (!result.includes(value)) {
      result.push(value);
    }
  }

  return result;
}
```

## Solution B

```js
function unique(values) {
  return [...new Set(values)];
}
```

Questions:

```text
Which most directly expresses uniqueness?
Which performs repeated searching?
Which makes the desired behavior part of the data structure?
Which is easiest to explain?
```

---

# Example: Find Maximum

## Solution A

```js
return [...numbers].sort((a, b) => b - a)[0];
```

## Solution B

```js
let max = -Infinity;

for (const number of numbers) {
  if (number > max) max = number;
}

return max;
```

Questions:

```text
Do we need the entire collection sorted?
What information is actually required?
Which approach does extra work?
```

---

# Example: Powers of Two

Possible approaches:

```js
while (2 ** n - 1 < grains) {
  n++;
}
```

```js
while ((1 << n) - 1 < grains) {
  n++;
}
```

```js
Math.ceil(Math.log2(grains + 1));
```

```js
grains ? grains.toString(2).length : 0;
```

Discussion questions:

```text
Which mirrors the formula most clearly?
Which mirrors binary representation?
Which depends on 32-bit bitwise behavior?
Which hides work inside a built-in?
Which is easiest for the team to maintain?
Which performs best for the actual workload?
```

---

# Final Principle

Do not teach students to ask only:

> "Does it pass?"

Also ask:

```text
Why does it work?
What assumptions does it make?
What behavior does the structure guarantee?
What repeated work does it remove?
What tradeoff does it introduce?
Would I still choose it if the input changed?
```

That is the beginning of engineering judgment.
