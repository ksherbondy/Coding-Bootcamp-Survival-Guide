# Coding Bootcamp Survival Guide

Plain-English field manuals, lesson series, visual aids, and worked examples for students who are learning to code in fast-moving bootcamp environments.

---

## What This Is

**Coding Bootcamp Survival Guide** is a curated learning repo designed to help students build the missing foundation behind common bootcamp assignments, projects, and technical discussions.

Many coding programs move quickly. Students are often expected to use tools, terms, and patterns before they fully understand them. This repo exists to slow that down just enough to make the material understandable.

This is not meant to replace an official curriculum.

It is meant to be a **companion resource** students can use:

- before starting a module
- during a project
- when confused by terminology
- while debugging
- when preparing to teach or help someone else
- when trying to build stronger mental models

---

## Who This Is For

This repo is for:

- brand-new coding students
- bootcamp students who feel overwhelmed
- self-taught developers filling in gaps
- students switching between languages and ecosystems
- mentors and instructors who want shareable support materials
- anyone who wants clearer explanations before jumping into implementation

You do **not** need to know everything before using these guides.

That is the point.

---

## What Makes This Different

This is not just a glossary, and it is not just a collection of syntax notes.

This repo focuses on:

- plain-English explanations
- beginner-friendly mental models
- practical examples
- worked JavaScript examples
- visual aids and infographics
- debugging and troubleshooting support
- problem-solving thought processes
- guide material that explains **why** something works, not just **what** to type

A major goal of this repo is to teach the layer that often gets skipped:

```text
problem statement
        ↓
what words matter?
        ↓
what does that suggest?
        ↓
what data structure or pattern should I consider?
        ↓
what is the simplest correct solution?
        ↓
how do I improve it?
```

---

## Repository Structure

Current structure:

```text
Coding-Bootcamp-Survival-Guide/
├── README.md
├── React/
│   ├── React_From_Cradle_to_Grave.md
│   └── images/
├── SQL/
│   ├── 00_sql_command_toolbox.md
│   ├── 01_database_foundations_lesson.md
│   ├── 02_dbms_and_file_systems.md
│   ├── 03_relational_model.md
│   ├── 04_entities_attributes_relationships.md
│   ├── 05_erds_and_cardinality.md
│   ├── 06_er_model_to_tables.md
│   ├── 07_keys_primary_foreign_composite.md
│   ├── 08_sql_select_basics.md
│   ├── 09_sql_formatting_style (1).md
│   ├── 10_normalization_1nf_2nf_3nf.md
│   └── sql_is_the_umbrella.md
├── foundations/
│   ├── 01-how-the-internet-works.md
│   ├── 02-terminal-basics.md
│   ├── 03-git-and-github-basics.md
│   ├── 04-project-directory-structure.md
│   ├── 05-node-express-routing-and-middleware.md
│   ├── 06-npm-and-package-json-basics.md
│   ├── 07-debugging-and-error-messages.md
│   ├── 08-arrays-and-multidimensional-data.md
│   ├── 09-for-loops-and-nested-loops.md
│   ├── 10-javascript_bitwise_operators_deep_dive.md
│   └── 11-javascript-types-instructor-guide.md
├── problem_solving_thinking_series/
│   ├── 01-reading-problems-like-a-programmer.md
│   ├── ...
│   ├── 21-putting-it-all-together.md
│   ├── examples/
│   ├── images/
│   └── worksheets/
├── visual-aides/
│   ├── Data Types For Popular Languages.png
│   └── cross-language-data-structures.png
└── structure.md
```
Additional External Learning Repo https://github.com/ksherbondy/dom-forum-practice
---

## Current Content

This repo currently includes four major learning areas:

1. **Foundations**
2. **SQL**
3. **React**
4. **Problem-Solving Thinking Series**

It also includes **visual aids / infographics** and a repository **structure.md** reference.

---

## Foundations Guides

These guides focus on the core concepts many students need before they can comfortably work with projects.

