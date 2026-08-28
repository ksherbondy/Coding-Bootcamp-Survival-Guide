# 1. Reading Problems Like a Programmer

## Objective

Learn to treat problem statements as collections of useful signals.

A programming problem is not just prose. It contains clues about:

- output shape;
- constraints;
- data relationships;
- ordering;
- repetition;
- stopping conditions;
- likely data structures;
- likely algorithms.

---

# The Core Translation

Written math works this way:

```text
sum      → +
product  → *
quotient → /
```

Programming can work the same way:

```text
unique      → Set?
frequency   → Map?
sorted      → binary search? two pointers?
contiguous  → sliding window?
first       → early return?
all         → collect/filter?
```

The question mark matters.

These are **clues, not rules**.

---

# A First Example

## Problem

> Given an array of numbers, return `true` if any number appears more than once.

Before coding, highlight:

```text
any
appears more than once
```

Translate:

```text
any → can stop as soon as one match is found
appears more than once → need to remember what has already appeared
```

Now ask:

> What data structure naturally answers "have I seen this before?"

Candidate:

```text
Set
```

Possible solution:

```js
function hasDuplicate(numbers) {
  const seen = new Set();

  for (const number of numbers) {
    if (seen.has(number)) {
      return true;
    }

    seen.add(number);
  }

  return false;
}
```

The important part is not the syntax.

It is the reasoning chain:

```text
duplicate
→ seen before
→ membership
→ Set
```

---

# A Repeatable Reading Routine

Before coding, ask:

1. What must I return?
2. Which words seem important?
3. What information must survive between iterations?
4. Does order matter?
5. Do duplicates matter?
6. Is the data sorted?
7. Can I stop early?
8. Am I looking for one answer or all answers?
9. Is the problem about values, positions, relationships, paths, or state?
10. What is the simplest correct solution?

---

# Signal Words

| Wording | Start thinking |
|---|---|
| unique / duplicate | Set |
| count / frequency | Map / Object |
| first | early return |
| all | collect / filter |
| sorted | exploit ordering |
| pair | Set / Map / two pointers |
| contiguous | sliding window |
| path / route | graph |
| parent / child | tree |
| most recent | stack |
| arrival order | queue |
| closest | distance + running minimum |
| top K | heap / priority queue |
| every possible | recursion / backtracking |
| powers of two | binary / shifts / log2 |

---

# Instructor Prompt

Give students a problem and do **not** let them code.

Ask only:

```text
What words matter?
What do those words suggest?
What must the program remember?
```

That discussion is the lesson.
