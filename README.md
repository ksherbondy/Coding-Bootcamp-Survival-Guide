# Coding Bootcamp Survival Guide

Plain-English field manuals for the coding concepts students are often expected to use before they fully understand them.

---

## What This Is

**Coding Bootcamp Survival Guide** is a collection of beginner-friendly Markdown guides designed to help new developers build the missing foundation behind common bootcamp assignments.

Many fast-paced coding programs introduce terms, tools, and projects quickly. Students may be given a glossary, a few links, and a project deadline, but not always enough step-by-step explanation to understand what is happening underneath.

This guide is meant to fill that gap.

It is not a replacement for any official curriculum. It is a companion resource students can read before, during, or after working on projects.

---

## Who This Is For

This guide is for:

- students new to coding
- bootcamp students who feel overwhelmed
- students who are tired of jumping between random videos
- people who need concepts explained in plain English
- anyone who wants stronger mental models before building projects
- instructors, mentors, and classmates who want a shareable support resource

You do not need to know everything before using these guides.

That is the point.

---

## Why This Exists

A lot of beginner coding material assumes students already understand things like:

- how the Internet works
- what the terminal is
- how Git and GitHub move code
- why project folders are structured a certain way
- what npm and `package.json` do
- how Express routes and middleware work
- how to read error messages

But many students are seeing these concepts for the first time.

When that foundation is missing, students often spend hours searching YouTube, copying commands, or bouncing between tutorials that all assume different starting points.

This project tries to give students one stable place to start.

---

## What Makes This Different

This is not a link dump.

Each guide is written as a standalone field manual with:

- plain-English explanations
- beginner-friendly mental models
- real code examples
- common mistakes and confusions
- practical workflows
- troubleshooting tips
- practice sections
- quick reference tables

The goal is not just to define terms.

The goal is to help students understand what they are doing and why it matters.

---

## Current Guides

### Foundations Pack

| # | Guide | Purpose |
|---|---|---|
| 01 | [How the Internet Works](./foundations/01-how-the-internet-works.md) | Explains browsers, servers, DNS, HTTP/HTTPS, URLs, APIs, and how websites load |
| 02 | [Terminal Basics](./foundations/02-terminal-basics.md) | Explains the command line, shell, navigation, file commands, pipes, permissions, and safe terminal habits |
| 03 | [Git and GitHub Basics](./foundations/03-git-and-github-basics.md) | Explains SSH, init, add, commit, push, pull, remotes, branches, merging, and conflicts |
| 04 | [Project Directory Structure](./foundations/04-project-directory-structure.md) | Explains how to organize project files and create a `structure.md` using the `tree` command |
| 05 | [Node, Express, Routing, and Middleware](./foundations/05-node-express-routing-and-middleware.md) | Explains Node, Express routes, middleware, `req`, `res`, JSON responses, and route order |
| 06 | [npm and package.json Basics](./foundations/06-npm-and-package-json-basics.md) | Explains Node vs npm, packages, dependencies, `node_modules`, scripts, and cloned project setup |
| 07 | [Debugging and Error Messages](./foundations/07-debugging-and-error-messages.md) | Explains how to read errors, debug calmly, use console logs, inspect browser/terminal errors, and ask for help |

> Note: Folder paths may be adjusted as the repository structure evolves.

---

## Suggested Reading Order

For brand-new students, the recommended order is:

```text
01. How the Internet Works
02. Terminal Basics
03. Git and GitHub Basics
04. Project Directory Structure
05. npm and package.json Basics
06. Node, Express, Routing, and Middleware
07. Debugging and Error Messages
```

The order can change depending on the module or project.

For example:

- working with GitHub first? Read Git and Terminal.
- starting a React app? Read Terminal, Project Structure, npm, and Debugging.
- building an Express server? Read npm, Express Routing, and Debugging.
- lost in errors? Read Debugging immediately.

---

## Recommended Repo Structure

This repository can be organized like this:

```text
Coding-Bootcamp-Survival-Guide/
├── README.md
├── foundations/
│   ├── 01-how-the-internet-works.md
│   ├── 02-terminal-basics.md
│   ├── 03-git-and-github-basics.md
│   ├── 04-project-directory-structure.md
│   ├── 05-node-express-routing-and-middleware.md
│   ├── 06-npm-and-package-json-basics.md
│   └── 07-debugging-and-error-messages.md
├── frontend/
│   └── README.md
├── backend/
│   └── README.md
├── project-workflow/
│   └── README.md
└── resources/
    └── README.md
```

Future sections may include:

```text
frontend/
├── modern-javascript-for-react.md
├── html-foundations.md
├── css-foundations.md
├── javascript-foundations.md
├── dom-basics.md
└── react-foundations.md

project-workflow/
├── how-to-start-any-coding-project.md
├── how-to-read-project-requirements.md
├── how-to-plan-a-react-project.md
└── how-to-write-a-good-readme.md
```

---

## How to Use This Guide

You can use these guides in a few different ways.

### 1. Read Before Starting a Project

Before starting a project, read the guide connected to the tools you will use.

Example:

```text
Project uses GitHub?
Read Git and GitHub Basics.

Project uses npm?
Read npm and package.json Basics.

Project uses Express?
Read Node, Express, Routing, and Middleware.
```

### 2. Use as a Troubleshooting Reference

When stuck, search the relevant guide for the term or error.

Examples:

```text
Cannot GET
missing script: dev
Permission denied publickey
node_modules
middleware
merge conflict
```

### 3. Use as a Study Path

If you are new, work through the guides in order and try the practice labs.

### 4. Use as a Mentor Tool

If you are helping another student, send them the section that explains the concept instead of only giving them the answer.

---

## Philosophy

This project is built around a simple belief:

> Beginners are not dumb. They are often missing the mental model.

A student can copy a command and still not understand what happened.

A student can finish a project and still not understand how the pieces fit together.

These guides are meant to help students connect the dots.

The goal is to move from:

```text
I copied the command, but I do not know what it did.
```

to:

```text
I understand what layer I am working in and why this command matters.
```

---

## Contribution Guidelines

Contributions are welcome if they support the purpose of the guide.

Good contributions should be:

- beginner-friendly
- practical
- clear
- accurate
- respectful
- free of unnecessary jargon
- focused on mental models, not just definitions

When adding or editing a guide, try to include:

- what the concept means
- why it matters
- where students will see it
- a real example
- common beginner mistakes
- a quick reference section
- practice or review questions when useful

Avoid:

- unexplained jargon
- link-only sections
- overly advanced details too early
- assuming the reader already knows the tool
- making beginners feel bad for not knowing something yet

---

## Suggested Guide Format

New guides should generally follow this pattern:

```md
# Topic Name

> Purpose statement

## Who This Guide Is For

## The Big Idea

## Why This Matters

## Core Concepts

## Real Code Examples

## Common Beginner Confusions

## Practice Lab

## Quick Reference

## Final Mental Model
```

The exact format can change depending on the topic, but the goal should stay the same:

```text
Explain the concept clearly enough that a beginner can use it.
```

---

## Roadmap

Possible future guides:

- Modern JavaScript Needed for React
- HTML Foundations
- CSS Foundations
- JavaScript Foundations
- DOM Basics
- React Foundations
- React Props, State, and Events
- Fetch, APIs, and JSON
- Forms and Controlled Inputs
- How to Start Any Coding Project
- How to Read Project Requirements
- How to Write a Good README
- How to Prepare for a Code Review
- Deployment Basics
- Environment Variables
- Testing Basics

---

## License and Attribution

If you use, adapt, or share this material, please keep attribution to the original repository.

Suggested attribution:

```text
Adapted from Coding Bootcamp Survival Guide.
```

A formal license can be added in a separate `LICENSE` file.

---

## Final Note

This guide was created to help students who are trying to learn fast, build real projects, and fill in the missing foundation along the way.

Coding is hard enough without feeling like everyone else received a secret instruction manual.

This is that missing manual.
