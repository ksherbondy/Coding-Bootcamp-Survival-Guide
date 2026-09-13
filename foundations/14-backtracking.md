# Backtracking: From "Try Something" to Constraint Search

> A concept-first lesson that starts with a five-year-old mental model and builds all the way to formal search trees, constraint satisfaction, pruning, heuristics, complexity, correctness, and advanced optimization.

## Learning Objectives

By the end of this lesson, you should be able to:

- explain backtracking in plain language
- recognize when a problem is a backtracking problem
- identify choices, constraints, state, base cases, and undo steps
- trace a recursive backtracking algorithm by hand
- build and read a search tree
- explain why undo is essential
- distinguish backtracking from brute force
- explain pruning and constraint propagation
- solve common backtracking families such as permutations, combinations, mazes, N-Queens, Sudoku-like puzzles, and constraint puzzles
- understand candidate generation
- reason about time and space complexity
- explain backtracking as depth-first search over an implicit state-space tree
- relate backtracking to constraint satisfaction problems
- understand MRV, least-constraining value, forward checking, branch-and-bound, memoization, symmetry reduction, bitsets, and other advanced ideas
- reason about soundness and completeness

# 1. Explain It Like You Are Five

Imagine you are in a maze.

You reach a fork:

```text
        start
          |
        fork
       /    \
    left    right
```

You do not know which direction reaches the exit.

So you try left.

If left works, great.

If left reaches a dead end:

```text
go back to the fork
try right
```

That is backtracking.

The whole idea is:

> Make a choice, keep going, and if the choice leads somewhere impossible, undo it and try another choice.

Most backtracking algorithms repeatedly do four things:

```text
1. CHOOSE
2. EXPLORE
3. UNCHOOSE
4. TRY SOMETHING ELSE
```

In code, that often looks like:

```js
makeChoice();

explore();

undoChoice();
```

The undo step is where backtracking gets its name.

# 2. Why Humans Already Do This

Suppose you are solving Sudoku.

You may think:

```text
This square can only be 2 or 4.

If I put 2 here,
that row would have two 2s.

So it must be 4.
```

That is constraint elimination.

Or:

```text
I still cannot tell whether this is 3 or 5.

Let me assume 3.

If that eventually creates a contradiction,
come back and try 5.
```

That is backtracking.

Humans do this without consciously naming every operation.

A computer cannot understand:

```text
"obviously that goes there"
```

It needs exact instructions:

```text
What choices exist?
Which choices are legal?
What changes after a choice?
How do I detect failure?
How do I undo?
When am I finished?
```

Backtracking is a framework for turning those invisible human decisions into mechanical steps.

# 3. The Questions to Ask Before Coding

When you think a problem may require backtracking, answer these first:

```text
What does a complete solution look like?

What decision am I making right now?

What choices are available?

What makes a choice illegal?

What information must I remember?

What changes when I make a choice?

How do I know I am finished?

What must I undo before trying the next choice?
```

If you can answer those in English, the code usually becomes much easier.

# 4. Backtracking Is Search

Suppose you need to build every three-letter arrangement from:

```text
A
B
C
```

without repeating letters.

The possibilities form a decision tree:

```text
                 ""
          /       |       \
         A        B        C
       /  \      / \      / \
      AB  AC    BA  BC   CA  CB
      |    |    |   |    |   |
     ABC  ACB  BAC BCA  CAB CBA
```

You normally do not build that tree explicitly.

Your recursive calls create it implicitly.

Backtracking is usually:

```text
depth-first search
through an implicit decision tree
```

# 5. State

State means:

> Everything the algorithm must know about the partial solution right now.

Examples:

Permutation:

```text
current sequence
which values have already been used
```

N-Queens:

```text
current row
used columns
used diagonals
queen placements
```

Sudoku:

```text
current board
row restrictions
column restrictions
box restrictions
candidate values
```

Maze:

```text
current position
visited positions
current path
```

A major part of designing a backtracking algorithm is deciding what state is actually necessary.

# 6. First Tiny Example: Binary Strings

Generate every binary string of length 3.

At every position, there are two choices:

```text
0
1
```

Using immutable strings:

```js
function binaryStrings(length) {
  const results = [];

  function explore(current) {
    if (current.length === length) {
      results.push(current);
      return;
    }

    explore(current + "0");
    explore(current + "1");
  }

  explore("");

  return results;
}
```

There is no explicit undo because:

```js
current + "0"
```

creates a new string.

Each recursive call receives its own value.

Now use mutable state:

