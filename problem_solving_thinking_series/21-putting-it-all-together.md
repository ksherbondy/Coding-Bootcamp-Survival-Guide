# 21. Putting It All Together
## From Problem Recognition to Working Code

This lesson is the capstone for the Problem-Solving Thinking Series.

The purpose is not to introduce a new data structure or algorithm.

It is to show how all of the earlier lessons combine when you face a problem where:

- you recognize the general shape;
- you know roughly what must happen;
- but you stall when it is time to turn the idea into working code.

That gap is normal.

Recognizing the solution family and implementing the solution are related skills, but they are not the same skill.

---

# The Problem

You are given a `number` in string format, and a `target` number. We can "cut" the `number` into some pieces, and then calculate their sum value. Your task is to find the maximum sum value, which is closest to the `target` number, but no more than the `target` number. If can not got a valid result, return `-1`.

For:

```js
number = "12346"
target = 50
```

the output should be:

```js
43
```

because one possible cutting is:

```text
1 + 2 + 34 + 6 = 43
```

and `43` is the largest valid sum that does not exceed `50`.

Other examples:

```text
number = "222", target = 2
→ -1

number = "106", target = 10
→ 7

number = "106", target = 20
→ 16

number = "1234", target = 10
→ 10

number = "1234", target = 20
→ 19

number = "1234", target = 30
→ 28

number = "1234", target = 50
→ 46

number = "1234", target = 1235
→ 1234
```

Constraints:

```text
0 < number.length < 20
0 < target <= 10000
```

---

# 1. First Pass: Do Not Code Yet

The first step is to identify the important phrases.

Key wording:

```text
"cut the number into some pieces"
"many ways"
"maximum sum"
"closest to target"
"no more than target"
"if no valid result, return -1"
```

Each phrase tells us something.

---

# 2. Translate the Wording Into Programming Signals

## "Cut the number into some pieces"

This means we are partitioning a string.

Example:

```text
"1234"
```

can become:

```text
1 | 2 | 3 | 4
1 | 2 | 34
1 | 23 | 4
1 | 234
12 | 3 | 4
12 | 34
123 | 4
1234
```

So the problem is not simply about arithmetic.

It is also about exploring possible partitions.

## "Many ways"

This should trigger:

```text
multiple possibilities
search space
recursion?
backtracking?
```

We do not know yet which implementation will be easiest, but the wording suggests that we must explore alternatives.

## "Maximum sum"

This tells us we need to track:

```text
best result so far
```

Possible tool:

```text
running maximum
```

## "No more than target"

This gives us a validity rule:

```js
sum <= target
```

The comparison matters.

This is a boundary condition.

## "If no valid result"

This tells us we need an explicit failure case:

```js
return -1;
```

---

# 3. Classify the Problem

The problem belongs to several families at once.

```text
string partitioning
combinatorial search
recursion/backtracking
running maximum
boundary filtering
```

Real problems often do not belong to exactly one category.

---

# 4. Ask: What Must I Remember?

From the earlier lesson:

> What information must survive while the program runs?

For this problem, we need to remember:

```text
where we are in the string
the sum built so far
the completed sums or best sum so far
the target
```

A useful recursive state is:

```text
start index
current sum
```

---

# 5. Before Recursion: Understand the Local Operation

A common mistake is to jump directly into recursion.

Instead, first solve the smaller question:

> From a given position in the string, how can I generate every possible next piece?

Suppose:

```js
const str = "12346";
let start = 0;
```

We want:

```text
"1"
"12"
"123"
"1234"
"12346"
```

That can be generated with:

```js
for (let end = start + 1; end <= str.length; end++) {
  const piece = str.slice(start, end);
  console.log(piece);
}
```

This is the first implementation milestone.

Do not worry about solving the entire problem yet.

Just prove that you can generate every possible first piece.

---

# 6. The Important Index Relationship

If we choose:

```text
"12"
```

from:

```text
"12346"
```

then the next unprocessed character is:

```text
"3"
```

The chosen piece was created by:

```js
str.slice(0, 2)
```

so the next position is:

```js
2
```

That means the value of `end` is also the next `start`.

This observation gives us the recursive step:

```js
explore(end, ...)
```

That is the bridge from slicing to recursion.

---

# 7. Build the Recursive Skeleton

Start with only structure.

```js
function cut(number, target) {
  const str = String(number);

  function explore(start) {
    if (start === str.length) {
      return;
    }

    for (let end = start + 1; end <= str.length; end++) {
      const piece = str.slice(start, end);

      console.log(piece);

      explore(end);
    }
  }

  explore(0);
}
```

This is not the full solution.

That is fine.

At this stage, the goal is only:

> Can I recursively visit every possible cut path?

---

# 8. Add the Running Sum

Now the problem needs arithmetic.

Change:

```js
explore(start)
```

into:

```js
explore(start, sum)
```

Convert the slice into a number:

```js
const piece = Number(str.slice(start, end));
```

Then pass the updated total:

```js
explore(end, sum + piece);
```

Now the helper becomes:

```js
function explore(start, sum) {
  if (start === str.length) {
    console.log("finished partition:", sum);
    return;
  }

  for (let end = start + 1; end <= str.length; end++) {
    const piece = Number(str.slice(start, end));

    explore(end, sum + piece);
  }
}
```

And begin with:

```js
explore(0, 0);
```

Now the recursion is generating completed partition sums.

For:

```text
"1234"
```

it reaches sums corresponding to:

```text
1 + 2 + 3 + 4
1 + 2 + 34
1 + 23 + 4
1 + 234
12 + 3 + 4
12 + 34
123 + 4
1234
```

---

# 9. Use a Temporary Collection if It Helps You Think

An optimized solution may not need to store every total.

But while learning, storing them can make the algorithm much easier to inspect.

```js
let totals = [];
```

At the base case:

```js
if (start === str.length) {
  return totals.push(sum);
}
```

Now after recursion:

```js
console.log(totals);
```

you can see all completed partition sums.

This is useful for debugging and understanding.

> The first version should be easy to reason about.

Optimization can come later.

---

# 10. Filter by the Rule From the Problem

The problem says:

> no more than the target

Translate directly:

```js
element <= target
```

So:

```js
const newArray = totals.filter(element => element <= target);
```

Now every value in `newArray` is valid.

This is an example of turning English into boolean logic.

---

# 11. Find the Closest Valid Value

Once everything greater than `target` has been removed, the remaining answer is the valid value closest to the target.

One approach:

```js
const closest = newArray.reduce((prev, curr) => {
  return Math.abs(curr - target) < Math.abs(prev - target)
    ? curr
    : prev;
});
```

This compares the distance between each value and the target.

Because all values are already:

```text
<= target
```

the closest value is also simply the largest valid value.

But the distance approach is still logically correct.

---

# 12. The Missing Edge Case

At this point the main logic works.

But consider:

```js
number = "222"
target = 2
```

Possible sums include:

```text
2 + 2 + 2 = 6
2 + 22 = 24
22 + 2 = 24
222 = 222
```

None satisfy:

```js
sum <= 2
```

So:

```js
newArray
```

becomes:

```js
[]
```

Calling:

```js
newArray.reduce(...)
```

without an initial value on an empty array throws an error.

The problem statement already warned us:

> If no valid result, return `-1`.

So we add:

```js
if (newArray.length === 0) return -1;
```

This is a perfect example of why edge-case discovery matters.

---

# 13. The Working Iterative Version

The implementation developed step by step into:

```js
function cut(number, target) {
  const str = String(number);
  let totals = [];

  function explore(start, sum) {
    if (start === str.length) {
      return totals.push(sum);
    }

    for (let end = start + 1; end <= str.length; end++) {
      const piece = Number(str.slice(start, end));

      explore(end, sum + piece);
    }
  }

  explore(0, 0);

  const newArray = totals.filter(element => element <= target);

  if (newArray.length === 0) return -1;

  const closest = newArray.reduce((prev, curr) => {
    return Math.abs(curr - target) < Math.abs(prev - target)
      ? curr
      : prev;
  });

  return closest;
}
```