| # | Guide | Purpose |
|---|---|---|
| 01 | [How the Internet Works](./foundations/01-how-the-internet-works.md) | Browsers, servers, DNS, HTTP/HTTPS, URLs, APIs, and how requests flow |
| 02 | [Terminal Basics](./foundations/02-terminal-basics.md) | Shell basics, navigation, file operations, commands, and safe terminal habits |
| 03 | [Git and GitHub Basics](./foundations/03-git-and-github-basics.md) | Repositories, commits, branches, remotes, pushes, pulls, merges, and conflicts |
| 04 | [Project Directory Structure](./foundations/04-project-directory-structure.md) | How project folders are organized and how to document them with `tree` and `structure.md` |
| 05 | [Node, Express, Routing, and Middleware](./foundations/05-node-express-routing-and-middleware.md) | Node/Express flow, routes, middleware, `req` / `res`, JSON responses, and route order |
| 06 | [npm and package.json Basics](./foundations/06-npm-and-package-json-basics.md) | Packages, dependencies, scripts, `node_modules`, and cloned project setup |
| 07 | [Debugging and Error Messages](./foundations/07-debugging-and-error-messages.md) | Reading errors, debugging calmly, using logs, inspecting browser and terminal issues |
| 08 | [Arrays and Multidimensional Data](./foundations/08-arrays-and-multidimensional-data.md) | 1D and 2D arrays, nested data, and how to reason about structure |
| 09 | [For Loops and Nested Loops](./foundations/09-for-loops-and-nested-loops.md) | Traversal, indexing, loop control, and when nested loops are appropriate |
| 10 | [JavaScript Bitwise Operators Deep Dive](./foundations/10-javascript_bitwise_operators_deep_dive.md) | Binary thinking, bitwise operators, shifts, masks, and practical intuition |
| 11 | [JavaScript Types Instructor Guide](./foundations/11-javascript-types-instructor-guide.md) | Primitive types, objects, equality, mutability, coercion, `NaN`, and common type misconceptions |

---

## SQL Section

The SQL section builds from database foundations through relational thinking and SQL query basics.

| # | Guide | Purpose |
|---|---|---|
| 00 | [SQL Command Toolbox](./SQL/00_sql_command_toolbox.md) | Quick command reference and SQL utility guide |
| 01 | [Database Foundations Lesson](./SQL/01_database_foundations_lesson.md) | Core database concepts and terminology |
| 02 | [DBMS and File Systems](./SQL/02_dbms_and_file_systems.md) | How databases differ from file-based storage |
| 03 | [Relational Model](./SQL/03_relational_model.md) | Rows, columns, tables, and relational structure |
| 04 | [Entities, Attributes, Relationships](./SQL/04_entities_attributes_relationships.md) | Conceptual database modeling basics |
| 05 | [ERDs and Cardinality](./SQL/05_erds_and_cardinality.md) | Entity relationship diagrams and relationship rules |
| 06 | [ER Model to Tables](./SQL/06_er_model_to_tables.md) | Translating conceptual models into actual tables |
| 07 | [Keys: Primary, Foreign, Composite](./SQL/07_keys_primary_foreign_composite.md) | Table identity and relationships |
| 08 | [SQL SELECT Basics](./SQL/08_sql_select_basics.md) | Reading data with `SELECT`, filtering, and basic querying |
| 09 | [SQL Formatting Style](./SQL/09_sql_formatting_style%20%281%29.md) | Readable SQL formatting conventions |
| 10 | [Normalization: 1NF, 2NF, 3NF](./SQL/10_normalization_1nf_2nf_3nf.md) | Structuring relational data well |
| — | [SQL Is the Umbrella](./SQL/sql_is_the_umbrella.md) | Big-picture framing for how SQL concepts relate |

---

## React Section

The React section currently contains a major overview guide plus supporting visuals.

| Guide | Purpose |
|---|---|
| [React From Cradle to Grave](./React/React_From_Cradle_to_Grave.md) | A broader React learning guide that helps students understand the full mental model of React development |

### React Visuals

Located in `React/images/`, including:

- `big-picture.png`
- `component-lifecycle.png`
- `components.png`
- `lists-keys.png`
- `react_overview.png`
- `recap-mental-model.png`
- `rendering.png`
- `state-hooks.png`

These visuals help students connect terminology to the larger React model.

---

## Problem-Solving Thinking Series

This is a full lesson series focused on a topic that is often under-taught:

> **How do you figure out how to solve a problem in the first place?**

Instead of only teaching syntax or named algorithms, this series teaches the reasoning layer between reading a prompt and writing working code.

### Core Focus

The series helps students learn how to:

- read problem statements like a programmer
- spot key words and constraints
- connect language to data structures and patterns
- ask what information must be remembered
- choose a representation
- write a simple correct version first
- recognize repeated work
- find edge cases
- reason about correctness
- compare multiple valid solutions

### Lesson Roadmap

| # | Lesson |
|---|---|
| 01 | [Reading Problems Like a Programmer](./problem_solving_thinking_series/01-reading-problems-like-a-programmer.md) |
| 02 | [Problem Decomposition](./problem_solving_thinking_series/02-problem-decomposition.md) |
| 03 | [What Must I Remember?](./problem_solving_thinking_series/03-what-must-i-remember.md) |
| 04 | [Data Structure Decision Guide](./problem_solving_thinking_series/04-data-structure-decision-guide.md) |
| 05 | [Algorithm Selection Checklist](./problem_solving_thinking_series/05-algorithm-selection-checklist.md) |
| 06 | [Brute Force First](./problem_solving_thinking_series/06-brute-force-first.md) |
| 07 | [Recognizing Repeated Work](./problem_solving_thinking_series/07-recognizing-repeated-work.md) |
| 08 | [Representation Is Part of the Solution](./problem_solving_thinking_series/08-representation-is-part-of-the-solution.md) |
| 09 | [Loop and Traversal Selection](./problem_solving_thinking_series/09-loop-and-traversal-selection.md) |
| 10 | [English Into Boolean Logic](./problem_solving_thinking_series/10-english-into-boolean-logic.md) |
| 11 | [Boundary Thinking and Off-by-One Errors](./problem_solving_thinking_series/11-boundary-thinking.md) |
| 12 | [Edge-Case Discovery](./problem_solving_thinking_series/12-edge-case-discovery.md) |
| 13 | [Trace Before You Code](./problem_solving_thinking_series/13-trace-before-you-code.md) |
| 14 | [Invariants: What Must Always Be True?](./problem_solving_thinking_series/14-invariants.md) |
| 15 | [State Machines and Legal Transitions](./problem_solving_thinking_series/15-state-machines.md) |
| 16 | [Make Invalid States Hard to Represent](./problem_solving_thinking_series/16-invalid-states.md) |
| 17 | [Time Complexity From Code Shape](./problem_solving_thinking_series/17-time-complexity-from-code-shape.md) |
| 18 | [Space Complexity and Tradeoffs](./problem_solving_thinking_series/18-space-complexity-and-tradeoffs.md) |
| 19 | [Common Problem Families](./problem_solving_thinking_series/19-common-problem-families.md) |
| 20 | [Why This Solution? Comparative Reasoning](./problem_solving_thinking_series/20-why-this-solution.md) |
| 21 | [Putting It All Together](./problem_solving_thinking_series/21-putting-it-all-together.md) |

### Additional Series Resources

The series also includes:

- [Series README](./problem_solving_thinking_series/README.md)
- [Problem Pattern Drills](./problem_solving_thinking_series/examples/problem-pattern-drills.md)
- Worksheets:
  - [Problem-Solving Worksheet](./problem_solving_thinking_series/worksheets/problem-solving-worksheet.md)
  - [Algorithm Selection Checklist](./problem_solving_thinking_series/worksheets/algorithm-selection-checklist.md)
  - [Trace Table](./problem_solving_thinking_series/worksheets/trace-table.md)
  - [Edge-Case Checklist](./problem_solving_thinking_series/worksheets/edge-case-checklist.md)
  - [Solution Comparison Worksheet](./problem_solving_thinking_series/worksheets/solution-comparison-worksheet.md)
- Visuals in `problem_solving_thinking_series/images/`

---

## Visual Aids

The repo also includes quick visual references for cross-language terminology and common structures.

Located in `visual-aides/`:

- `Data Types For Popular Languages.png`
- `cross-language-data-structures.png`

These are especially helpful when students hear terms like:

```text
hash map
dictionary
map
object
array
list
vector
set
```

in different languages and want a quick translation guide.

---

## structure.md

This repo includes a `structure.md` file that documents the repository layout.

This is useful for:

- keeping the repo organized
- showing students how a documented project structure looks
- demonstrating use of the `tree` command
- helping future contributors navigate the content

---

## Suggested Reading Paths

Students do not need to read everything in order.