```js
function binaryStrings(length) {
  const results = [];
  const current = [];

  function explore() {
    if (current.length === length) {
      results.push([...current]);
      return;
    }

    current.push(0);
    explore();
    current.pop();

    current.push(1);
    explore();
    current.pop();
  }

  explore();

  return results;
}
```

Now you can clearly see:

```text
choose
explore
undo
```

# 7. Why Undo Matters

Suppose you forget:

```js
current.pop();
```

You explore:

```text
[0, 0, 0]
```

but never restore:

```text
[0, 0]
```

before trying another branch.

Now sibling branches inherit state they should not have.

A central invariant of mutable backtracking is:

> After a recursive branch returns, the caller's state must be exactly what it was before that branch was tried.

This is one of the most important rules in backtracking.

# 8. Generic Backtracking Skeleton

A common conceptual shape is:

```js
function backtrack(state) {
  if (solutionComplete(state)) {
    recordSolution(state);
    return;
  }

  for (const choice of getChoices(state)) {
    if (!isValid(choice, state)) {
      continue;
    }

    makeChoice(choice, state);

    backtrack(state);

    undoChoice(choice, state);
  }
}
```

Read it as:

```text
Are we finished?
    yes → record the answer

Otherwise:
    inspect each possible next choice

    illegal?
        skip it

    legal?
        make the choice

        explore from there

        undo the choice
```

Do not memorize the code before understanding the flow.

# 9. Backtracking vs Brute Force

They are related, but they are not identical.

## Pure brute force

Generate every complete possibility first:

```text
generate everything
then test everything
```

## Backtracking

Build a possibility piece by piece and reject it as soon as it becomes impossible.

Example with queens:

```text
place queen 1
place queen 2

queen 2 attacks queen 1?
→ stop this branch immediately
```

You never bother placing queens 3 through N for that branch.

That early rejection is called:

```text
pruning
```

# 10. Pruning

Pruning means:

> Stop exploring a branch as soon as you can prove it cannot lead to a valid answer.

Suppose you want a sum no greater than 10.

Current sum:

```text
13
```

If all future additions are nonnegative:

```text
this branch can never return to 10 or below
```

So stop.

Conceptually:

```text
             choice
            /      \
        possible   impossible
        /  \          X
       ... ...
```

That `X` can represent thousands or millions of descendants you never need to visit.

A pruning rule should be a proof of impossibility, not a guess.

# 11. Permutations

Generate all permutations of:

```js
[1, 2, 3]
```

State:

```text
current permutation
used values
```

Choice:

```text
any unused number
```

Code:

```js
function permutations(nums) {
  const results = [];
  const current = [];
  const used = new Set();

  function explore() {
    if (current.length === nums.length) {
      results.push([...current]);
      return;
    }

    for (const num of nums) {
      if (used.has(num)) {
        continue;
      }

      used.add(num);
      current.push(num);

      explore();

      current.pop();
      used.delete(num);
    }
  }

  explore();

  return results;
}
```

Trace:

```text
[]
choose 1

[1]
choose 2

[1,2]
choose 3

[1,2,3] → solution

undo 3
undo 2

[1]
choose 3

[1,3]
choose 2

[1,3,2] → solution

...
```

The Set answers:

```text
Have I already used this choice in the current branch?
```

# 12. A Common Pattern: Used State

Many backtracking problems require:

```text
do not reuse this thing
```

Examples:

```text
permutations
queen columns
Sudoku digits
graph nodes on the current path
```

Typical cycle:

```js
if (used.has(choice)) continue;

used.add(choice);

explore();

used.delete(choice);
```

The same idea appears again and again with different meanings.

# 13. Combinations

Suppose you want all 2-element combinations from:

```js
[1, 2, 3, 4]
```

Unlike permutations:

```text
[1,2]
and
[2,1]
```

represent the same combination.

So we introduce a `start` position:

```js
function combinations(nums, size) {
  const results = [];
  const current = [];

  function explore(start) {
    if (current.length === size) {
      results.push([...current]);
      return;
    }

    for (let i = start; i < nums.length; i++) {
      current.push(nums[i]);

      explore(i + 1);

      current.pop();
    }
  }

  explore(0);

  return results;
}
```

The `start` index prevents the search from moving backward and generating reordered duplicates.

# 14. What Moves Forward?

A backtracking function usually has some concept of progress.

Examples:

```text
combinations
→ start index

N-Queens
→ current row

Sudoku
→ next unresolved cell

maze
→ current coordinate

string partition
→ current character index
```

A powerful question is:

> What tells this recursive call how much of the problem has already been solved?

# 15. String Partitioning

Suppose:

```text
"123"
```

can be split as:

```text
1 | 2 | 3
1 | 23
12 | 3
123
```

At a fixed `start`, every possible `end` represents one choice.

```text
"123"
├── "1"
│   ├── "2"
│   │   └── "3"
│   └── "23"
├── "12"
│   └── "3"
└── "123"
```

The important questions are:

```text
Where does the current piece begin?

Where may it end?

After choosing that piece,
where does the next piece begin?

When have I consumed the entire string?
```

A surprising number of recursive partition problems are really boundary-choice problems.

# 16. Maze Example

Suppose:

```text
S . #
# . .
. . E
```

Allowed moves:

```text
up
down
left
right
```

State:

```text
row
column
visited cells
path
```

Choices:

```text
four neighboring coordinates
```

Invalid if:

```text
outside board
wall
already visited
```

Success:

```text
current position is E
```

The logic is:

```text
visit
try neighbor
try deeper
dead end?
undo
try next neighbor
```

# 17. Branch-Specific vs Global State

Visited state can be subtle.

For finding one path:

```text
a permanently visited node may be fine
```

For finding all simple paths:

```text
visited often belongs only to the current branch
```

So after returning:

```js
visited.delete(node);
```

may be necessary.

The general question is:

> Does this restriction apply forever, or only while I am exploring this particular branch?

# 18. N-Queens

Goal:

```text
place N queens
one per row
no shared column
no shared diagonal
```

If you deliberately place one queen per row, you do not need to check row collisions.

State can be:

```text
current row
used columns
used row-col diagonals
used row+col diagonals
```

For position:

```text
(row, col)
```

the diagonals are identified by:

```text
row - col
row + col
```

A position is legal if all three are unused.

The search is:

```text
for current row:
    try each column

    legal?
        place queen
        recurse to next row
        remove queen afterward
```

Dead end:

```text
current row has no legal column
```

Then you backtrack to an earlier queen.

# 19. Four Queens by Hand

For a 4×4 board:

```text
row 0
→ try a column

row 1
→ try a non-attacking column

row 2
→ try a non-attacking column

row 3
→ no legal choice?
```

If row 3 is impossible:

```text
undo row 2's queen
try row 2's next column
```

If every row-2 choice fails:

```text
undo row 1
try row 1's next choice
```

Eventually one solution is:

```text
row → column
[1, 3, 0, 2]
```

The important concept is not the answer.

It is:

> A contradiction discovered late can force reconsideration of an earlier decision.

# 20. Sudoku as Constraint Satisfaction

Sudoku can be modeled as:

```text
variables:
empty cells

domains:
possible digits

constraints:
no duplicate in row
no duplicate in column
no duplicate in 3×3 box
```

A simple backtracking solver does:

```text
find an empty cell

for every legal number:
    place it
    recurse
    undo if it fails
```

But good Sudoku solvers do more work before guessing.

# 21. Candidate Sets

Suppose a cell begins with:

```text
{1,2,3,4,5,6,7,8,9}
```

Its row removes:

```text
1,3,5,8
```

Its column removes:

```text
2,7
```

Its box removes:

```text
4
```

Remaining:

```text
{6,9}
```

Now the search has two choices instead of nine.

This is constraint propagation:

> Use what is already known to shrink future choices before branching.

# 22. Constraint Propagation

Suppose:

```text
cell A → {2,4}
```

Another deduction eliminates `2`.

Now:

```text
cell A → {4}
```

There is no longer a choice.

Place `4` automatically.

That placement may shrink another candidate set:

```text
cell B → {1,4}
```

becomes:

```text
cell B → {1}
```

which causes another forced placement.

This chain reaction is constraint propagation.

A strong puzzle solver often follows:

```text
propagate forced information
↓
solved?
    stop
contradiction?
    backtrack
still ambiguous?
    choose a candidate
```

# 23. Skyscrapers as a Backtracking Problem

For a 4×4 skyscraper puzzle, each row and column must be a permutation of:

```text
1,2,3,4
```

There are:

```text
4! = 24
```

possible line permutations.

Each permutation has:

```text
visibility from one direction
visibility from the other direction
```

So a clue pair reduces those 24 candidates to a smaller bucket.

Example mental model:

```text
clue pair
↓
candidate permutations
↓
crossing rows/columns remove candidates
↓
one candidate remains?
    forced
↓
otherwise eventually branch
```

This is similar to Sudoku, except a candidate may represent an entire row or column permutation rather than one cell value.

# 24. Visibility as a Mechanical Rule

For:

```text
[2, 1, 4, 3]
```

scan from the left.

