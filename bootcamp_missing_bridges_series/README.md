# Bootcamp Missing Bridges Series

## The concepts students are expected to use before anyone explains how the pieces connect

This series is designed as a companion track for the **Coding Bootcamp Survival Guide**.

The Problem-Solving Thinking Series asks:

```text
How do I figure out what kind of solution to try?
```

This series asks:

```text
How do all of these tools, runtimes, files, requests, states, tests, and systems actually connect?
```

Bootcamps often teach individual technologies in separate modules:

```text
Git
JavaScript
Node
Express
SQL
React
APIs
testing
deployment
```

The missing part is often the bridge between them.

Students learn how to call `fetch()`, but not necessarily what happens after the call leaves the browser. They learn `async/await`, but may not understand why asynchronous behavior exists. They learn React state, but may not have a durable model for ownership and data flow. They learn Git commands, but may not know what state their repository is actually in.

This series exists to make those hidden connections visible.

---

# Series Map

| # | Guide | Core Question |
|---|---|---|
| 01 | [How a Web Application Actually Works](./01-how-a-web-application-actually-works.md) | What happens from click to database and back? |
| 02 | [JavaScript Execution and Async Mental Models](./02-javascript-execution-and-async-mental-models.md) | What is JavaScript actually doing when code runs? |
| 03 | [Reading Other People's Code](./03-reading-other-peoples-code.md) | How do I enter an unfamiliar repository without reading everything? |
| 04 | [Debugging as a Process](./04-debugging-as-a-process.md) | How do I debug systematically instead of guessing? |
| 05 | [How to Read Documentation](./05-how-to-read-documentation.md) | How do I turn docs into working knowledge? |
| 06 | [Testing Mental Models](./06-testing-mental-models.md) | What exactly is a test proving? |
| 07 | [Git Beyond the Basics](./07-git-beyond-the-basics.md) | What state is my repository in and how do Git commands change it? |
| 08 | [Data Flow and State](./08-data-flow-and-state.md) | Where did this value come from, who owns it, and who can change it? |
| 09 | [What the Computer Is Actually Doing](./09-what-the-computer-is-actually-doing.md) | What is happening beneath the language abstraction? |
| 10 | [Security Foundations for Developers](./10-security-foundations-for-developers.md) | What assumptions should I never make about input, identity, or trust? |

---

# Suggested Reading Order

```text
web application flow
        ↓
JavaScript execution
        ↓
reading unfamiliar systems
        ↓
debugging
        ↓
documentation
        ↓
testing
        ↓
Git state
        ↓
application state
        ↓
computer internals
        ↓
security
```

Students can also jump directly to the guide connected to the problem they are facing.

---

# How to Use This Series

## Before a Module

```text
Starting React?
→ Data Flow and State

Starting APIs?
→ How a Web Application Actually Works

Starting async JavaScript?
→ JavaScript Execution and Async Mental Models
```

## While Stuck

Use the checklists in `worksheets/`.

Do not immediately search for a code answer. First ask what layer of the system you are actually working in.

## While Teaching

Each guide is structured to support discussion, tracing, examples, mental models, and reusable exercises.

---

# Core Philosophy

A student often does not need another syntax example.

They need the missing sentence:

```text
"Here is what is actually happening."
```

The goal of this series is to supply that sentence.