This version is valuable because every piece maps directly to part of the reasoning process.

---

# 14. Notice How the Solution Was Actually Built

The implementation did not appear all at once.

It evolved like this:

```text
I need to cut the string somehow.
        ↓
I can use slice().
        ↓
From index 0, try every possible end position.
        ↓
I can generate every possible first piece.
        ↓
After choosing a piece, continue from where that piece ended.
        ↓
That suggests recursion.
        ↓
The recursive function needs the current index.
        ↓
I also need the running sum.
        ↓
At the end of the string, I have one complete partition.
        ↓
Store that sum.
        ↓
Filter sums that exceed target.
        ↓
Choose the closest valid one.
        ↓
Handle the empty-valid-results case.
```

This is the real lesson.

The final code is not the important part.

The important part is the chain of small observations.

---

# 15. Recognizing the Problem Was Not the Same as Implementing It

It is possible to know:

```text
this needs recursion
```

and still not know what the recursive function should look like.

That does not mean the pattern recognition failed.

The missing questions are:

```text
What changes between recursive calls?
What stays the same?
What is the base case?
What choice is made at each level?
What state must be passed forward?
```

For this problem:

```text
What changes?
→ start index
→ running sum

What stays the same?
→ original string
→ target

What is the base case?
→ start reaches string length

What choice is made?
→ where the next cut ends
```

Once those questions are answered, the recursive function becomes much easier to construct.

---

# 16. Draw the Search Tree

For:

```text
"123"
```

the recursion can be visualized as:

```text
start
├── 1
│   ├── 2
│   │   └── 3
│   │       → 1 + 2 + 3 = 6
│   │
│   └── 23
│       → 1 + 23 = 24
│
├── 12
│   └── 3
│       → 12 + 3 = 15
│
└── 123
    → 123
```

The recursion is not mysterious.

It is simply walking this decision tree.

---

# 17. Why There Are So Many Possibilities

For a string of length `n`, there are:

```text
n - 1
```

spaces between digits.

For:

```text
1234
```

there are three boundaries:

```text
1 _ 2 _ 3 _ 4
```

At each boundary:

```text
cut
or
do not cut
```

That gives:

```text
2^(n - 1)
```

possible partition patterns.

For a length-19 string:

```text
2^18 = 262,144
```

This explains the performance warning in the challenge.

It also tells us that pruning may eventually matter.

---

# 18. Optimization Comes After Understanding

Once the simple version works, ask:

> What unnecessary work am I doing?

## Optimization 1: Do Not Store Every Total

Instead of:

```js
let totals = [];
```

we could keep:

```js
let best = -1;
```

At the base case:

```js
if (sum <= target && sum > best) {
  best = sum;
}
```

This avoids building a large array of totals.

## Optimization 2: Stop Dead Branches

All pieces are nonnegative.

Therefore, if:

```js
sum > target
```

adding more pieces cannot make the sum smaller.

So that branch can stop immediately.

Conceptually:

```js
if (sum > target) return;
```

This is pruning.

## Optimization 3: Whole Number Already Fits

If:

```js
Number(number) <= target
```

then leaving the entire number uncut produces the largest possible sum.

So the function can return immediately.

---

# 19. But Do Not Optimize Too Early

The optimized version may be shorter or faster.

It is not necessarily the best version to learn from first.

The development path matters:

```text
generate everything
→ inspect results
→ verify correctness
→ identify waste
→ remove waste
```

A clear working model creates the understanding needed to optimize safely.

---

# 20. Lessons From the Entire Series Used Here

This single problem uses almost every major concept from the series.

## Reading Problems Like a Programmer

We identified:

```text
cut
many ways
maximum
closest
no more than
```

## Problem Decomposition

We separated:

```text
generate pieces
explore partitions
calculate sums
filter valid sums
choose best
handle failure
```

## What Must I Remember?

We identified:

```text
start
sum
best/totals
```

## Algorithm Selection

"Many ways" and partitioning suggested:

```text
recursion
backtracking
```

## Brute Force First

We generated all partition sums before optimizing.