```text
max = 0
visible = 0

2 > 0
→ visible = 1
→ max = 2

1 > 2?
no

4 > 2
→ visible = 2
→ max = 4

3 > 4?
no
```

So the visibility is:

```text
2
```

This is an important algorithm-design lesson:

Human:

```text
"I can see two buildings."
```

Computer:

```text
scan left to right
track largest value seen
increment when a new maximum appears
```

Backtracking often requires translating visual intuition into exact tests like this.

# 25. Human Reasoning vs Computer Reasoning

Human:

```text
"That obviously cannot be 3."
```

Computer:

```text
candidate = 3

does row already contain 3?
yes

reject
```

Human:

```text
"The 4 has to go there."
```

Computer:

```text
generate all legal candidates
filter candidates against constraints

every surviving candidate
has 4 at this position

therefore place 4
```

The difficult part of writing a solver is often not recursion.

It is converting:

```text
"obvious"
```

into:

```text
a sequence of explicit constraint checks
```

# 26. Search Trees, Branching Factor, and Depth

Suppose the average number of choices per state is:

```text
b
```

and the search depth is:

```text
d
```

A full tree may contain on the order of:

```text
b^d
```

states.

This is why reducing the number of choices matters so much.

If you reduce average branching from:

```text
10
```

to:

```text
2
```

you can shrink the practical search enormously.

Backtracking performance is often mostly about search-tree size.

# 27. Worst-Case Complexity

Common families:

```text
subsets
→ O(2^n)

permutations
→ O(n!)

binary decision at n levels
→ O(2^n)

naive N-Queens
→ factorial/exponential-style search
```

These are often worst-case bounds.

Pruning may make real inputs much easier.

Also distinguish:

```text
find one solution
count solutions
generate all solutions
```

Generating all answers may inherently require enormous time because the output itself is enormous.

# 28. One Solution vs All Solutions vs Count

Find one:

```js
return true;
```

once a solution appears.

Find all:

```js
results.push(solution);
```

and continue searching.

Count only:

```js
count++;
```

without storing each solution.

If you only care whether a puzzle has a unique solution, you may stop after:

```text
count > 1
```

The search should match the actual question.

# 29. Minimum Remaining Values (MRV)

Suppose candidate sets are:

```text
A → {1,2,3,4}
B → {2}
C → {1,3}
D → {1,2,3}
```

Process:

```text
B
```

first because it is forced.

If the remaining ambiguous choices are:

```text
A → 4 choices
C → 2 choices
D → 3 choices
```

choose:

```text
C
```

This is Minimum Remaining Values:

> Choose the most constrained variable first.

It is also called the fail-first principle because impossible branches tend to expose themselves earlier.

# 30. Least-Constraining Value

MRV answers:

```text
Which variable should I choose next?
```

Least-constraining value asks:

```text
Which candidate should I try first?
```

The heuristic says:

> Try the candidate that removes the fewest options from neighboring variables.

This attempts to preserve flexibility for the remainder of the search.

# 31. Forward Checking

Suppose you choose:

```text
A = 4
```

Immediately update all variables constrained by A.

If one becomes:

```text
{}
```

then the branch is already impossible.

Backtrack now.

Do not wait until five decisions later.

This is forward checking.

# 32. Constraint Satisfaction Problems

Formally, a CSP has:

```text
variables
domains
constraints
```

N-Queens:

```text
variable:
queen position for each row

domain:
columns 0..N-1

constraints:
different columns
different diagonals
```

Sudoku:

```text
variables:
cells

domains:
1..9

constraints:
row, column, box uniqueness
```

Skyscrapers:

```text
variables:
cells or line permutations

domains:
1..N or allowed permutations

constraints:
row/column uniqueness
visibility clues
```

Backtracking is one of the foundational general techniques for solving CSPs.

# 33. Arc Consistency

More advanced CSP solvers propagate constraints farther than immediate assignments.

Suppose:

```text
A → {1,2}
B → {2}
```

and A must differ from B.

Since B can only be `2`, A cannot be `2`.

Therefore:

```text
A → {1}
```

Algorithms such as AC-3 enforce forms of arc consistency across constraint relationships.

The core idea remains:

```text
shrink domains before branching
```

# 34. Backtracking Invariants

An invariant is something that remains true throughout valid search states.

Permutation:

```text
current contains no repeated chosen element
```

N-Queens:

```text
all currently placed queens are mutually non-attacking
```

Sudoku:

```text
all currently filled cells obey Sudoku constraints
```

A strong solver tries never to recurse into a state that already violates its invariant.

That is why legality checks happen before recursion.

# 35. Soundness and Completeness

Two important correctness properties:

## Soundness