Here are some practical entry points.

### If You Are Brand New

Start with:

```text
foundations/
01 → 07 first
```

Then continue into:

```text
08 arrays
09 loops
11 JavaScript types
10 bitwise operators
```

depending on current coursework.

### If You Are Struggling With Projects

Focus on:

- Project Directory Structure
- npm and package.json Basics
- Debugging and Error Messages
- Node, Express, Routing, and Middleware
- React From Cradle to Grave

### If You Are Learning Databases

Start with the SQL section in order:

```text
00 or 01 through 10
```

Then use `sql_is_the_umbrella.md` as a big-picture reference.

### If You Are Struggling With Coding Challenges

Start with the **Problem-Solving Thinking Series**.

A great path is:

```text
01
02
03
04
05
06
07
11
12
13
19
20
21
```

### If You Are Teaching or Mentoring

Use:

- the worksheets
- the visual aids
- the problem-solving lesson series
- the React visuals
- the foundations guides as assignment support

---

## How to Use This Repo

You can use this repo in several ways.

### 1. As a Learning Path

Read a section in sequence when you want structured support.

### 2. As a Just-in-Time Reference

Jump directly to the guide connected to the tool or concept you are using.

Examples:

```text
Need Git help?
Read Git and GitHub Basics.

Need SQL help?
Open the SQL section.

Stuck on a coding challenge?
Use the Problem-Solving Thinking Series.

Need to understand React better?
Read React From Cradle to Grave.
```

### 3. As a Mentor / Instructor Resource

Instead of only telling students the answer, send them the lesson, worksheet, or visual that helps them build the missing model.

### 4. As a Discussion Guide

Many of the materials can be used for:

- breakout sessions
- forum discussions
- tutoring
- whiteboard practice
- self-study prompts

---

## Philosophy

This repo is built around a simple belief:

> Beginners are not dumb. They are often missing the mental model.

A student can copy commands and still not know what happened.

A student can solve a challenge and still not know why the solution shape made sense.

A student can memorize syntax and still freeze when looking at a new problem statement.

This repo is meant to help bridge that gap.

The goal is to move students from:

```text
I copied it, but I don't understand it.
```

to:

```text
I understand what problem I am solving,
what tool I am using,
and why this approach makes sense.
```

---

## Contribution Guidelines

Contributions are welcome if they support the purpose of the repo.

Good contributions should be:

- accurate
- beginner-friendly
- practical
- respectful
- clear
- concept-first
- strong on mental models
- free of unnecessary jargon

When creating or editing content, try to include:

- what the concept means
- why it matters
- where students encounter it
- a concrete example
- common beginner confusions
- helpful mental models
- practice or review material when useful

Avoid:

- unexplained jargon
- link-only sections
- skipping reasoning steps
- assuming the reader already knows the tool
- making beginners feel bad for not knowing something yet

---

## Suggested Guide Format

New guides do not have to be identical, but this structure works well:

```md
# Topic Name

> Purpose statement

## Who This Guide Is For

## The Big Idea

## Why This Matters

## Core Concepts

## Real Examples

## Common Beginner Confusions

## Practice / Reflection

## Quick Reference

## Final Mental Model
```

For lesson-series style material, it is also useful to include:

- learning objectives
- worksheets
- reflection questions
- staged implementation guidance
- compare/contrast exercises

---

## Future Expansion Ideas

Possible future additions could include:

- HTML Foundations
- CSS Foundations
- Modern JavaScript Needed for React
- DOM Basics
- Fetch, APIs, and JSON
- Forms and Controlled Inputs
- Environment Variables
- Testing Basics
- Deployment Basics
- How to Read Project Requirements
- How to Start Any Coding Project
- How to Write a Good README
- How to Prepare for a Code Review
- State and Events in Frontend Apps
- Common Backend Patterns
- API Design Basics

---

## License and Attribution

If you use, adapt, or share this material, please keep attribution to the original repository.

Suggested attribution:

```text
Adapted from Coding Bootcamp Survival Guide.
```

A formal license can be added in a separate `LICENSE` file if desired.

---

## Final Note

This repo was created to help students who are trying to learn quickly, build real projects, and fill in the missing foundation along the way.

Coding is hard enough without feeling like everyone else got the secret instruction manual first.

This repo is meant to be part of that missing manual.