## Recognizing Repeated Work

After correctness, we noticed that storing all totals was unnecessary.

## Representation

Keeping the input as a string made:

```js
slice()
```

the natural operation.

## Boundary Thinking

"No more than target" became:

```js
<= target
```

## Edge Cases

"No valid result" became:

```js
return -1;
```

## Trace Before You Code

Tracing:

```text
start
end
piece
sum
```

makes the recursion understandable.

## Invariants

During a recursive call:

> `sum` is the total of all pieces chosen before `start`.

That invariant explains the meaning of the state.

## Complexity

The number of possible cut patterns grows exponentially.

## Space/Time Tradeoffs

Storing all completed totals is easy to understand but uses more memory.

Tracking only `best` reduces space.

---

# 21. The Most Important Lesson: Implementation Stalling

A programmer can correctly identify:

```text
This is recursion.
```

and still stall.

When that happens, do not ask:

> "Why can't I write the recursion?"

Ask smaller questions.

```text
What is one choice?
What changes after that choice?
Where does the next step begin?
What state must be passed?
When am I finished?
```

For this problem:

```text
One choice:
→ choose the end of the next slice

What changes:
→ start becomes end

State passed:
→ running sum

Finished:
→ start === str.length
```

That is enough to build the recursive skeleton.

---

# 22. A General Recursion Construction Checklist

When you suspect recursion, fill this out.

## What is the current position/state?

```text
start index
```

## What choices exist from here?

```text
every possible end index
```

## How do I make one choice?

```js
str.slice(start, end)
```

## What is the smaller remaining problem?

```text
everything beginning at end
```

## What must be carried forward?

```text
sum + chosen piece
```

## What is the base case?

```text
start === str.length
```

## What result is produced at the base case?

```text
one complete partition sum
```

This checklist can be reused for:

- permutations;
- combinations;
- path exploration;
- subset problems;
- partitioning;
- maze solving;
- decision trees.

---

# 23. Student Exercise: Rebuild It in Stages

Students should not copy the final function immediately.

Build it in stages.

## Stage 1

Print all possible first slices.

Expected idea:

```js
str.slice(start, end)
```

## Stage 2

Turn the slice generator into:

```js
explore(start)
```

and recursively move to:

```js
explore(end)
```

## Stage 3

Add:

```js
sum
```

to the recursive state.

## Stage 4

Print completed sums at the base case.

## Stage 5

Store completed sums.

## Stage 6

Filter:

```js
sum <= target
```

## Stage 7

Choose the closest valid result.

## Stage 8

Handle:

```text
no valid result
```

## Stage 9

Only after it works, identify possible optimizations.

---

# 24. Reflection Questions

After solving the problem, ask:

1. Which words in the problem pointed toward recursion?
2. Why was `slice()` a useful representation tool?
3. What exactly does `start` represent?
4. Why does the recursive call use `end`?
5. What exactly does `sum` represent at any moment?
6. What is the base case?
7. Why is `sum <= target` a boundary condition?
8. Why can an empty filtered array occur?
9. What repeated or unnecessary work exists in the first working version?
10. What would you change if the input length were much larger?
11. Could the problem be solved without storing every total?
12. What part of the implementation caused the most hesitation?

---

# 25. Final Mental Model

The full problem-solving process is:

```text
READ
↓
IDENTIFY SIGNAL WORDS
↓
CLASSIFY THE PROBLEM
↓
DECOMPOSE IT
↓
ASK WHAT MUST BE REMEMBERED
↓
BUILD ONE SMALL MECHANISM
↓
TRACE IT
↓
ADD STATE
↓
HANDLE THE BASE CASE
↓
MAKE IT CORRECT
↓
TEST BOUNDARIES AND EDGE CASES
↓
LOOK FOR REPEATED WORK
↓
OPTIMIZE
```

The critical point is:

> You do not need to see the whole implementation before you begin.

You only need to know the next small thing that must work.

In this problem, that first small thing was:

```text
From this index, generate every possible next piece.
```

Once that worked, the next step became visible.

That is how difficult problems become manageable.