Every solution returned is actually valid.

Constraint checks give soundness.

## Completeness

Every valid solution can eventually be found.

Trying every legal choice gives completeness.

Pruning must preserve completeness.

If you prune a branch merely because it "looks bad," you may accidentally remove the real solution.

# 36. Why Backtracking Is Correct

At each state:

```text
every valid completed solution
must begin with one of the legal next choices
```

If the algorithm explores every legal choice, then whichever first choice belongs to a real solution will eventually be explored.

Then the same reasoning applies to the next decision.

Recursively, the algorithm cannot miss a valid solution unless:

```text
a legal choice was never generated
or
a valid branch was incorrectly pruned
```

# 37. Formal State-Space View

At a more formal level, define:

```text
S     = set of possible states
A(s)  = legal actions from state s
T(s,a)= state after taking action a
G     = goal states
```

Backtracking performs a depth-first exploration beginning at:

```text
s₀
```

For each:

```text
a ∈ A(s)
```

it explores:

```text
T(s,a)
```

and restores the representation of `s` after returning.

A pruning rule replaces:

```text
A(s)
```

with a smaller set:

```text
A'(s)
```

only when removed actions are proven unable to reach any goal state.

# 38. Recursion and the Call Stack

Backtracking maps naturally to recursion because the call stack remembers the branch history.

Conceptually:

```text
decision 1
  decision 2
    decision 3
      contradiction
    return to decision 2
    try another decision 3
```

The runtime stack remembers where to return after the deeper branch finishes.

That is why recursive backtracking can feel elegant once the recursive contract is clear.

# 39. Backtracking Without Recursion

Backtracking does not require recursion.

You can use your own explicit stack containing:

```text
current state
next choice to try
```

This can be useful when:

```text
search depth is huge
call-stack limits matter
you need resumable search
you need more explicit execution control
```

Recursive form is usually easier to learn first.

# 40. DFS Relationship

Depth-first search means:

```text
follow one branch deeply
before exploring sibling branches
```

Backtracking typically does exactly that.

A useful distinction:

```text
DFS
→ general graph/tree traversal strategy

backtracking
→ DFS-like search involving decisions,
   constraints,
   pruning,
   and usually undoing temporary state
```

# 41. Memoization

Backtracking and memoization can work together if different branches reach the same meaningful state.

Suppose the future depends only on:

```text
(index, remainingTarget)
```

If that pair has already been solved:

```text
reuse the result
```

But if state includes:

```text
large board arrangement
path-dependent visited sets
many unique assignments
```

there may be little overlap to cache.

Ask:

> Can two different paths arrive at the same future problem?

# 42. Dynamic Programming vs Backtracking

Backtracking:

```text
explores different choices
```

Dynamic programming:

```text
reuses results from repeated subproblems
```

Some problems naturally support both:

```text
search
+
memoization
```

Others have almost no repeated state, so memoization adds overhead without much benefit.

# 43. Branch and Bound

Backtracking usually asks:

```text
Is there a valid solution?
```

Branch-and-bound often asks:

```text
What is the best valid solution?
```

Track:

```text
best solution found so far
```

If a partial branch cannot possibly beat `best`, prune it.

Example:

```text
current cost = 100
best complete cost = 80

even perfect future choices
cannot reduce current cost

→ prune
```

This adds mathematical bounds to the search.

# 44. Symmetry Reduction

Some search spaces contain equivalent solutions.

N-Queens may produce boards related by:

```text
reflection
rotation
```

If you only care about fundamental solutions, exploring every symmetric version is redundant.

You may restrict the search to a canonical subset and reconstruct symmetry later.

This can substantially reduce work.

# 45. Duplicate Branches

Input:

```js
[1, 1, 2]
```

A naive permutation search may generate identical results multiple times.

A common technique:

```text
sort choices
skip equal sibling choices
```

The word sibling matters.

You are removing branches that are equivalent at the same decision level, not banning the value everywhere.

# 46. Bit Masks

Sets are easy to understand:

```js
usedColumns.has(col);
```

But small bounded domains can often be stored as bits.

Example:

```text
bit 0 → column 0 used?
bit 1 → column 1 used?
...
```

Benefits:

```text
fast membership checks
compact memory
cheap copying
fast union/intersection
```

High-performance N-Queens solvers often represent columns and diagonals using bit masks.

The search algorithm is still backtracking.

Only the state representation changes.

# 47. Copy State vs Mutate and Undo

## Copy state

```js
explore([...current, choice]);
```

Pros:

```text
easy to reason about
fewer restoration bugs
```

Cons:

```text
extra allocations
copying cost
garbage collection pressure
```

## Mutate and undo

```js
current.push(choice);
explore();
current.pop();
```

Pros:

```text
cheap
low allocation
often faster
```

Cons:

```text
easy to restore incorrectly
```

Both are valid styles.

# 48. Transaction Mental Model

A useful systems-style mental model is:

```text
BEGIN TEMPORARY CHANGE

apply choice
update all related state

explore

ROLL BACK
restore all related state
```

Example:

```js
usedCols.add(col);
diagA.add(row - col);
diagB.add(row + col);
board[row] = col;

explore(row + 1);

board[row] = -1;
diagB.delete(row + col);
diagA.delete(row - col);
usedCols.delete(col);
```

Every temporary mutation must have a matching restoration.

# 49. Common Bugs

## Forgetting to undo

Sibling branches inherit stale state.

## Undoing too much

You remove state belonging to an earlier level.

## Wrong base case

The recursion ends too early or never ends.

## Missing a legal choice

Valid solutions disappear.

## Bad pruning

The solver becomes fast but incorrect.

## Saving a mutable reference

This:

```js
results.push(current);
```

may store the same changing array repeatedly.

For a snapshot:

```js
results.push([...current]);
```

# 50. How to Debug Backtracking

Indent logs according to recursion depth.

Example trace:

```text
depth 0: []
choose 1

  depth 1: [1]
  choose 2

    depth 2: [1,2]
    choose 3

      solution: [1,2,3]

    undo 3

  undo 2

  choose 3
```

Helpful things to log:

```text
current state
choice being made
reason a choice is rejected
base case
undo operation
```

The goal is to make the invisible search tree visible.

# 51. Trace Before Code

Use tiny examples.

Permutation:

```text
[1,2,3]
```

N-Queens:

```text
4×4
```

Maze:

```text
3×3
```

Constraint puzzle:

```text
2×2 or 4×4
```

If you cannot explain one branch by hand, recursion will probably feel mysterious in code.

# 52. The Five Questions for Recursion

Before implementing, write:

```text
1. What does one recursive call mean?

2. What is one choice?

3. What state changes when I make it?

4. What is the base case?

5. What must be restored afterward?
```

N-Queens example:

```text
explore(row)
means:
rows before `row` are already valid;
find ways to place queens from `row` onward.

choice:
column for this row

state change:
mark column and diagonals

base case:
row === N

undo:
unmark column and diagonals
```

# 53. Give the Recursive Call a Contract

Do not think:

```text
"this function recursively solves the problem"
```

That is too vague.

Define it precisely.

For example:

```text
explore(row)
```

means:

> Assuming all rows before `row` are already validly filled, find valid completions for rows `row` through the end.

Now:

```js
explore(row + 1);
```

means:

> I solved this row. Solve the smaller remaining problem.

That is the conceptual heart of recursion.

# 54. Strong Puzzle-Solver Architecture

For Sudoku-like problems, a good development sequence is:

```text
Stage 1
Implement obvious deterministic rules

Stage 2
Repeat rules until no progress occurs

Stage 3
Track candidate sets

Stage 4
If one candidate remains, place it

Stage 5
If still stuck, choose an ambiguous candidate

Stage 6
Recurse

Stage 7
If contradiction occurs, undo and try another candidate
```

This mirrors human puzzle solving much more closely than blind guessing.

# 55. Separate the Pieces

A robust solver often separates:

```text
candidate generation
constraint checking
state mutation
constraint propagation
search
output formatting
```

For example:

```text
getCandidates(...)
isValid(...)
place(...)
remove(...)
propagate(...)
search(...)
```

That makes debugging much easier because each function has one conceptual job.

# 56. Pure Helpers + Mutable Search Core

Some operations are ideal pure functions:

```text
count visibility
calculate row candidates
check a clue
convert coordinates
```

The recursive core may deliberately mutate:

```text
place
recurse
remove
```

This is often a strong combination:

```text
pure constraint logic
+
small controlled mutable state
```

# 57. Explainable Search

For educational solvers, log reasoning:

```text
FORCED:
row 2 col 3 = 4

GUESS:
row 1 col 0 = 2

FAIL:
column 2 has no legal candidates

UNDO:
row 1 col 0 = 2

TRY:
row 1 col 0 = 3
```

This turns a solver from a black box into a reasoning tool.

# 58. Advanced: Exact Cover

Some puzzles can be transformed into an exact-cover problem.

Sudoku is a classic example.

Constraints become things such as:

```text
every cell gets exactly one digit
every row contains each digit once
every column contains each digit once
every box contains each digit once
```

