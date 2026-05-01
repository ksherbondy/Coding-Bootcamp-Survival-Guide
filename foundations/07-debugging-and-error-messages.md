# Debugging and Error Messages: A Beginner Guide for New Developers

> **Purpose:** This guide teaches beginners how to read errors, troubleshoot problems, and debug code without panicking.  
> It covers terminal errors, browser console errors, HTTP status codes, Git errors, npm errors, import/path problems, and basic debugging habits.

---

## Table of Contents

1. [Who This Guide Is For](#who-this-guide-is-for)
2. [The Big Idea](#the-big-idea)
3. [Errors Are Clues, Not Insults](#errors-are-clues-not-insults)
4. [The Debugging Mindset](#the-debugging-mindset)
5. [The First Question: What Changed?](#the-first-question-what-changed)
6. [Read the Error Message Slowly](#read-the-error-message-slowly)
7. [Common Places Errors Appear](#common-places-errors-appear)
8. [Browser Console Errors](#browser-console-errors)
9. [Terminal Errors](#terminal-errors)
10. [JavaScript Error Types](#javascript-error-types)
11. [HTTP Status Codes](#http-status-codes)
12. [Path and File Name Problems](#path-and-file-name-problems)
13. [Import and Export Problems](#import-and-export-problems)
14. [npm Errors](#npm-errors)
15. [Git Errors](#git-errors)
16. [Express and Server Errors](#express-and-server-errors)
17. [React Errors](#react-errors)
18. [Using `console.log()`](#using-consolelog)
19. [Commenting Out Code](#commenting-out-code)
20. [Rubber Duck Debugging](#rubber-duck-debugging)
21. [How to Search for an Error](#how-to-search-for-an-error)
22. [How to Ask for Help](#how-to-ask-for-help)
23. [Common Beginner Confusions](#common-beginner-confusions)
24. [Debugging Checklist](#debugging-checklist)
25. [Practice Lab](#practice-lab)
26. [Quick Reference](#quick-reference)
27. [Final Mental Model](#final-mental-model)

---

## Who This Guide Is For

This guide is for students who are learning to code and feel overwhelmed by error messages.

Every developer sees errors.

Beginners see errors.

Senior developers see errors.

Professional developers see errors every day.

The difference is not that experienced developers avoid errors.

The difference is that experienced developers know how to read errors and narrow down the problem.

Debugging is not a sign that you are bad at coding.

Debugging is part of coding.

---

## The Big Idea

Debugging means finding and fixing problems in your code.

A simple debugging flow looks like this:

```text
Something is wrong
   ↓
Find where the problem appears
   ↓
Read the error carefully
   ↓
Form a small guess
   ↓
Test one change
   ↓
Check the result
   ↓
Repeat until fixed
```

Debugging is not random guessing.

Good debugging is a process of narrowing things down.

---

## Errors Are Clues, Not Insults

When beginners see red text, they often feel like the computer is yelling at them.

That is understandable.

But error messages are not insults.

They are clues.

An error message is the computer saying:

```text
I tried to do what you asked, but something prevented me from finishing.
Here is what I know.
```

The error might tell you:

- what kind of problem happened
- which file caused the issue
- which line number is involved
- what value was unexpected
- what command failed
- what dependency is missing
- what route could not be found

Your job is to slow down and read the clue.

---

## The Debugging Mindset

Good debugging requires patience.

The goal is not to immediately know the answer.

The goal is to reduce confusion step by step.

Helpful mindset:

```text
I do not need to know everything.
I need to find the next useful clue.
```

When debugging, avoid changing ten things at once.

If you change too many things, you may not know which change fixed or broke the code.

Better:

```text
Change one thing.
Test.
Observe.
Repeat.
```

---

## The First Question: What Changed?

When something breaks, ask:

```text
What changed since the last time this worked?
```

Common answers:

- You renamed a file.
- You moved a file.
- You changed an import path.
- You installed a package.
- You deleted `node_modules/`.
- You changed a route.
- You changed a function name.
- You changed state in React.
- You changed CSS class names.
- You pulled new code from GitHub.
- You switched branches.
- You edited `package.json`.

This question is powerful because bugs often appear near recent changes.

---

## Read the Error Message Slowly

Do not try to understand the entire error at once.

Look for useful pieces.

Example error:

```text
ReferenceError: data is not defined
    at server.js:12:25
```

Break it down:

| Piece | Meaning |
|---|---|
| `ReferenceError` | JavaScript looked for something that does not exist in scope |
| `data is not defined` | The variable `data` was used but not created or imported |
| `server.js` | The problem is in this file |
| `12:25` | Line 12, column 25 |

This gives you a starting point.

Go to `server.js`, line 12, and look for `data`.

---

## Common Places Errors Appear

Errors may appear in different places depending on what you are building.

### Browser Page

The visible web page may show:

```text
Cannot GET /route
404 Not Found
Application error
Blank screen
```

### Browser Console

The console may show JavaScript errors, failed network requests, or warnings.

Open browser DevTools.

Common shortcut:

```text
Right click page → Inspect → Console
```

Or:

```text
F12
```

On some laptops you may need:

```text
Fn + F12
```

### Terminal

The terminal may show errors from:

- npm
- Node
- Express
- React/Vite
- Git
- tests
- build tools

### Editor

Your code editor may show:

- red squiggly lines
- lint errors
- TypeScript errors
- missing import warnings
- formatting warnings

### Network Tab

The browser Network tab can show:

- failed API requests
- 404 responses
- 500 responses
- incorrect URLs
- blocked CORS requests
- missing files

---

## Browser Console Errors

The browser console is one of the most important debugging tools for frontend developers.

Common browser console errors include:

```text
Uncaught ReferenceError
Uncaught TypeError
SyntaxError
Failed to fetch
404 Not Found
CORS policy error
```

### Example: ReferenceError

```text
Uncaught ReferenceError: userName is not defined
```

Meaning:

```text
The code tried to use userName, but JavaScript does not know what userName is.
```

Possible causes:

- variable was never created
- variable name is misspelled
- variable is outside the current scope
- import is missing

### Example: TypeError

```text
Uncaught TypeError: Cannot read properties of undefined
```

Meaning:

```text
The code tried to access something on a value that is undefined.
```

Example:

```js
console.log(user.name);
```

If `user` is undefined, then `user.name` fails.

### Example: Failed to Fetch

```text
TypeError: Failed to fetch
```

Possible causes:

- server is not running
- wrong URL
- wrong port
- CORS issue
- network issue
- backend route does not exist

---

## Terminal Errors

Terminal errors appear when running commands.

Examples:

```bash
npm run dev
npm start
node server.js
git push
```

### Example: Command Not Found

```text
npm: command not found
```

Meaning:

```text
The terminal does not recognize npm.
```

Possible causes:

- Node/npm is not installed
- terminal PATH issue
- typo in the command

Check:

```bash
node --version
npm --version
```

### Example: Cannot Find Module

```text
Error: Cannot find module 'express'
```

Meaning:

```text
Your code tried to use Express, but Express is not installed or cannot be found.
```

Try:

```bash
npm install
```

Or:

```bash
npm install express
```

### Example: Port Already in Use

```text
EADDRINUSE: address already in use :::3000
```

Meaning:

```text
Something is already running on port 3000.
```

Possible fixes:

- stop the old server with `Ctrl + C`
- close the terminal running it
- use a different port

---

## JavaScript Error Types

JavaScript errors often name the type of problem.

### SyntaxError

A `SyntaxError` means the code is not written in valid JavaScript syntax.

Example:

```js
if (isLoggedIn {
  console.log("Welcome");
}
```

Problem:

```text
Missing closing parenthesis.
```

Fixed:

```js
if (isLoggedIn) {
  console.log("Welcome");
}
```

Common causes:

- missing parenthesis
- missing bracket
- missing curly brace
- missing quote
- extra comma
- invalid import/export syntax

### ReferenceError

A `ReferenceError` means JavaScript cannot find a variable or function name.

Example:

```js
console.log(totalPrice);
```

If `totalPrice` was never created, you may see:

```text
ReferenceError: totalPrice is not defined
```

Common causes:

- typo
- missing declaration
- missing import
- wrong scope

### TypeError

A `TypeError` means a value is not the type or shape your code expected.

Example:

```js
const user = undefined;
console.log(user.name);
```

Error:

```text
TypeError: Cannot read properties of undefined
```

Common causes:

- trying to use `.map()` on something that is not an array
- trying to access a property on `undefined`
- calling something as a function when it is not a function
- expecting data before it has loaded

### RangeError

A `RangeError` means a value is outside an allowed range.

Example causes:

- infinite recursion
- invalid array length
- number outside expected limits

### Error

Sometimes JavaScript or a library throws a general `Error`.

Read the message that comes with it.

The message is usually more useful than the word `Error`.

---

## HTTP Status Codes

When working with websites, APIs, and Express, you will see HTTP status codes.

These codes describe what happened with a request.

### Common Status Codes

| Code | Meaning |
|---|---|
| 200 | OK / success |
| 201 | Created |
| 301 | Moved permanently |
| 400 | Bad request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not found |
| 500 | Server error |

### 404 Not Found

A 404 means:

```text
The client reached a server, but the requested route/resource was not found.
```

Possible causes:

- wrong URL
- wrong route
- typo in path
- file does not exist
- server route not created
- route order problem

### 500 Server Error

A 500 means:

```text
The server received the request but crashed or failed while handling it.
```

Possible causes:

- bug in server code
- database issue
- missing variable
- failed import
- unhandled exception

### Important Difference

```text
404 = I cannot find what you asked for.
500 = I found the server, but the server failed while processing.
```

---

## Path and File Name Problems

Path problems are extremely common.

Examples:

```text
Cannot find file
Module not found
404 for image
CSS not loading
Import failed
```

### Check Spelling

These are different:

```text
Header.jsx
header.jsx
```

Some systems are case-sensitive.

### Check Location

If this import fails:

```js
import Header from "./components/Header.jsx";
```

Ask:

```text
Is there a components folder next to this file?
Is Header.jsx inside it?
Is the capitalization correct?
```

### Check Relative Paths

Common symbols:

| Symbol | Meaning |
|---|---|
| `./` | Current folder |
| `../` | One folder up |
| `/` | Root path in many contexts |

Example:

```js
import helper from "../utils/helper.js";
```

Meaning:

```text
Go up one folder, then into utils, then find helper.js.
```

### Avoid Spaces in File Names

Instead of:

```text
my file.js
```

Use:

```text
my-file.js
```

or:

```text
myFile.js
```

Spaces can create annoying path problems.

---

## Import and Export Problems

Modern JavaScript projects often use imports and exports.

### Named Export

```js
export function add(a, b) {
  return a + b;
}
```

Import:

```js
import { add } from "./math.js";
```

Named imports use curly braces.

### Default Export

```js
export default function add(a, b) {
  return a + b;
}
```

Import:

```js
import add from "./math.js";
```

Default imports do not use curly braces.

### Common Mistake

Export:

```js
export function add(a, b) {
  return a + b;
}
```

Wrong import:

```js
import add from "./math.js";
```

Better:

```js
import { add } from "./math.js";
```

### Another Common Mistake

Import path typo:

```js
import Header from "./componets/Header.jsx";
```

Problem:

```text
components is misspelled.
```

---

## npm Errors

npm errors are common in JavaScript projects.

### Missing Script

Error:

```text
Missing script: "dev"
```

Meaning:

```text
package.json does not have a dev script.
```

Check:

```json
"scripts": {
  "dev": "vite"
}
```

Maybe the project uses:

```bash
npm start
```

instead.

### Dependency Missing

Error:

```text
Cannot find module 'vite'
```

Try:

```bash
npm install
```

### Weird Dependency Problems

Sometimes dependencies get out of sync.

Common reset:

```bash
rm -rf node_modules
npm install
```

Be careful with `rm -rf`.

Make sure you are inside the project folder before running it.

### npm Install Fails

Possible causes:

- no internet connection
- typo in package name
- permission issue
- incompatible Node version
- corrupted install
- old lock file
- package no longer exists

Check:

```bash
node --version
npm --version
```

---

## Git Errors

Git errors can look scary, but they usually point to a clear issue.

### Not a Git Repository

Error:

```text
fatal: not a git repository
```

Meaning:

```text
You are not inside a Git-tracked project folder.
```

Check:

```bash
pwd
ls
```

Maybe you need to `cd` into the project.

### Nothing to Commit

Message:

```text
nothing to commit, working tree clean
```

Meaning:

```text
Git sees no changes that need committing.
```

This is not an error.

### Permission Denied Publickey

Error:

```text
Permission denied (publickey)
```

Meaning:

```text
GitHub did not accept your SSH authentication.
```

Possible causes:

- SSH key not created
- public key not added to GitHub
- wrong remote URL
- SSH agent not running
- no permission to repo

### Rejected Push

Error idea:

```text
Updates were rejected because the remote contains work that you do not have locally.
```

Meaning:

```text
GitHub has commits that your local branch does not have.
```

Usually:

```bash
git pull
```

Then resolve conflicts if needed, then push again.

### Merge Conflict

Meaning:

```text
Git could not automatically combine changes.
```

Use:

```bash
git status
```

Open the conflicted files, resolve markers, then:

```bash
git add .
git commit
```

---

## Express and Server Errors

When using Express, errors may appear in the terminal, browser, or API response.

### Cannot GET

Browser shows:

```text
Cannot GET /data
```

Meaning:

```text
The server is running, but no GET route matched /data.
```

Check:

```js
app.get("/data", (req, res) => {
  res.json(data);
});
```

Also check:

- route spelling
- server port
- route order
- HTTP method
- whether the server restarted after edits

### Server Not Running

Browser fails to connect.

Possible causes:

- forgot `npm start`
- server crashed
- wrong port
- terminal process stopped

### Response Already Sent

Error idea:

```text
Cannot set headers after they are sent
```

Meaning:

```text
The server tried to send more than one response for one request.
```

Example problem:

```js
app.get("/test", (req, res) => {
  res.send("First response");
  res.send("Second response");
});
```

A request should usually get one final response.

---

## React Errors

React errors often happen because the UI tries to render before data is ready.

### Cannot Read Properties of Undefined

Example:

```js
return <h1>{user.name}</h1>;
```

If `user` is undefined, this fails.

Possible fix:

```js
return <h1>{user?.name}</h1>;
```

Or conditionally render:

```js
if (!user) {
  return <p>Loading...</p>;
}

return <h1>{user.name}</h1>;
```

### `.map is not a function`

Example:

```js
items.map(item => <p>{item.name}</p>)
```

If `items` is not an array, this fails.

Check:

```js
console.log(items);
```

Possible fix:

```js
const [items, setItems] = useState([]);
```

### Missing Key Warning

Warning:

```text
Each child in a list should have a unique "key" prop.
```

Example fix:

```jsx
items.map(item => (
  <li key={item.id}>{item.name}</li>
));
```

This is a warning, not always a crash, but it should still be fixed.

---

## Using `console.log()`

`console.log()` is one of the simplest debugging tools.

It prints information so you can inspect what your code is doing.

Example:

```js
console.log("Button clicked");
```

Example with data:

```js
console.log("user:", user);
```

Example in Express:

```js
app.get("/data", (req, res) => {
  console.log("GET /data was hit");
  res.json(data);
});
```

### Good Uses

Use `console.log()` to check:

- Did this function run?
- What value does this variable have?
- Did this route get hit?
- What data came back from the API?
- What is inside `req.body`?
- What is inside state?

### Label Your Logs

Weak:

```js
console.log(data);
```

Better:

```js
console.log("data from API:", data);
```

Labels help when many logs appear.

### Remove Unneeded Logs

Before final submission, remove logs that are no longer useful.

Keep only intentional logs.

---

## Commenting Out Code

Sometimes you can isolate a problem by temporarily commenting out code.

Example:

```js
// const result = riskyFunction();
```

This helps answer:

```text
Does the app work when this section is removed?
```

But be careful.

Do not randomly comment out huge sections without tracking what you changed.

Better:

```text
Comment out one small section.
Test.
Restore or fix.
Move to the next section.
```

---

## Rubber Duck Debugging

Rubber duck debugging means explaining the problem out loud, step by step, as if explaining it to a rubber duck.

It sounds silly, but it works.

When you explain slowly, you often notice the problem yourself.

Use this format:

```text
I expected this to happen:
...

Instead, this happened:
...

The error message says:
...

The file and line number are:
...

The last thing I changed was:
...

I have already tried:
...
```

This also prepares you to ask another person for help clearly.

---

## How to Search for an Error

Searching for errors is a normal developer skill.

### Search the Important Part

Do not always paste the entire error.

Look for the reusable part.

Example:

```text
TypeError: Cannot read properties of undefined
```

Search:

```text
JavaScript TypeError Cannot read properties of undefined
```

For React:

```text
React Cannot read properties of undefined useState
```

For Express:

```text
Express Cannot GET route
```

For npm:

```text
npm missing script dev
```

### Remove Project-Specific Paths

Do not search:

```text
/Users/student/Desktop/project-3/src/components/Card.jsx error
```

That path only exists on your computer.

Search the general error instead.

### Add the Tool Name

Include the tool or framework:

```text
React
Express
Vite
npm
Git
JavaScript
Node
```

Example:

```text
Vite failed to resolve import React
```

---

## How to Ask for Help

A good help request saves time.

Instead of:

```text
It does not work.
```

Say:

```text
I am trying to run npm run dev.
I expected the React app to start.
Instead, I get "Missing script: dev".
I checked package.json and only see a start script.
Should I use npm start instead?
```

Include:

- what you are trying to do
- what you expected
- what actually happened
- the exact error message
- what file/line is involved
- what you already tried
- screenshot or code snippet if useful

### Good Help Template

```text
Goal:
I am trying to...

Expected result:
I expected...

Actual result:
Instead...

Error message:
...

File/line:
...

What I tried:
...
```

---

## Common Beginner Confusions

### Confusion 1: “The error is long, so it must be impossible.”

Long errors often contain repeated stack trace information.

Start with:

```text
First error line
File name
Line number
Message
```

### Confusion 2: “The error points to a file I did not write.”

Sometimes tools show internal files.

Look for your project files in the stack trace.

Those are usually more useful.

### Confusion 3: “I refreshed the browser, but nothing changed.”

Maybe the server crashed.

Check the terminal.

Maybe the file was not saved.

Save the file.

Maybe the dev server needs restarting.

Stop with `Ctrl + C`, then start again.

### Confusion 4: “The code looks right.”

It may be almost right.

Common invisible problems:

- capitalization
- missing import
- wrong folder
- unsaved file
- wrong terminal location
- wrong branch
- wrong port
- extra bracket
- missing comma
- stale server

### Confusion 5: “I fixed the code, but Git still shows changes.”

That is expected.

Git tracks file changes.

After fixing, use:

```bash
git status
git add .
git commit -m "Fix ..."
```

### Confusion 6: “The page is blank.”

Check:

1. Browser console
2. Terminal
3. Network tab
4. Root component
5. Import paths
6. API response
7. Whether the server is running

A blank page usually has clues somewhere.

---

## Debugging Checklist

When something breaks, walk through this checklist.

### Basic Checks

```text
Did I save the file?
Am I in the correct folder?
Is the server running?
Am I using the correct port?
Did I spell the file/path/variable correctly?
Did I check capitalization?
Did I read the first error message?
```

### Terminal Checks

```bash
pwd
ls
npm install
npm run dev
npm start
git status
```

### Browser Checks

```text
Open DevTools
Check Console
Check Network tab
Refresh page
Verify URL and port
```

### Code Checks

```text
Check imports
Check exports
Check brackets
Check variable names
Check function names
Check route paths
Check middleware order
Check data shape
```

### Git Checks

```text
Am I on the correct branch?
Did I pull latest changes?
Did I commit my work?
Did I push?
Is there a merge conflict?
```

---

## Practice Lab

This lab gives you small debugging scenarios.

### Scenario 1: ReferenceError

Code:

```js
console.log(userName);
```

Error:

```text
ReferenceError: userName is not defined
```

Question:

```text
Was userName declared?
Was it spelled correctly?
Should it be imported?
```

Possible fix:

```js
const userName = "Student";
console.log(userName);
```

### Scenario 2: Missing npm Script

Command:

```bash
npm run dev
```

Error:

```text
Missing script: "dev"
```

Check `package.json`:

```json
"scripts": {
  "start": "node server.js"
}
```

Fix:

```bash
npm start
```

Or add a `dev` script if the project needs one.

### Scenario 3: Cannot GET

Browser:

```text
Cannot GET /data
```

Check server:

```js
app.get("/items", (req, res) => {
  res.json(data);
});
```

Problem:

```text
The route is /items, not /data.
```

Fix the URL or add the correct route.

### Scenario 4: Import Path

Code:

```js
import Header from "./components/header.jsx";
```

Actual file:

```text
components/Header.jsx
```

Problem:

```text
Capitalization mismatch.
```

Fix:

```js
import Header from "./components/Header.jsx";
```

### Scenario 5: React Data

Code:

```jsx
function App() {
  const [items, setItems] = useState();

  return items.map(item => <p>{item.name}</p>);
}
```

Problem:

```text
items starts as undefined.
```

Fix:

```jsx
function App() {
  const [items, setItems] = useState([]);

  return items.map(item => <p key={item.id}>{item.name}</p>);
}
```

---

## Quick Reference

| Problem | First Place to Look |
|---|---|
| Blank page | Browser console |
| Server not responding | Terminal |
| API not working | Network tab and server route |
| `Cannot GET /path` | Express routes |
| `command not found` | Installation/PATH/spelling |
| `missing script` | `package.json` scripts |
| `Cannot find module` | `npm install`, dependencies, import |
| `not a git repository` | Current folder |
| `Permission denied publickey` | GitHub SSH setup |
| Merge conflict | `git status` and conflicted files |
| CSS not loading | File path, link tag, browser Network tab |
| Image not loading | Image path, file name, capitalization |
| React `.map is not a function` | Check whether data is an array |
| `undefined` property error | Check data exists before using it |

---

## Common Error Translations

| Error | Beginner Translation |
|---|---|
| `ReferenceError: x is not defined` | JavaScript does not know what `x` is |
| `SyntaxError` | The code is not valid JavaScript syntax |
| `TypeError` | A value is not the type/shape expected |
| `Cannot find module` | A package/file/import could not be found |
| `Missing script: dev` | `package.json` has no `dev` script |
| `Cannot GET /route` | Express has no matching GET route |
| `404 Not Found` | The requested resource/route was not found |
| `500 Server Error` | The server failed while handling the request |
| `EADDRINUSE` | The port is already being used |
| `not a git repository` | You are not inside a Git repo |
| `Permission denied` | You do not have permission for that action |

---

## Final Mental Model

Debugging is not guessing wildly.

Debugging is investigation.

Use this flow:

```text
1. What was I trying to do?
2. What did I expect to happen?
3. What actually happened?
4. Where did the error appear?
5. What does the first useful error message say?
6. What file and line number are involved?
7. What changed recently?
8. What is one small thing I can test?
9. Did that test change the result?
10. What clue do I have now?
```

The goal is not to never see errors.

The goal is to get better at following the clues.

A strong developer is not someone who never breaks code.

A strong developer is someone who can calmly work from:

```text
It broke.
```

to:

```text
I found the cause.
```

to:

```text
I fixed it and understand why.
```
