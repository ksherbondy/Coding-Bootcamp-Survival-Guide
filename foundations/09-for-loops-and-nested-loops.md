# For Loops and Nested Loops: A Beginner Guide

> **Purpose:** This guide explains how `for` loops work, why they are useful, and how nested loops move through data step by step.  
> It is language-agnostic and focuses on the mental model behind loops rather than one specific programming language.

---

## Table of Contents

1. [Who This Guide Is For](#who-this-guide-is-for)
2. [The Big Idea](#the-big-idea)
3. [Why Loops Exist](#why-loops-exist)
4. [What Is a For Loop?](#what-is-a-for-loop)
5. [The Three Main Parts of a For Loop](#the-three-main-parts-of-a-for-loop)
6. [Loop Variables](#loop-variables)
7. [Counting From Zero](#counting-from-zero)
8. [Walking Through a Simple Loop](#walking-through-a-simple-loop)
9. [Loops and Arrays](#loops-and-arrays)
10. [Loop Boundaries](#loop-boundaries)
11. [Off-by-One Errors](#off-by-one-errors)
12. [Changing the Step](#changing-the-step)
13. [Counting Backward](#counting-backward)
14. [Nested Loops](#nested-loops)
15. [Outer Loop vs. Inner Loop](#outer-loop-vs-inner-loop)
16. [Walking Through a Nested Loop](#walking-through-a-nested-loop)
17. [Nested Loops and 2D Data](#nested-loops-and-2d-data)
18. [Comparing Items with Nested Loops](#comparing-items-with-nested-loops)
19. [Swapping Values](#swapping-values)
20. [Common Beginner Confusions](#common-beginner-confusions)
21. [Practice Lab](#practice-lab)
22. [Quick Reference](#quick-reference)
23. [Final Mental Model](#final-mental-model)

---

## Who This Guide Is For

This guide is for students who are learning loops and feel confused by code like:

```text
for index from 0 to length - 1:
    do something
```

or:

```text
for outer from 0 to length - 2:
    for inner from outer + 1 to length - 1:
        compare values
```

Loops are one of the first big unlocks in programming.

Once loops make sense, many other topics become easier:

- arrays
- strings
- tables
- grids
- searching
- sorting
- rendering lists
- processing data
- reading files
- building repeated UI elements

The syntax changes between languages, but the core idea is the same.

---

## The Big Idea

A loop repeats an action.

A `for` loop repeats an action a known number of times, or across a known range.

Beginner-friendly definition:

```text
A for loop is a controlled repetition machine.
```

It usually answers three questions:

```text
Where do I start?
When do I stop?
How do I move each time?
```

Example:

```text
Start at 0.
Keep going while index is less than 5.
Add 1 each time.
```

That gives you:

```text
0, 1, 2, 3, 4
```

Five loop runs.

---

## Why Loops Exist

Without loops, repeated work becomes painful.

Imagine printing five values:

```text
print value[0]
print value[1]
print value[2]
print value[3]
print value[4]
```

That is not too bad for five values.

But what about 75 values?

Or 1,000 values?

Or every item in a database result?

Loops let you write one instruction that repeats.

Instead of writing:

```text
print item 1
print item 2
print item 3
...
print item 100
```

You write:

```text
for each item:
    print item
```

The loop handles the repetition.

---

## What Is a For Loop?

A `for` loop is a loop with a built-in counter or iterator.

Different languages write it differently.

### C / Java / JavaScript Style

```c
for (int i = 0; i < 5; i++) {
    print(i);
}
```

### Python Style

```python
for i in range(5):
    print(i)
```

### Pseudocode Style

```text
for i from 0 to 4:
    print i
```

All three mean roughly:

```text
Run the loop with i equal to 0, 1, 2, 3, and 4.
```

The syntax differs, but the concept is the same.

---

## The Three Main Parts of a For Loop

A traditional `for` loop usually has three pieces:

```text
start; condition; update
```

Example:

```c
for (int i = 0; i < 5; i++) {
    print(i);
}
```

Breakdown:

| Part | Example | Meaning |
|---|---|---|
| Start | `int i = 0` | Begin with `i` equal to 0 |
| Condition | `i < 5` | Keep looping while this is true |
| Update | `i++` | Add 1 after each loop run |

### Step-by-Step

```text
1. Set i = 0.
2. Check: is i < 5?
3. If true, run the loop body.
4. Update i.
5. Go back to step 2.
6. Stop when i < 5 is false.
```

The loop does not run forever because the update eventually makes the condition false.

---

## Loop Variables

The loop variable keeps track of the current position or count.

Common names:

```text
i
j
k
index
row
column
outer
inner
```

Short names like `i`, `j`, and `k` are common, but beginners often understand better with descriptive names.

Instead of:

```text
for i from 0 to rows:
    for j from 0 to columns:
```

Use:

```text
for row from 0 to rowCount:
    for column from 0 to columnCount:
```

Clear names make the loop easier to read.

---

## Counting From Zero

Many programming loops start at `0`.

This connects directly to arrays.

If an array has 5 values:

```text
values = [10, 20, 30, 40, 50]
```

The indexes are:

```text
Index:   0   1   2   3   4
Value:  10  20  30  40  50
```

So a loop that visits every value often starts at `0`:

```text
for index from 0 to 4:
    print values[index]
```

Or:

```text
for index from 0 while index < length:
    print values[index]
```

If `length` is 5:

```text
index < 5
```

gives:

```text
0, 1, 2, 3, 4
```

That is exactly what we need.

---

## Walking Through a Simple Loop

Look at this pseudocode:

```text
for x from 0 while x < 5:
    print x
```

Walkthrough:

| Loop Run | x | Is x < 5? | Action |
|---|---:|---|---|
| 1 | 0 | true | print 0 |
| 2 | 1 | true | print 1 |
| 3 | 2 | true | print 2 |
| 4 | 3 | true | print 3 |
| 5 | 4 | true | print 4 |
| Stop check | 5 | false | stop |

Output:

```text
0
1
2
3
4
```

Notice:

```text
The loop starts at 0.
The loop stops before 5.
The loop runs 5 times.
```

This is why `index < length` is such a common pattern.

---

## Loops and Arrays

Loops are often used to visit every element in an array.

Example:

```text
numbers = [95, 60, 6, 86, 50, 24]
```

Indexes:

```text
Index:    0   1   2   3   4   5
Value:   95  60  6   86  50  24
```

To print every value:

```text
for index from 0 while index < length:
    print numbers[index]
```

Walkthrough:

```text
index = 0 → numbers[0] → 95
index = 1 → numbers[1] → 60
index = 2 → numbers[2] → 6
index = 3 → numbers[3] → 86
index = 4 → numbers[4] → 50
index = 5 → numbers[5] → 24
```

The loop variable becomes the index.

That is the key connection:

```text
Loop counter = current array position
```

---

## Loop Boundaries

The hardest part of many loops is knowing where they should stop.

Common loop pattern:

```text
for index = 0; index < length; index++
```

This means:

```text
Start at 0.
Stop before length.
Move by 1 each time.
```

For an array of length 6:

```text
index values: 0, 1, 2, 3, 4, 5
```

That is correct because the last index is:

```text
length - 1
```

### Safe Pattern

```text
index < length
```

This avoids going past the end of the array.

---

## Off-by-One Errors

An off-by-one error happens when a loop runs one too many or one too few times.

Example mistake:

```text
for index = 0; index <= length; index++
```

If length is 6, this gives:

```text
0, 1, 2, 3, 4, 5, 6
```

But valid indexes are only:

```text
0, 1, 2, 3, 4, 5
```

Index `6` is outside the array.

That is out of bounds.

### Common Rule

For zero-based arrays:

```text
Use index < length
```

not:

```text
index <= length
```

### Mental Check

Ask:

```text
What is the first valid index?
What is the last valid index?
Will my loop reach only those indexes?
```

---

## Changing the Step

A loop does not always have to move by 1.

### Count by 2

```text
for index = 0; index < 10; index += 2
```

Values:

```text
0, 2, 4, 6, 8
```

### Count by 5

```text
for count = 0; count <= 20; count += 5
```

Values:

```text
0, 5, 10, 15, 20
```

The update controls how the loop moves.

Common updates:

| Update | Meaning |
|---|---|
| `i++` | add 1 |
| `i += 2` | add 2 |
| `i--` | subtract 1 |
| `i -= 5` | subtract 5 |

---

## Counting Backward

Loops can count backward.

Example:

```text
for index = 5; index >= 0; index--
```

Values:

```text
5, 4, 3, 2, 1, 0
```

This is useful when:

- processing items from the end
- removing items while looping
- reversing output
- counting down

Example:

```text
for index from length - 1 down to 0:
    print values[index]
```

This prints an array backward.

---

## Nested Loops

A nested loop is a loop inside another loop.

Example:

```text
for outer from 0 to 2:
    for inner from 0 to 2:
        print outer, inner
```

The outer loop controls the larger cycle.

The inner loop completes all of its runs for each single run of the outer loop.

Output pattern:

```text
outer = 0, inner = 0
outer = 0, inner = 1
outer = 0, inner = 2

outer = 1, inner = 0
outer = 1, inner = 1
outer = 1, inner = 2

outer = 2, inner = 0
outer = 2, inner = 1
outer = 2, inner = 2
```

Important idea:

```text
For every one outer loop run,
the inner loop runs completely.
```

---

## Outer Loop vs. Inner Loop

The **outer loop** is the loop on the outside.

The **inner loop** is the loop inside it.

Example:

```text
for row from 0 to rowCount - 1:
    for column from 0 to columnCount - 1:
        print grid[row][column]
```

Here:

```text
outer loop = row loop
inner loop = column loop
```

That means:

```text
Pick a row.
Visit every column in that row.
Move to the next row.
Visit every column in that row.
Repeat.
```

### Excel / Table Mental Model

For a table:

```text
outer loop = which row am I on?
inner loop = which column am I on inside that row?
```

---

## Walking Through a Nested Loop

Consider this:

```text
for outer = 0; outer < 3; outer++:
    for inner = outer + 1; inner < 4; inner++:
        print outer, inner
```

This is a common pattern when comparing one item to the items after it.

### Outer = 0

Inner starts at:

```text
outer + 1 = 1
```

So:

```text
outer = 0, inner = 1
outer = 0, inner = 2
outer = 0, inner = 3
```

### Outer = 1

Inner starts at:

```text
outer + 1 = 2
```

So:

```text
outer = 1, inner = 2
outer = 1, inner = 3
```

### Outer = 2

Inner starts at:

```text
outer + 1 = 3
```

So:

```text
outer = 2, inner = 3
```

### Output

```text
0, 1
0, 2
0, 3
1, 2
1, 3
2, 3
```

Notice what this avoids:

```text
0, 0
1, 1
2, 2
3, 3
```

It does not compare an item to itself.

It also avoids repeating the same pair backward.

For example, if you already compared:

```text
0, 2
```

you usually do not need:

```text
2, 0
```

---

## Nested Loops and 2D Data

Nested loops are natural for 2D arrays, tables, grids, and boards.

Example grid:

```text
grid = [
  [1, 2, 3],
  [4, 5, 6],
  [7, 8, 9]
]
```

Use:

```text
for row = 0; row < numberOfRows; row++:
    for column = 0; column < numberOfColumns; column++:
        print grid[row][column]
```

Walkthrough:

```text
row = 0
    column = 0 → grid[0][0] → 1
    column = 1 → grid[0][1] → 2
    column = 2 → grid[0][2] → 3

row = 1
    column = 0 → grid[1][0] → 4
    column = 1 → grid[1][1] → 5
    column = 2 → grid[1][2] → 6

row = 2
    column = 0 → grid[2][0] → 7
    column = 1 → grid[2][1] → 8
    column = 2 → grid[2][2] → 9
```

A good beginner translation:

```text
The outer loop moves down the rows.
The inner loop moves across the columns.
```

---

## Comparing Items with Nested Loops

Nested loops can compare values in an array.

Example array:

```text
numbers = [95, 60, 6, 86, 50, 24]
```

A comparison pattern:

```text
for outer = 0; outer < length - 1; outer++:
    for inner = outer + 1; inner < length; inner++:
        if numbers[outer] > numbers[inner]:
            swap them
```

This is not the same as the classic adjacent-swap bubble sort pattern, but it is a useful comparison-based sorting pattern.

The idea is:

```text
Outer chooses the position we are trying to fill.
Inner searches the rest of the array for a better value.
If a smaller value is found, swap it into the outer position.
```

### Why `outer < length - 1`?

If the array has 6 values, valid indexes are:

```text
0, 1, 2, 3, 4, 5
```

The outer loop only needs to go to index `4`.

By the time indexes `0` through `4` are in the right place, index `5` is already the remaining value.

So:

```text
outer < length - 1
```

For length 6:

```text
outer < 5
```

Outer values:

```text
0, 1, 2, 3, 4
```

### Why `inner = outer + 1`?

Because the inner loop compares the outer value to the values after it.

If:

```text
outer = 0
```

then:

```text
inner starts at 1
```

No need to compare index `0` to itself.

If:

```text
outer = 2
```

then:

```text
inner starts at 3
```

Because indexes before `2` have already been handled.

---

## Swapping Values

A swap exchanges two values.

Suppose:

```text
a = 95
b = 60
```

If we want:

```text
a = 60
b = 95
```

we need a temporary holding place.

### Why Temporary Storage Is Needed

Wrong idea:

```text
a = b
b = a
```

If `a = 95` and `b = 60`, then:

```text
a = b  → a becomes 60
b = a  → b becomes 60
```

The original `95` is lost.

### Correct Swap

```text
temp = a
a = b
b = temp
```

Step-by-step:

```text
temp = a  → temp holds 95
a = b     → a becomes 60
b = temp  → b becomes 95
```

Now the values are swapped.

### Array Swap

For array positions:

```text
temp = numbers[outer]
numbers[outer] = numbers[inner]
numbers[inner] = temp
```

This means:

```text
Save the outer value.
Move the inner value into the outer spot.
Move the saved outer value into the inner spot.
```

---

## Common Beginner Confusions

### Confusion 1: “Why does the loop start at 0?”

Because arrays in many languages start at index 0.

If you are looping through an array, starting at 0 lets you access the first element.

### Confusion 2: “Why do we use `< length` instead of `<= length`?”

Because the last valid index is usually:

```text
length - 1
```

If length is 6, the last index is 5.

`index < length` stops at 5.

`index <= length` tries to access 6, which is out of bounds.

### Confusion 3: “What does `i++` mean?”

It means:

```text
increase i by 1
```

Same idea as:

```text
i = i + 1
```

### Confusion 4: “Why does the inner loop restart?”

Because for every outer loop value, the inner loop begins again according to its own start rule.

Example:

```text
inner = outer + 1
```

So when `outer` changes, the inner loop's starting point changes too.

### Confusion 5: “Why does the inner loop run more times than the outer loop?”

The inner loop can run many times for each one outer loop run.

Example:

```text
outer = 0
    inner = 1, 2, 3, 4, 5
```

That is one outer run and five inner runs.

### Confusion 6: “Why do nested loops feel so slow?”

Nested loops multiply work.

If an outer loop runs 10 times and an inner loop runs 10 times for each outer run:

```text
10 × 10 = 100 total inner actions
```

For 100 and 100:

```text
100 × 100 = 10,000
```

Nested loops can become expensive for large data.

### Confusion 7: “What is the difference between outer and inner?”

Outer controls the larger pass.

Inner does repeated work inside each pass.

For grids:

```text
outer = row
inner = column
```

For comparing items:

```text
outer = current item being placed/checked
inner = other items being compared against it
```

### Confusion 8: “Why do we need temp when swapping?”

Because assigning one variable into another overwrites the old value.

The temp variable preserves one value long enough to complete the exchange.

### Confusion 9: “Is every nested loop a sort?”

No.

Nested loops can be used for many things:

- printing grids
- comparing pairs
- checking duplicates
- generating combinations
- processing tables
- searching 2D data
- drawing shapes

Sorting is only one use case.

### Confusion 10: “Why does changing the loop condition break everything?”

Because the loop condition controls when the loop stops.

A small boundary change can cause:

- missing the last item
- going out of bounds
- infinite loops
- skipping needed comparisons

Loop boundaries matter.

---

## Practice Lab

These exercises are language-agnostic. Use pseudocode or your preferred language.

### Practice 1: Count From 0 to 4

Write a loop that prints:

```text
0
1
2
3
4
```

Pseudocode:

```text
for i = 0; i < 5; i++:
    print i
```

### Practice 2: Print Every Array Value

Given:

```text
numbers = [3, 6, 9, 12]
```

Write a loop that prints every value.

Expected index walkthrough:

```text
index = 0 → 3
index = 1 → 6
index = 2 → 9
index = 3 → 12
```

### Practice 3: Find the Last Index

Given:

```text
items = ["a", "b", "c", "d", "e"]
```

Answer:

```text
length = ?
first index = ?
last index = ?
```

Expected:

```text
length = 5
first index = 0
last index = 4
```

### Practice 4: Count Backward

Write a loop that prints:

```text
5
4
3
2
1
0
```

Pseudocode:

```text
for i = 5; i >= 0; i--:
    print i
```

### Practice 5: Nested Loop Coordinates

Write a nested loop that prints:

```text
row 0, column 0
row 0, column 1
row 0, column 2
row 1, column 0
row 1, column 1
row 1, column 2
```

Pseudocode:

```text
for row = 0; row < 2; row++:
    for column = 0; column < 3; column++:
        print row, column
```

### Practice 6: Walk the Pair Comparison Pattern

Given:

```text
values = [8, 3, 5, 1]
```

List the outer/inner pairs for:

```text
for outer = 0; outer < length - 1; outer++:
    for inner = outer + 1; inner < length; inner++:
```

Expected:

```text
0,1
0,2
0,3
1,2
1,3
2,3
```

### Practice 7: Swap Two Values

Given:

```text
a = 10
b = 20
```

Use a temp variable to swap them.

Expected:

```text
temp = a
a = b
b = temp
```

Result:

```text
a = 20
b = 10
```

### Practice 8: Explain a Loop in English

Explain this in plain English:

```text
for outer = 0; outer < length - 1; outer++:
    for inner = outer + 1; inner < length; inner++:
        if values[outer] > values[inner]:
            swap values[outer] and values[inner]
```

Possible answer:

```text
For each position in the array, compare it with every value after it.
If a smaller value is found later in the array, swap it into the current position.
Repeat until the array is ordered.
```

---

## Quick Reference

| Concept | Beginner Meaning |
|---|---|
| Loop | Repeats an action |
| For loop | Controlled loop with start, stop condition, and update |
| Loop variable | Tracks the current loop position |
| Initialization | Where the loop starts |
| Condition | Rule that decides whether the loop keeps going |
| Update | How the loop variable changes each time |
| `i++` | Add 1 to `i` |
| `i--` | Subtract 1 from `i` |
| Array index | Position inside an array |
| `index < length` | Common safe pattern for array loops |
| Off-by-one error | Loop runs one too many or one too few times |
| Nested loop | Loop inside another loop |
| Outer loop | Larger cycle |
| Inner loop | Repeated cycle inside the outer loop |
| Swap | Exchange two values |
| Temp variable | Temporary storage used during a swap |

---

## Common Loop Patterns

### Visit Every Array Item

```text
for index = 0; index < length; index++:
    use array[index]
```

### Count Backward

```text
for index = length - 1; index >= 0; index--:
    use array[index]
```

### Visit Every Cell in a Grid

```text
for row = 0; row < rowCount; row++:
    for column = 0; column < columnCount; column++:
        use grid[row][column]
```

### Compare Each Item to Items After It

```text
for outer = 0; outer < length - 1; outer++:
    for inner = outer + 1; inner < length; inner++:
        compare array[outer] and array[inner]
```

### Swap Two Values

```text
temp = a
a = b
b = temp
```

### Swap Two Array Values

```text
temp = array[first]
array[first] = array[second]
array[second] = temp
```

---

## Final Mental Model

A `for` loop is a controlled repetition machine.

It asks:

```text
Where do I start?
When do I stop?
How do I move?
What do I do each time?
```

For arrays:

```text
The loop variable often becomes the array index.
```

For 2D data:

```text
The outer loop usually chooses the row.
The inner loop usually chooses the column.
```

For comparing values:

```text
The outer loop chooses the current value.
The inner loop checks the values after it.
```

For swapping:

```text
Use a temporary variable so one value is not lost.
```

The real skill is learning to trace a loop slowly.

Do not try to understand every loop all at once.

Track the variables one pass at a time:

```text
What is outer?
What is inner?
What condition is being checked?
What value is being accessed?
What changes before the next pass?
```

Once you can trace a loop step by step, loops stop being mysterious.

They become one of the most useful tools in programming.
