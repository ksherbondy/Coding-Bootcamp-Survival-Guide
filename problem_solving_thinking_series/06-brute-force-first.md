# 6. Brute Force First

## Objective

Learn to write the obvious correct solution before chasing the clever solution.

"Brute force" is not an insult.

It is often the easiest way to understand:

- the problem;
- correctness;
- repeated work;
- what should be optimized.

---

# Example: Pair Sum

## Problem

> Return `true` if any two numbers add to the target.

Brute force:

```js
function hasPair(numbers, target) {
  for (let i = 0; i < numbers.length; i++) {
    for (let j = i + 1; j < numbers.length; j++) {
      if (numbers[i] + numbers[j] === target) {
        return true;
      }
    }
  }

  return false;
}
```

This is clear.

Now ask:

> What work is being repeated?

For each number, we search many other numbers.

Can we remember values we have already seen?

```js
function hasPair(numbers, target) {
  const seen = new Set();

  for (const number of numbers) {
    const needed = target - number;

    if (seen.has(needed)) {
      return true;
    }

    seen.add(number);
  }

  return false;
}
```

The optimization was discovered by examining the brute-force solution.

---

# Transformation Pattern

```text
brute force
↓
identify repeated work
↓
ask what could be remembered
↓
choose a structure
↓
remove repeated work
```

---

# Instructor Message

Students should not feel embarrassed by a correct nested-loop solution.

A correct slow solution is often the bridge to a better solution.
