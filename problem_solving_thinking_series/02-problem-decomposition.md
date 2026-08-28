# 2. Problem Decomposition

## Objective

Turn a large written problem into smaller, testable questions.

A common beginner reaction is:

> "I don't know how to solve the whole thing."

That is normal.

Do not solve the whole thing yet.

Break it apart.

---

# Decomposition Checklist

Ask:

```text
What is the input?
What is the output?
What are the rules?
What are the special cases?
What changes over time?
What must be remembered?
What can be solved independently?
```

---

# Example

## Problem

> Given a list of student scores, return the name of the student with the highest average. Ignore students with no submitted assignments.

This sounds larger than it is.

Break it down:

```text
1. Loop through students.
2. Skip students with no assignments.
3. Compute each student's average.
4. Track the highest average so far.
5. Track the name associated with that average.
6. Return the name.
```

Now each part is familiar.

Possible solution:

```js
function topStudent(students) {
  let bestName = null;
  let bestAverage = -Infinity;

  for (const student of students) {
    if (student.scores.length === 0) {
      continue;
    }

    const total = student.scores.reduce((sum, score) => sum + score, 0);
    const average = total / student.scores.length;

    if (average > bestAverage) {
      bestAverage = average;
      bestName = student.name;
    }
  }

  return bestName;
}
```

---

# The Key Habit

When a problem feels too large, replace:

> "How do I solve this?"

with:

> "What is the first small thing I need to know?"

Then:

> "What do I need after that?"

---

# Exercise

Problem:

> Given a string, return the most common character, ignoring spaces and capitalization.

Do not code immediately.

Decompose:

```text
1. Normalize capitalization.
2. Ignore spaces.
3. Count characters.
4. Track the largest count.
5. Return that character.
```

Only after decomposition should students choose syntax.
