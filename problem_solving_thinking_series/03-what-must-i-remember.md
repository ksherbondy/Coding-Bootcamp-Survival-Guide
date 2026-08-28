# 3. What Must I Remember?

## Objective

Choose data structures by asking what information must survive while the algorithm runs.

This is one of the strongest problem-solving questions:

> **What must I remember from what I have already processed?**

---

# Memory Need → Candidate Tool

## "Have I seen this?"

Think:

```text
Set
```

Example:

> Return the first duplicate.

```js
function firstDuplicate(values) {
  const seen = new Set();

  for (const value of values) {
    if (seen.has(value)) return value;
    seen.add(value);
  }
}
```

---

## "How many times have I seen this?"

Think:

```text
Map
Object
frequency counter
```

```js
function countWords(words) {
  const counts = new Map();

  for (const word of words) {
    counts.set(word, (counts.get(word) || 0) + 1);
  }

  return counts;
}
```

---

## "What happened most recently?"

Think:

```text
Stack
```

Examples:

- undo;
- nested brackets;
- DFS;
- backtracking.

---

## "What arrived first?"

Think:

```text
Queue
```

Examples:

- task processing;
- BFS;
- waiting lines.

---

## "What is best so far?"

Think:

```text
Accumulator
running maximum/minimum
```

```js
function maxValue(numbers) {
  let max = -Infinity;

  for (const number of numbers) {
    if (number > max) max = number;
  }

  return max;
}
```

---

## "How did I get here?"

Think:

```text
parent/predecessor tracking
```

Useful in:

- path reconstruction;
- graph traversal;
- search trees.

---

# Core Lesson

A data structure is often the answer to:

> What kind of memory does this algorithm need?

That question is more useful than blindly asking:

> Which data structure should I use?
