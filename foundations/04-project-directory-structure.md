# Project Directory Structure: A Beginner Guide for Organizing Code

> **Purpose:** This guide explains how to organize project folders so your code is easier to understand, maintain, debug, and share.  
> It also shows how to create a `structure.md` file from a terminal tree view so students can document the layout of their projects.

---

## Table of Contents

1. [Who This Guide Is For](#who-this-guide-is-for)
2. [The Big Idea](#the-big-idea)
3. [Why Project Structure Matters](#why-project-structure-matters)
4. [Files vs. Folders](#files-vs-folders)
5. [The Root Directory](#the-root-directory)
6. [Common Files in a Project](#common-files-in-a-project)
7. [Common Folders in a Project](#common-folders-in-a-project)
8. [A Simple Static Website Structure](#a-simple-static-website-structure)
9. [A Beginner JavaScript Project Structure](#a-beginner-javascript-project-structure)
10. [A React Project Structure](#a-react-project-structure)
11. [Separating Concerns](#separating-concerns)
12. [Naming Files and Folders](#naming-files-and-folders)
13. [The `tree` Command](#the-tree-command)
14. [Creating a `structure.md` File](#creating-a-structuremd-file)
15. [What to Exclude from Your Structure File](#what-to-exclude-from-your-structure-file)
16. [Documenting Your Project Structure](#documenting-your-project-structure)
17. [Common Beginner Confusions](#common-beginner-confusions)
18. [Practice Lab](#practice-lab)
19. [Quick Reference](#quick-reference)
20. [Final Mental Model](#final-mental-model)

---

## Who This Guide Is For

This guide is for students who are learning to build coding projects and are starting to feel the pain of messy folders.

At the beginning, it is common to put everything in one folder:

```text
index.html
style.css
script.js
image1.png
image2.png
test.js
old-script.js
final-final.js
notes.txt
```

That works for tiny projects.

But as a project grows, messy structure becomes a problem.

You start asking:

```text
Where should this file go?
Why is this file here?
Which file controls the page?
Where are the images?
Where are the components?
Where is the main JavaScript file?
What files should I commit?
What files should I ignore?
```

This guide helps answer those questions.

---

## The Big Idea

A project directory is not just a folder full of files.

It is the shape of your project.

Good structure helps other people understand your project before they even read the code.

A strong project structure should make it easier to answer:

```text
What kind of project is this?
Where does the app start?
Where are the source files?
Where are the styles?
Where are the images?
Where are the reusable pieces?
Where is the documentation?
What files are generated automatically?
What files should not be edited manually?
```

A simple rule:

```text
Project structure should reduce confusion.
```

---

## Why Project Structure Matters

Good project structure helps with:

- readability
- teamwork
- debugging
- GitHub presentation
- onboarding new developers
- preventing duplicate files
- separating responsibilities
- finding files quickly
- scaling the project later

Bad project structure causes:

- lost files
- repeated code
- unclear entry points
- broken imports
- confusing Git commits
- accidental deletion
- messy deployments
- harder debugging

A beginner project can be simple.

Simple is good.

But simple should still be intentional.

---

## Files vs. Folders

A **file** stores content.

Examples:

```text
index.html
style.css
script.js
README.md
package.json
```

A **folder** groups related files.

Examples:

```text
src/
assets/
components/
styles/
utils/
docs/
```

A folder should answer:

```text
Why do these files belong together?
```

If the answer is unclear, the folder may not be useful.

---

## The Root Directory

The **root directory** is the top-level folder of your project.

Example:

```text
my-project/
```

Everything inside the project belongs under that folder.

Example:

```text
my-project/
├── README.md
├── index.html
├── css/
├── js/
└── images/
```

When someone opens your GitHub repository, they usually start at the root directory.

That root should contain the most important project-level files.

Common root-level files:

```text
README.md
.gitignore
package.json
index.html
LICENSE
structure.md
```

The root should not become a junk drawer.

---

## Common Files in a Project

### `README.md`

The README explains the project.

It usually includes:

- project name
- purpose
- setup instructions
- usage instructions
- screenshots
- technologies used
- known issues
- future improvements

A README is often the first thing people see on GitHub.

### `.gitignore`

The `.gitignore` file tells Git what not to track.

Examples of files/folders often ignored:

```text
node_modules/
.env
dist/
build/
.DS_Store
```

### `index.html`

In many web projects, `index.html` is the main HTML file.

It is often the browser entry point.

### `package.json`

In Node/npm projects, `package.json` describes the project and lists scripts and dependencies.

Examples:

```json
{
  "scripts": {
    "dev": "vite",
    "start": "node server.js"
  }
}
```

### `structure.md`

A `structure.md` file documents the project folder layout.

This can help instructors, teammates, reviewers, and future developers understand the project quickly.

---

## Common Folders in a Project

### `src/`

Short for **source**.

This usually contains the main code developers write.

Examples:

```text
src/
├── main.js
├── App.jsx
├── components/
└── utils/
```

### `assets/`

Used for static project assets.

Examples:

```text
assets/
├── images/
├── icons/
└── fonts/
```

### `css/` or `styles/`

Used for stylesheets.

Examples:

```text
css/
└── style.css
```

or:

```text
styles/
├── global.css
└── layout.css
```

### `js/`

Used for JavaScript files in simpler projects.

Example:

```text
js/
├── main.js
└── helpers.js
```

### `components/`

Used for reusable UI pieces, especially in React.

Example:

```text
components/
├── Header.jsx
├── Footer.jsx
└── Card.jsx
```

### `utils/`

Used for helper functions that are not tied to one visual component.

Example:

```text
utils/
├── formatDate.js
├── calculateTotal.js
└── sanitizeInput.js
```

### `docs/`

Used for extra documentation.

Example:

```text
docs/
├── planning.md
├── api-notes.md
└── structure.md
```

### `tests/`

Used for test files.

Example:

```text
tests/
├── calculateTotal.test.js
└── sanitizeInput.test.js
```

---

## A Simple Static Website Structure

A basic HTML/CSS/JavaScript website might look like this:

```text
simple-website/
├── README.md
├── index.html
├── about.html
├── contact.html
├── css/
│   └── style.css
├── js/
│   └── script.js
└── assets/
    └── images/
        ├── hero.jpg
        └── logo.png
```

### Why This Works

```text
HTML files are easy to find.
CSS is grouped in css/.
JavaScript is grouped in js/.
Images are grouped in assets/images/.
README explains the project.
```

This structure is simple but clear.

---

## A Beginner JavaScript Project Structure

For a small JavaScript app, you might use:

```text
change-calculator/
├── README.md
├── index.html
├── css/
│   └── style.css
├── js/
│   ├── main.js
│   ├── calculateChange.js
│   └── renderChange.js
└── assets/
    └── screenshots/
        └── app-preview.png
```

### Possible File Responsibilities

| File | Purpose |
|---|---|
| `index.html` | Page structure |
| `style.css` | Visual styling |
| `main.js` | Connects the app together |
| `calculateChange.js` | Handles calculation logic |
| `renderChange.js` | Updates the page |

This is better than putting all JavaScript into one huge file.

---

## A React Project Structure

A beginner React project might look like this:

```text
react-app/
├── README.md
├── package.json
├── vite.config.js
├── index.html
├── public/
│   └── favicon.svg
└── src/
    ├── main.jsx
    ├── App.jsx
    ├── components/
    │   ├── Header.jsx
    │   ├── TodoForm.jsx
    │   └── TodoList.jsx
    ├── hooks/
    │   └── useTodos.js
    ├── utils/
    │   └── filterTodos.js
    └── styles/
        └── global.css
```

### Possible File Responsibilities

| File/Folder | Purpose |
|---|---|
| `package.json` | npm project settings, dependencies, scripts |
| `index.html` | HTML shell where React mounts |
| `src/main.jsx` | React entry point |
| `src/App.jsx` | Main app component |
| `src/components/` | Reusable visual components |
| `src/hooks/` | Custom React hooks |
| `src/utils/` | Helper functions |
| `src/styles/` | CSS files |
| `public/` | Static files served directly |

React project structures vary.

The most important thing is consistency.

---

## Separating Concerns

**Separation of concerns** means each file or folder should have a clear job.

Instead of one file doing everything, split the project into understandable pieces.

### Poor Separation

```text
script.js
```

Inside this one file:

```text
form validation
API calls
DOM updates
math calculations
event listeners
date formatting
```

This can become hard to read.

### Better Separation

```text
js/
├── main.js
├── api.js
├── validateForm.js
├── renderResults.js
└── formatDate.js
```

Now each file has a more focused job.

A good question to ask:

```text
Can I explain what this file does in one sentence?
```

If not, the file may be doing too much.

---

## Naming Files and Folders

Good names reduce confusion.

### Use Clear Names

Good:

```text
calculateTotal.js
userProfile.css
TodoList.jsx
apiClient.js
```

Weak:

```text
stuff.js
things.css
newnew.js
final2.js
misc.jsx
```

### Be Consistent

Choose a naming style and stick with it.

Common styles:

| Style | Example |
|---|---|
| kebab-case | `user-profile.js` |
| camelCase | `userProfile.js` |
| PascalCase | `UserProfile.jsx` |
| snake_case | `user_profile.py` |

In React, components are commonly written in PascalCase:

```text
Header.jsx
TodoList.jsx
UserCard.jsx
```

Utility files are often camelCase or kebab-case:

```text
formatDate.js
calculate-total.js
```

### Avoid Spaces

Avoid spaces in project file names.

Instead of:

```text
my notes.txt
```

Use:

```text
my-notes.txt
```

or:

```text
my_notes.txt
```

Spaces can make terminal commands more annoying because the shell treats spaces as separators.

---

## The `tree` Command

The `tree` command prints a visual structure of your project folders and files.

Example:

```bash
tree
```

Example output:

```text
my-project/
├── README.md
├── index.html
├── css/
│   └── style.css
└── js/
    └── script.js
```

This is very useful for documentation.

It lets someone see the shape of your project quickly.

### Installing `tree`

Some systems already have `tree`.

Check:

```bash
tree --version
```

If it works, you have it.

If not, you may need to install it.

#### macOS with Homebrew

```bash
brew install tree
```

#### Ubuntu/Debian Linux

```bash
sudo apt install tree
```

#### Fedora Linux

```bash
sudo dnf install tree
```

#### Windows

Options may include:

```powershell
tree
```

Windows has a built-in `tree` command, but its options/output may differ from Linux/macOS.

Developers on Windows may also use Git Bash, WSL, or PowerShell depending on the project.

---

## Creating a `structure.md` File

A `structure.md` file is a Markdown document that shows your project layout.

This is helpful because:

- instructors can quickly inspect your project
- teammates can understand where things live
- GitHub readers can see the architecture
- future you can remember why the project was organized this way

### Basic Method

From the root of your project, run:

```bash
tree > structure.md
```

This writes the tree output into a file called `structure.md`.

But the raw file may not be formatted nicely for Markdown.

A better version is to create the file manually and wrap the tree in a code block.

Example `structure.md`:

````md
# Project Structure

```text
my-project/
├── README.md
├── index.html
├── css/
│   └── style.css
└── js/
    └── script.js
```
````

### Generate Then Edit

You can generate the tree first:

```bash
tree > structure.md
```

Then open `structure.md` and add:

````md
# Project Structure

```text
````

above the tree output, and:

````md
```
````

below it.

### Better: Exclude Large Folders

For npm projects, do not include `node_modules`.

Use:

```bash
tree -I "node_modules" > structure.md
```

For multiple exclusions:

```bash
tree -I "node_modules|dist|build|.git" > structure.md
```

Then edit the file to add a title and Markdown code block.

### Useful Depth Limit

Sometimes a project is too large.

Limit the depth:

```bash
tree -L 3 -I "node_modules|dist|build|.git" > structure.md
```

This shows only three levels deep.

---

## What to Exclude from Your Structure File

Some folders are too large, generated, private, or unnecessary.

Usually exclude:

```text
node_modules/
.git/
dist/
build/
coverage/
.env
.DS_Store
```

### Why Exclude `node_modules/`?

`node_modules/` can contain thousands of files.

It is generated by npm.

It should usually not be committed to GitHub.

Including it in `structure.md` makes the file huge and unreadable.

### Why Exclude `.git/`?

`.git/` contains Git internals.

It is not useful for a beginner project structure document.

### Why Exclude `.env`?

`.env` files often contain secrets.

Never publish real secrets.

---

## Documenting Your Project Structure

A strong `structure.md` does more than show a tree.

It can also explain the purpose of important files and folders.

Example:

````md
# Project Structure

```text
my-app/
├── README.md
├── package.json
├── index.html
└── src/
    ├── main.jsx
    ├── App.jsx
    ├── components/
    ├── utils/
    └── styles/
```

## Folder Notes

| Path | Purpose |
|---|---|
| `src/` | Main source code for the app |
| `src/components/` | Reusable React components |
| `src/utils/` | Helper functions |
| `src/styles/` | CSS files |
| `public/` | Static files served directly |
````

This is much more useful than a raw tree dump.

---

## Common Beginner Confusions

### Confusion 1: “Where should my files go?”

Ask what job the file has.

Examples:

```text
Visual component?     → components/
Helper function?      → utils/
CSS file?             → styles/ or css/
Image?                → assets/images/
Documentation?        → docs/ or root README.md
Test?                 → tests/ or next to the file being tested
```

### Confusion 2: “Should everything go in `src/`?”

Not always.

In many modern JavaScript/React projects, most source code goes in `src/`.

But project-level files stay at the root.

Examples:

```text
README.md
package.json
.gitignore
vite.config.js
```

Those are not usually placed inside `src/`.

### Confusion 3: “What is `public/`?”

In many frontend projects, `public/` contains static files that are served directly.

Examples:

```text
favicon.svg
robots.txt
static images
```

Files in `public/` are usually not processed the same way as files imported inside `src/`.

### Confusion 4: “What is `node_modules/`?”

`node_modules/` contains installed npm packages.

It is generated by:

```bash
npm install
```

You usually do not edit it.

You usually do not commit it.

### Confusion 5: “Why does my import path break when I move a file?”

Because imports depend on file location.

Example:

```js
import Header from "./components/Header.jsx";
```

If you move the file, the path may need to change.

### Confusion 6: “Is there one correct project structure?”

No.

There are common patterns, but no single perfect structure for every project.

The best structure depends on:

- project size
- framework
- team rules
- deployment platform
- testing setup
- project complexity

For beginners, clear and simple beats clever and complicated.

---

## Practice Lab

Create a practice project structure.

### Step 1: Create a Project Folder

```bash
mkdir project-structure-practice
cd project-structure-practice
```

### Step 2: Create Common Files

```bash
touch README.md
touch index.html
touch .gitignore
```

### Step 3: Create Folders

```bash
mkdir css js assets docs
mkdir assets/images
```

### Step 4: Create Starter Files

```bash
touch css/style.css
touch js/script.js
touch docs/notes.md
touch assets/images/.gitkeep
```

`.gitkeep` is sometimes used as a placeholder so Git tracks an otherwise empty folder.

### Step 5: View the Structure

```bash
tree
```

If `tree` is not installed, use:

```bash
find . -maxdepth 3
```

### Step 6: Create `structure.md`

```bash
tree -I ".git" > structure.md
```

Then open `structure.md` and wrap the output in a Markdown code block.

### Step 7: Add Folder Notes

Add a table explaining what each folder is for:

```md
## Folder Notes

| Path | Purpose |
|---|---|
| `css/` | Stylesheets |
| `js/` | JavaScript files |
| `assets/images/` | Image files |
| `docs/` | Project notes and documentation |
```

### Step 8: Check Your Work

Your project should look similar to:

```text
project-structure-practice/
├── README.md
├── index.html
├── structure.md
├── css/
│   └── style.css
├── js/
│   └── script.js
├── assets/
│   └── images/
│       └── .gitkeep
└── docs/
    └── notes.md
```

---

## Quick Reference

| Thing | Purpose |
|---|---|
| Root directory | Top-level project folder |
| `README.md` | Explains the project |
| `.gitignore` | Tells Git what not to track |
| `index.html` | Common web entry page |
| `package.json` | npm project config |
| `src/` | Main source code |
| `components/` | Reusable UI pieces |
| `utils/` | Helper functions |
| `styles/` or `css/` | Styling files |
| `assets/` | Images, icons, fonts, media |
| `docs/` | Extra documentation |
| `tests/` | Test files |
| `structure.md` | Project layout documentation |
| `node_modules/` | Installed npm packages; usually ignored |
| `dist/` or `build/` | Generated production output |
| `tree` | Prints visual project structure |

---

## Useful Commands

| Command | Meaning |
|---|---|
| `pwd` | Show current folder |
| `ls` | List current folder |
| `mkdir folder-name` | Create a folder |
| `mkdir -p a/b/c` | Create nested folders |
| `touch file.txt` | Create an empty file |
| `tree` | Show folder tree |
| `tree > structure.md` | Save tree output to a file |
| `tree -I "node_modules|.git"` | Exclude folders from tree |
| `tree -L 3` | Limit tree depth |
| `find . -maxdepth 3` | Alternative if `tree` is unavailable |

---

## Final Mental Model

A project folder should tell a story.

When someone opens it, they should be able to understand the basic shape of the project quickly.

Think of project structure like a workshop:

```text
Root folder      = the workshop
README.md        = the instruction sheet
src/             = the main workbench
components/      = reusable parts
utils/           = small tools
assets/          = materials like images and icons
styles/          = paint and visual design
docs/            = notes and explanations
tests/           = quality checks
structure.md     = map of the workshop
```

A clean structure does not need to be fancy.

It needs to be understandable.

Beginner rule:

```text
Start simple.
Name things clearly.
Group related files together.
Document the structure.
Avoid dumping everything into one folder.
```