Each possible placement satisfies a collection of these constraints.

Then Algorithm X searches for a set of rows that covers every constraint exactly once.

# 59. Dancing Links

Donald Knuth's Dancing Links technique is an efficient representation for Algorithm X.

Rows and columns can be removed and restored very cheaply.

That restore operation is another version of:

```text
choose
modify
explore
undo
```

Even a sophisticated exact-cover solver still contains the core backtracking idea.

# 60. Advanced: SAT Solving

Boolean satisfiability asks whether variables can be assigned:

```text
true
false
```

so that a formula becomes true.

A primitive search could:

```text
choose variable
try false
propagate
contradiction?
    backtrack
try true
```

Modern SAT solvers add powerful machinery such as:

```text
unit propagation
conflict analysis
clause learning
non-chronological backtracking
```

but the family resemblance is clear.

# 61. Non-Chronological Backtracking

Basic backtracking usually undoes the most recent decision.

Advanced solvers may discover that:

```text
the true cause of failure
came from a decision several levels earlier
```

Instead of backing up one level at a time, they jump directly to the responsible decision.

This is often called:

```text
backjumping
```

# 62. Learning From Failure

A failed branch may reveal:

```text
this combination of assignments can never work
```

An advanced solver can remember that information.

Later:

```text
same bad pattern begins to form
→ prune immediately
```

Constraint solvers may call such recorded conflicts:

```text
nogoods
```

Modern SAT solvers use related conflict-learning ideas extensively.

# 63. Search Ordering Can Matter More Than Micro-Optimization

Suppose one solver chooses a variable with:

```text
100 candidates
```

while another chooses one with:

```text
2 candidates
```

Both are logically correct.

But their search trees may differ by orders of magnitude.

Reducing:

```text
number of visited states
```

usually matters much more than shaving nanoseconds off a recursive function call.

# 64. Measure Search Nodes

Add:

```js
let nodesVisited = 0;
```

Increment once per recursive state.

Then compare strategies:

```text
first empty cell:
1,200,000 nodes

MRV:
4,300 nodes
```

Even before benchmarking runtime, node count tells you whether your search strategy is improving.

# 65. Low-Level Performance

After the search tree is under control, representation starts to matter.

Potential optimizations:

```text
arrays instead of object-heavy structures
bit masks instead of Sets
reuse mutable buffers
avoid copying entire boards
cheap undo operations
stable memory layout
```

These can improve:

```text
cache locality
allocation rate
garbage collection pressure
constant factors
```

But first reduce unnecessary search.

# 66. Recursion Depth

Recursive backtracking uses the call stack.

Depths such as:

```text
10
50
100
```

are often manageable depending on runtime.

Very deep search can overflow the stack.

JavaScript does not provide unlimited recursion depth.

For very deep problems:

```text
explicit stack
iterative search
different algorithm
```

may be safer.

# 67. Parallel Search

Top-level branches can sometimes be searched independently.

Example:

```text
queen 0 in column 0
queen 0 in column 1
queen 0 in column 2
...
```

Different workers could explore different branches.

Challenges include:

```text
load balancing
shared best bounds
communication overhead
duplicate work
determinism
```

Parallelism does not remove combinatorial complexity, but it can divide independent work.

# 68. Game Trees

Games also create decision trees:

```text
my move
opponent move
my move
opponent move
```

Minimax explores these trees.

Alpha-beta pruning removes branches that cannot change the final decision.

Different objective, same broad family:

```text
tree search
+
pruning
```

# 69. When to Think "Backtracking"

Signals include:

```text
find all arrangements
find one valid arrangement
permutations
combinations
fill a board
assign values under rules
partition something
place objects without conflicts
construct paths
schedule things
try possibilities and reject invalid ones
```

Especially when:

> One choice changes what choices remain available later.

# 70. When Backtracking May Be the Wrong Tool

Be cautious when:

```text
N is extremely large
a direct constructive formula exists
a greedy algorithm is provably optimal
the problem is a shortest-path problem
overlapping subproblems strongly suggest DP
```

Backtracking is powerful, but recognizing when **not** to search is equally important.

# 71. Beginner-to-Advanced Workflow

When solving a new problem:

```text
STEP 1
Describe a completed solution.

STEP 2
Identify one decision.

STEP 3
List possible choices.

STEP 4
Define what makes a choice illegal.

STEP 5
Define state.

STEP 6
Define how to apply and undo.

STEP 7
Write the base case.

STEP 8
Trace a tiny example.

STEP 9
Add safe pruning.

STEP 10
Propagate forced choices.

STEP 11
Improve variable ordering.

STEP 12
Only then optimize representation.
```

