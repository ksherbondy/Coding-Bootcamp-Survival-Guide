# npm and package.json Basics: A Beginner Guide for JavaScript Projects

> **Purpose:** This guide explains Node.js project setup, npm, `package.json`, dependencies, scripts, `node_modules`, and the commands beginners see when working with modern JavaScript projects.

---

## Table of Contents

1. [Who This Guide Is For](#who-this-guide-is-for)
2. [The Big Idea](#the-big-idea)
3. [Node.js vs. npm](#nodejs-vs-npm)
4. [Why npm Exists](#why-npm-exists)
5. [What Is a Package?](#what-is-a-package)
6. [What Is `package.json`?](#what-is-packagejson)
7. [Creating `package.json`](#creating-packagejson)
8. [What Is `node_modules/`?](#what-is-node_modules)
9. [What Is `package-lock.json`?](#what-is-package-lockjson)
10. [Dependencies vs. Dev Dependencies](#dependencies-vs-dev-dependencies)
11. [Installing Packages](#installing-packages)
12. [Removing Packages](#removing-packages)
13. [npm Scripts](#npm-scripts)
14. [`npm start` vs. `npm run dev` vs. `npm run build`](#npm-start-vs-npm-run-dev-vs-npm-run-build)
15. [Starting a Project Cloned from GitHub](#starting-a-project-cloned-from-github)
16. [Why You Do Not Commit `node_modules/`](#why-you-do-not-commit-node_modules)
17. [Common Beginner Confusions](#common-beginner-confusions)
18. [Common npm Errors](#common-npm-errors)
19. [Practice Lab](#practice-lab)
20. [Quick Reference](#quick-reference)
21. [Final Mental Model](#final-mental-model)

---

## Who This Guide Is For

This guide is for students who are learning modern JavaScript projects.

You may have already seen commands like:

```bash
npm install
npm start
npm run dev
npm run build
```

You may also have seen files and folders like:

```text
package.json
package-lock.json
node_modules/
```

At first, these can feel mysterious.

This guide explains what they are, why they exist, and how they fit together.

---

## The Big Idea

Modern JavaScript projects often use outside code written by other developers.

That outside code may include:

- Express
- React
- Vite
- Morgan
- Axios
- Jest
- ESLint
- Bootstrap
- many other tools and libraries

npm helps manage those packages.

A beginner-friendly summary:

```text
npm downloads project tools and libraries.
package.json describes what the project needs.
node_modules/ stores the downloaded packages.
package-lock.json records the exact installed versions.
```

---

## Node.js vs. npm

Node.js and npm are related, but they are not the same thing.

### Node.js

Node.js lets JavaScript run outside the browser.

You can use Node to:

- run JavaScript files from the terminal
- create servers
- build backend APIs
- run development tools
- automate tasks
- process files

Example:

```bash
node server.js
```

That command runs `server.js` using Node.

### npm

npm is a package manager.

It helps you:

- install packages
- remove packages
- run project scripts
- manage dependencies
- recreate project setup from `package.json`

Example:

```bash
npm install express
```

That command installs the Express package.

### Simple Summary

```text
Node.js = runs JavaScript outside the browser
npm = manages JavaScript packages and project commands
```

---

## Why npm Exists

Imagine you are building a project and need a web server.

You could write all the server logic yourself from scratch.

Or you could install Express:

```bash
npm install express
```

Now your project can use Express.

Without npm, developers would have to manually:

- find package files
- download them
- place them in the right folders
- track versions
- update packages
- share setup instructions with teammates

npm automates that work.

The core problem npm solves:

```text
How do we reliably install and manage outside code that our project depends on?
```

---

## What Is a Package?

A package is reusable code that can be installed into a project.

Examples:

| Package | What It Is Commonly Used For |
|---|---|
| `express` | Building Node servers |
| `morgan` | Logging server requests |
| `cors` | Handling cross-origin requests |
| `axios` | Making HTTP requests |
| `vite` | Running/building frontend projects |
| `react` | Building user interfaces |
| `jest` | Testing JavaScript code |
| `eslint` | Checking code quality |

Packages can be small or large.

Some packages are tools you run during development.

Some packages become part of your app.

---

## What Is `package.json`?

`package.json` is the project instruction card for an npm project.

It tells npm important information about the project.

A simple `package.json` might look like this:

```json
{
  "name": "sample-project",
  "version": "1.0.0",
  "type": "module",
  "scripts": {
    "start": "node server.js",
    "dev": "vite",
    "build": "vite build"
  },
  "dependencies": {
    "express": "^4.18.0"
  },
  "devDependencies": {
    "vite": "^5.0.0"
  }
}
```

### Common Fields

| Field | Meaning |
|---|---|
| `name` | Project/package name |
| `version` | Project version |
| `type` | Module style, often `"module"` for modern `import` syntax |
| `scripts` | Shortcut commands |
| `dependencies` | Packages needed by the app |
| `devDependencies` | Packages mainly needed during development |
| `main` | Entry file for some Node projects |
| `private` | Prevents accidental publishing when set to `true` |

### Beginner Mental Model

```text
package.json = project setup instructions
```

It answers:

```text
What packages does this project need?
What commands can I run?
How should this project behave?
```

---

## Creating `package.json`

To create a `package.json` file, run:

```bash
npm init
```

This asks a series of questions.

For a quick default setup, run:

```bash
npm init -y
```

The `-y` means:

```text
Yes, accept the default answers.
```

Example:

```bash
mkdir npm-practice
cd npm-practice
npm init -y
```

This creates:

```text
package.json
```

---

## What Is `node_modules/`?

`node_modules/` is the folder where npm stores installed packages.

If you run:

```bash
npm install express
```

npm downloads Express and the packages Express depends on.

Those files go into:

```text
node_modules/
```

This folder can get very large.

That is normal.

### Important

You usually do not edit files inside `node_modules/`.

They are downloaded package files.

If something is wrong, you usually fix your own code or change package versions rather than editing `node_modules/` directly.

### Beginner Mental Model

```text
node_modules/ = downloaded toolbox
```

Your project uses tools from the toolbox, but you do not hand-edit the tools themselves.

---

## What Is `package-lock.json`?

`package-lock.json` records the exact dependency versions npm installed.

If `package.json` says:

```json
"express": "^4.18.0"
```

That can allow compatible newer versions.

But `package-lock.json` records the exact version installed on your machine and the exact versions of related packages.

### Why It Matters

It helps teammates and deployment systems install the same dependency tree.

Beginner mental model:

```text
package.json = what the project asks for
package-lock.json = exact receipt of what npm installed
```

Usually, commit `package-lock.json` to GitHub.

Do not delete it casually.

---

## Dependencies vs. Dev Dependencies

There are two common dependency categories.

### Dependencies

Dependencies are packages your app needs to run.

Install with:

```bash
npm install package-name
```

Example:

```bash
npm install express
```

This adds the package under:

```json
"dependencies": {
  "express": "^4.18.0"
}
```

Examples of dependencies:

- Express for a server
- React for a React app
- Axios for HTTP requests
- Morgan for request logging

### Dev Dependencies

Dev dependencies are packages mainly needed while developing, testing, or building.

Install with:

```bash
npm install package-name --save-dev
```

or:

```bash
npm install -D package-name
```

Example:

```bash
npm install -D vite
```

This adds the package under:

```json
"devDependencies": {
  "vite": "^5.0.0"
}
```

Examples of dev dependencies:

- Vite
- Jest
- ESLint
- Prettier
- testing libraries

### Beginner Rule

```text
Needed when the app runs? dependencies.
Only needed to build, test, or develop? devDependencies.
```

The line can sometimes be blurry, especially for frontend build tools, but this rule is good enough for beginners.

---

## Installing Packages

### Install One Package

```bash
npm install express
```

Short version:

```bash
npm i express
```

### Install Multiple Packages

```bash
npm install express morgan cors
```

### Install a Dev Dependency

```bash
npm install -D vite
```

### Install Everything Listed in `package.json`

```bash
npm install
```

This is what you usually run after cloning a project from GitHub.

It reads `package.json` and `package-lock.json`, then rebuilds `node_modules/`.

---

## Removing Packages

To remove a package:

```bash
npm uninstall package-name
```

Example:

```bash
npm uninstall morgan
```

This removes it from:

- `node_modules/`
- `package.json`
- `package-lock.json`

Short version:

```bash
npm remove morgan
```

or:

```bash
npm rm morgan
```

---

## npm Scripts

npm scripts are shortcut commands stored in `package.json`.

Example:

```json
{
  "scripts": {
    "start": "node server.js",
    "dev": "vite",
    "build": "vite build",
    "test": "jest"
  }
}
```

You run them with npm.

### `start`

```bash
npm start
```

This runs:

```bash
node server.js
```

### `dev`

```bash
npm run dev
```

This runs:

```bash
vite
```

### `build`

```bash
npm run build
```

This runs:

```bash
vite build
```

### `test`

```bash
npm test
```

This runs:

```bash
jest
```

### Why Scripts Matter

Scripts let a project define its own commands.

Different projects may use different tools.

Instead of remembering the full command, you use the project shortcut.

Beginner mental model:

```text
npm scripts = project-specific shortcut buttons
```

---

## `npm start` vs. `npm run dev` vs. `npm run build`

These commands are often confused.

### `npm start`

Commonly used to start an app or server.

Example:

```json
"scripts": {
  "start": "node server.js"
}
```

Run:

```bash
npm start
```

Common meaning:

```text
Start the application.
```

### `npm run dev`

Commonly used to start a development server.

Example:

```json
"scripts": {
  "dev": "vite"
}
```

Run:

```bash
npm run dev
```

Common meaning:

```text
Start the app in development mode.
```

Development mode often includes helpful features like:

- auto reload
- error overlays
- faster rebuilds
- local server
- debugging information

### `npm run build`

Commonly used to create production-ready files.

Example:

```json
"scripts": {
  "build": "vite build"
}
```

Run:

```bash
npm run build
```

Common meaning:

```text
Create optimized files for deployment.
```

Build output often goes into a folder like:

```text
dist/
```

or:

```text
build/
```

### Important

These commands only work if they are defined in `package.json`.

If a project does not have a `dev` script, then this will fail:

```bash
npm run dev
```

Always check the `scripts` section.

---

## Starting a Project Cloned from GitHub

When you clone a JavaScript project from GitHub, it usually does not include `node_modules/`.

That is normal.

### Common Setup Flow

```bash
git clone git@github.com:username/project-name.git
cd project-name
npm install
npm run dev
```

or for a Node server:

```bash
git clone git@github.com:username/project-name.git
cd project-name
npm install
npm start
```

### What Each Step Does

| Command | Meaning |
|---|---|
| `git clone ...` | Download the project |
| `cd project-name` | Move into the project folder |
| `npm install` | Install needed packages |
| `npm run dev` | Start development server |
| `npm start` | Start app/server, if defined |

### Always Read the README

A good project should explain setup steps in `README.md`.

Look for sections like:

```text
Installation
Setup
Running the app
Development
Environment variables
```

---

## Why You Do Not Commit `node_modules/`

`node_modules/` is usually not committed to GitHub.

Reasons:

- it is huge
- it can contain thousands of files
- it can be recreated with `npm install`
- it makes Git slow and messy
- it creates unnecessary commits
- it may differ between systems

Instead, commit:

```text
package.json
package-lock.json
```

Then other people can run:

```bash
npm install
```

And npm rebuilds `node_modules/`.

### Add to `.gitignore`

Your `.gitignore` should usually include:

```text
node_modules/
```

Often also:

```text
.env
dist/
build/
.DS_Store
```

---

## Common Beginner Confusions

### Confusion 1: “I cloned the repo. Why is `node_modules/` missing?”

Because `node_modules/` is usually ignored by Git.

Run:

```bash
npm install
```

That rebuilds it.

### Confusion 2: “Why does `npm run dev` not work?”

Check `package.json`.

Look for:

```json
"scripts": {
  "dev": "..."
}
```

If there is no `dev` script, npm does not know what to run.

### Confusion 3: “What is the difference between `npm install` and `npm install express`?”

```bash
npm install
```

Installs all packages listed in `package.json`.

```bash
npm install express
```

Installs Express and adds it to the project.

### Confusion 4: “Should I edit `package-lock.json`?”

Usually no.

npm updates it automatically.

### Confusion 5: “Should I delete `package-lock.json`?”

Usually no.

It helps keep installs consistent.

### Confusion 6: “Why is `node_modules/` so huge?”

Packages can depend on other packages.

Those packages can depend on more packages.

This creates a dependency tree.

### Confusion 7: “Can I delete `node_modules/`?”

Yes, usually.

If things get weird, developers sometimes delete `node_modules/` and reinstall.

Common reset:

```bash
rm -rf node_modules
npm install
```

Be careful with `rm -rf`. Make sure you are in the project folder.

### Confusion 8: “Why does npm say command not found?”

Possible reasons:

- Node/npm is not installed
- terminal needs to be restarted
- PATH is not configured correctly
- command was misspelled

Check:

```bash
node --version
npm --version
```

### Confusion 9: “Why does the project run on localhost?”

Many npm scripts start a local development server.

Example:

```bash
npm run dev
```

Then the terminal may show:

```text
http://localhost:5173
```

That means your computer is temporarily serving the project.

### Confusion 10: “What is Vite?”

Vite is a modern frontend development/build tool.

It is commonly used with React projects.

In many projects:

```bash
npm run dev
```

starts Vite's development server.

```bash
npm run build
```

creates production-ready output.

---

## Common npm Errors

### Error: `missing script: dev`

Meaning:

```text
package.json does not have a dev script.
```

Check:

```json
"scripts": {
  "dev": "..."
}
```

Maybe the project uses:

```bash
npm start
```

instead.

### Error: `command not found: npm`

Meaning:

```text
npm is not installed or not available in your terminal.
```

Check whether Node.js is installed.

### Error: `Cannot find module`

Meaning:

```text
A package or file could not be found.
```

Try:

```bash
npm install
```

Also check spelling and import paths.

### Error: Port Already in Use

Example:

```text
EADDRINUSE: address already in use
```

Meaning:

```text
Another process is already using that port.
```

Solutions may include:

- stop the other server with `Ctrl + C`
- use a different port
- close the terminal running the old server

### Error: Permission Denied

Meaning:

```text
The system blocked the action due to permissions.
```

Avoid blindly using `sudo` with npm unless you understand why.

Permission problems may come from install location, system setup, or trying to modify protected folders.

---

## Practice Lab

This lab creates a tiny npm project.

### Step 1: Create a Folder

```bash
mkdir npm-practice
cd npm-practice
```

### Step 2: Create `package.json`

```bash
npm init -y
```

### Step 3: Create a JavaScript File

```bash
touch index.js
```

Add this to `index.js`:

```js
console.log("Hello from npm practice");
```

### Step 4: Add a Start Script

Open `package.json`.

Find:

```json
"scripts": {
  "test": "echo \"Error: no test specified\" && exit 1"
}
```

Replace it with:

```json
"scripts": {
  "start": "node index.js"
}
```

### Step 5: Run the Script

```bash
npm start
```

You should see:

```text
Hello from npm practice
```

### Step 6: Install a Package

Install Morgan as practice:

```bash
npm install morgan
```

Now check:

```bash
ls
```

You should see:

```text
node_modules/
package.json
package-lock.json
index.js
```

### Step 7: Inspect `package.json`

You should now see `morgan` listed under dependencies.

### Step 8: Add `.gitignore`

```bash
touch .gitignore
```

Add:

```text
node_modules/
```

### Step 9: Rebuild Test

Delete `node_modules/` carefully:

```bash
rm -rf node_modules
```

Then reinstall:

```bash
npm install
```

This proves `node_modules/` can be rebuilt from `package.json` and `package-lock.json`.

---

## Quick Reference

| Command | Meaning |
|---|---|
| `node --version` | Check Node version |
| `npm --version` | Check npm version |
| `npm init` | Create `package.json` with prompts |
| `npm init -y` | Create `package.json` with defaults |
| `npm install` | Install all project dependencies |
| `npm install package-name` | Install a package |
| `npm i package-name` | Short version of install |
| `npm install -D package-name` | Install dev dependency |
| `npm uninstall package-name` | Remove a package |
| `npm start` | Run the `start` script |
| `npm run dev` | Run the `dev` script |
| `npm run build` | Run the `build` script |
| `npm test` | Run the `test` script |
| `npm run` | List available scripts |

---

## Important Files and Folders

| Name | Meaning |
|---|---|
| `package.json` | Project instructions, scripts, dependency list |
| `package-lock.json` | Exact dependency install record |
| `node_modules/` | Downloaded packages |
| `.gitignore` | Tells Git what not to track |
| `dist/` | Common production build output |
| `build/` | Common production build output |

---

## Common Setup Flows

### New npm Project

```bash
mkdir my-project
cd my-project
npm init -y
```

### Install Express

```bash
npm install express
```

### Install Vite as a Dev Dependency

```bash
npm install -D vite
```

### Start a Cloned Project

```bash
git clone git@github.com:username/project-name.git
cd project-name
npm install
npm run dev
```

### Start a Node Server Project

```bash
npm install
npm start
```

### Check Available Scripts

```bash
npm run
```

---

## Final Mental Model

For beginner JavaScript projects, keep this picture in your head:

```text
package.json
   ↓
Lists scripts and packages the project needs
   ↓
npm install
   ↓
Downloads packages into node_modules/
   ↓
package-lock.json
   ↓
Records the exact install details
   ↓
npm run dev / npm start / npm run build
   ↓
Runs project commands defined in package.json
```

Or even shorter:

```text
package.json = instruction card
npm install = gather the tools
node_modules/ = toolbox
package-lock.json = exact receipt
npm scripts = shortcut buttons
```

When you clone a project and feel lost, check this order:

```text
1. Am I inside the project folder?
2. Is there a package.json?
3. Did I run npm install?
4. What scripts are listed in package.json?
5. Should I run npm start or npm run dev?
```

That checklist solves many beginner npm problems.