# 72. Worksheet

Before coding, fill this in:

```text
Problem:

Goal:

State:

One recursive call means:

One choice:

Possible choices:

Illegal choice if:

State changed by choice:

Undo operation:

Success base case:

Failure/pruning case:

Could choices be forced before branching?

Could duplicate branches exist?

Would MRV help?

Do I need:
- one solution?
- every solution?
- a count?
- the best solution?
```

# 73. Practice: Binary Strings

Generate all binary strings of length 4.

Before coding, answer:

```text
state?
choice?
base case?
undo?
number of leaves?
```

Then draw the first two levels of the search tree.

# 74. Practice: Permutations

Generate all permutations of:

```js
["A", "B", "C"]
```

Label each of these in your code:

```text
state
choice
validity
apply
recurse
undo
base case
```

# 75. Practice: Combinations

Find every 2-item combination of:

```js
[1,2,3,4]
```

Explain why:

```text
start index
```

prevents:

```text
[1,2]
[2,1]
```

from both being produced.

# 76. Practice: Maze

Solve a small maze.

Then answer:

```text
Should visited be global?
Or should it be removed during backtracking?

Does the answer change if I want one path vs all paths?
```

# 77. Practice: Target Sum

Given positive integers, find combinations that reach a target.

Add:

```text
if sum > target
    prune
```

Then explain why that pruning rule is safe only because future additions cannot decrease the sum.

# 78. Practice: Four Queens

Solve 4-Queens.

Track:

```text
used columns
row-col diagonals
row+col diagonals
```

Draw at least one branch that fails and show exactly which choice gets undone.

# 79. Practice: Mini Latin Square

Solve a 4×4 Latin square using:

```text
1,2,3,4
```

First use only deterministic candidate elimination.

Only introduce guessing when propagation stops making progress.

# 80. Practice: Skyscrapers

For every permutation of:

```text
1,2,3,4
```

calculate:

```text
left visibility
right visibility
```

Group permutations by clue pair.

Then reason about:

```text
row candidate sets
column candidate sets
crossing-cell agreement
```

Try to solve as much as possible through propagation before adding recursive guesses.

# 81. Practice: Sudoku

Implement in stages:

```text
1. row validity
2. column validity
3. box validity
4. candidate generation
5. deterministic propagation
6. first-empty-cell backtracking
7. MRV
```

Measure recursive node count before and after MRV.

# 82. Practice: Performance Study

Implement N-Queens twice:

```text
Version A:
Sets

Version B:
bit masks
```

Keep search ordering the same.

Compare:

```text
nodes visited
runtime
allocation behavior
```

This isolates the effect of state representation.

# 83. Final Mental Model

At age five:

```text
try a path
dead end?
go back
try another
```

At beginner-programmer level:

```text
choose
recurse
undo
```

At algorithm level:

```text
depth-first search
through a decision tree
with pruning
```

At constraint-solver level:

```text
variables
domains
constraints
propagation
branching
backtracking
```

At advanced level:

```text
engineer the search space:

reduce branching
detect contradictions early
choose constrained variables first
represent state cheaply
avoid equivalent work
learn from failures
use mathematical bounds
```

The recursion itself is rarely the hardest part.

The hard part is answering:

> What exactly is one choice, what information makes it legal, and what can I prove is impossible before I spend time exploring it?

Once those questions become explicit, backtracking stops looking like magic.

It becomes a disciplined way to explore possibilities without ever losing the ability to return to the last known-good state.

# 84. Review Questions

1. What does "backtracking" mean in plain language?
2. What are choose, explore, and undo?
3. What is state?
4. Why is undo necessary with mutable state?
5. How is backtracking different from pure brute force?
6. What is pruning?
7. Why must pruning be logically safe?
8. What does one recursive call represent?
9. What is a search tree?
10. What is branching factor?
11. What is search depth?
12. Why does candidate filtering matter?
13. What is constraint propagation?
14. What is MRV?
15. What is least-constraining value?
16. What is forward checking?
17. What is a CSP?
18. How is N-Queens a CSP?
19. How is Sudoku a CSP?
20. How can Skyscrapers be modeled as a CSP?
21. What is soundness?
22. What is completeness?
23. How can memoization interact with backtracking?
24. What does branch-and-bound add?
25. What is symmetry reduction?
26. Why might bit masks outperform Sets?
27. When is copying state easier than mutate/undo?
28. What is a nogood?
29. Why can two correct backtracking algorithms have radically different runtimes?
30. Why is reducing the number of visited states usually more important than micro-optimizing the recursive call?
