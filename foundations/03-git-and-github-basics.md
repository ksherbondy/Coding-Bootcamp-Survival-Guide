# Git and GitHub Basics: A Beginner Walkthrough

> **Purpose:** This guide explains Git and GitHub from the ground up.  
> It is written for new developers who need a practical walkthrough of SSH setup, repositories, commits, remotes, branches, merging, pulling, and pushing.

---

## Table of Contents

1. [Who This Guide Is For](#who-this-guide-is-for)
2. [The Big Idea](#the-big-idea)
3. [Git vs. GitHub](#git-vs-github)
4. [The Mental Model](#the-mental-model)
5. [Installing and Checking Git](#installing-and-checking-git)
6. [First-Time Git Configuration](#first-time-git-configuration)
7. [SSH Setup for GitHub](#ssh-setup-for-github)
8. [Creating a New Local Repository](#creating-a-new-local-repository)
9. [The Basic Git Workflow](#the-basic-git-workflow)
10. [Understanding Git Status](#understanding-git-status)
11. [Adding Files](#adding-files)
12. [Committing Changes](#committing-changes)
13. [Connecting to GitHub with Remotes](#connecting-to-github-with-remotes)
14. [Pushing to GitHub](#pushing-to-github)
15. [Cloning an Existing Repository](#cloning-an-existing-repository)
16. [Pulling Changes](#pulling-changes)
17. [Fetching vs. Pulling](#fetching-vs-pulling)
18. [Branches](#branches)
19. [Merging Branches](#merging-branches)
20. [Merge Conflicts](#merge-conflicts)
21. [Removing or Changing a Remote](#removing-or-changing-a-remote)
22. [Common Team Workflow](#common-team-workflow)
23. [Common Beginner Confusions](#common-beginner-confusions)
24. [Beginner Safety Rules](#beginner-safety-rules)
25. [Practice Lab](#practice-lab)
26. [Quick Reference](#quick-reference)
27. [Further Reading](#further-reading)

---

## Who This Guide Is For

This guide is for students who are new to Git and GitHub.

Git can feel confusing because beginners are usually expected to use it before they understand what it is doing.

You may hear commands like:

```bash
git add .
git commit -m "message"
git push
git pull
git merge
```

At first, these commands can feel like magic words.

They are not magic.

Git is just a tool for tracking changes in your project over time.

This guide explains the foundation and then walks through the commands you will use most often.

---

## The Big Idea

Git is a version control system.

That means Git helps you track the history of a project.

It can answer questions like:

```text
What changed?
Who changed it?
When did it change?
Why did it change?
Can we go back to an earlier version?
Can multiple people work on this project without overwriting each other?
```

A simple way to think about Git:

```text
Git lets you save meaningful checkpoints as your project changes.
```

Those checkpoints are called **commits**.

---

## Git vs. GitHub

Git and GitHub are related, but they are not the same thing.

### Git

Git is the tool installed on your computer.

It tracks changes in your project.

You can use Git without GitHub.

### GitHub

GitHub is a website/platform that hosts Git repositories online.

GitHub lets you:

- store your code online
- share code with others
- collaborate with teammates
- review code changes
- open pull requests
- track issues
- show projects publicly

### Simple Summary

```text
Git = version control tool
GitHub = online place to store and collaborate on Git repositories
```

A useful analogy:

```text
Git is the camera that takes snapshots.
GitHub is the photo album stored online.
```

---

## The Mental Model

Git has a few important areas.

```text
Working Directory → Staging Area → Local Repository → Remote Repository
```

### Working Directory

This is your actual project folder.

It contains the files you edit.

Example:

```text
index.html
style.css
script.js
README.md
```

### Staging Area

The staging area is where you place changes before committing them.

You stage files with:

```bash
git add
```

### Local Repository

The local repository is the Git history stored on your computer.

You save changes to it with:

```bash
git commit
```

### Remote Repository

The remote repository is the version hosted somewhere else, usually GitHub.

You send commits to it with:

```bash
git push
```

You receive commits from it with:

```bash
git pull
```

### Full Flow

```text
Edit files
   ↓
git add
   ↓
git commit
   ↓
git push
```

Beginner translation:

```text
Change something.
Choose what should be saved.
Save a checkpoint.
Upload the checkpoint to GitHub.
```

---

## Installing and Checking Git

Before using Git, make sure it is installed.

Run:

```bash
git --version
```

Example output:

```bash
git version 2.45.0
```

If you see a version number, Git is installed.

If you see something like:

```text
command not found
```

Then Git may not be installed or your terminal cannot find it.

### Windows

Common options:

- Install Git for Windows.
- Use Git Bash, PowerShell, or Windows Terminal.
- Some coding programs may also install Git or prompt you to install it.

### macOS

Git may be available through Xcode Command Line Tools.

If Git is not installed, macOS may prompt you to install the tools when you run:

```bash
git --version
```

### Linux

Use your package manager.

Examples:

```bash
sudo apt install git
```

or:

```bash
sudo dnf install git
```

The exact command depends on your Linux distribution.

---

## First-Time Git Configuration

Git needs to know your name and email for commits.

Run these once:

```bash
git config --global user.name "Your Name"
git config --global user.email "your-email@example.com"
```

Check your config:

```bash
git config --global --list
```

You should see something like:

```text
user.name=Your Name
user.email=your-email@example.com
```

### Important Note About Email

If you are using GitHub and want privacy, GitHub can provide a no-reply email address.

Use the email you want attached to your commits.

---

## SSH Setup for GitHub

SSH lets your computer connect securely to GitHub without typing your username and password every time.

SSH uses a key pair:

```text
Private key = stays on your computer
Public key = gets added to GitHub
```

Never share your private key.

### Step 1: Check for Existing SSH Keys

Run:

```bash
ls -al ~/.ssh
```

Look for files such as:

```text
id_ed25519
id_ed25519.pub
id_rsa
id_rsa.pub
```

The `.pub` file is the public key.

The file without `.pub` is the private key.

### Step 2: Generate a New SSH Key

A common modern option is Ed25519:

```bash
ssh-keygen -t ed25519 -C "your-email@example.com"
```

When asked where to save the key, pressing Enter usually accepts the default location.

Example default:

```text
~/.ssh/id_ed25519
```

You may be asked for a passphrase.

A passphrase adds protection to your SSH key.

### Step 3: Start the SSH Agent

On macOS/Linux/Git Bash, run:

```bash
eval "$(ssh-agent -s)"
```

Then add your key:

```bash
ssh-add ~/.ssh/id_ed25519
```

### Step 4: Copy Your Public Key

Print the public key:

```bash
cat ~/.ssh/id_ed25519.pub
```

Copy the entire output.

It usually starts with:

```text
ssh-ed25519
```

### Step 5: Add the Public Key to GitHub

In GitHub:

```text
Profile picture
→ Settings
→ SSH and GPG keys
→ New SSH key
→ Paste public key
→ Add SSH key
```

Give the key a clear title, such as:

```text
Personal laptop
```

### Step 6: Test the SSH Connection

Run:

```bash
ssh -T git@github.com
```

You may see a message asking if you trust the host.

Type:

```text
yes
```

If successful, you should see a message from GitHub confirming authentication.

### SSH Setup Summary

```text
Generate key
   ↓
Add private key to ssh-agent
   ↓
Copy public key
   ↓
Paste public key into GitHub
   ↓
Test connection
```

---

## Creating a New Local Repository

A repository, often called a **repo**, is a project tracked by Git.

Create a folder:

```bash
mkdir my-project
cd my-project
```

Initialize Git:

```bash
git init
```

This creates a hidden `.git` folder.

That `.git` folder stores the Git history and repository information.

Do not casually delete the `.git` folder. If you delete it, the project files may remain, but the Git history for that local repo is gone.

### Main vs. Master

Older Git repositories often used `master` as the default branch name.

Many newer repositories use `main`.

You can initialize with `main` like this:

```bash
git init -b main
```

If your Git version does not support that option, you can rename the branch after init:

```bash
git branch -M main
```

---

## The Basic Git Workflow

The core Git workflow is:

```bash
git status
git add .
git commit -m "your message"
git push
```

That flow means:

```text
Check what changed.
Stage the changes.
Commit the changes.
Push the commits to GitHub.
```

A beginner should run `git status` constantly.

It tells you what Git sees.

---

## Understanding Git Status

Run:

```bash
git status
```

This command tells you the current state of your repository.

You may see files as:

```text
Untracked
Modified
Staged
Committed
```

### Untracked

Git sees the file, but Git is not tracking it yet.

Example:

```text
Untracked files:
  README.md
```

### Modified

Git is tracking the file, but you changed it since the last commit.

Example:

```text
modified: index.html
```

### Staged

The file is ready to be committed.

Example:

```text
Changes to be committed:
  modified: index.html
```

### Clean Working Tree

If Git says:

```text
nothing to commit, working tree clean
```

That means your local files match your latest commit.

---

## Adding Files

`git add` stages files.

Stage one file:

```bash
git add README.md
```

Stage multiple files:

```bash
git add index.html style.css script.js
```

Stage everything changed in the current folder:

```bash
git add .
```

### What Does Staging Mean?

Staging means:

```text
Include this change in the next commit.
```

Git lets you choose what goes into a commit.

That is useful because not every file change belongs in the same checkpoint.

---

## Committing Changes

A commit saves a snapshot/checkpoint in your local Git history.

Run:

```bash
git commit -m "Add project README"
```

The message should explain what changed.

Good commit messages:

```text
Add homepage layout
Fix navbar alignment
Create API fetch helper
Update README setup steps
```

Weak commit messages:

```text
stuff
changes
final
fixed things
asdf
```

A good commit message helps future readers understand the history.

### Commit Mental Model

A commit is like saying:

```text
This is a meaningful point in the project history.
Save it.
```

---

## Connecting to GitHub with Remotes

A **remote** is a connection between your local Git repo and an online repo.

The most common remote name is:

```text
origin
```

You can view remotes with:

```bash
git remote -v
```

If there are no remotes, nothing may print.

### Add a Remote

If you created an empty GitHub repository, GitHub will provide a URL.

SSH example:

```bash
git remote add origin git@github.com:username/repository-name.git
```

HTTPS example:

```bash
git remote add origin https://github.com/username/repository-name.git
```

Check it:

```bash
git remote -v
```

You should see something like:

```text
origin  git@github.com:username/repository-name.git (fetch)
origin  git@github.com:username/repository-name.git (push)
```

### SSH vs. HTTPS

| Method | What It Means |
|---|---|
| SSH | Uses SSH keys for authentication |
| HTTPS | Uses HTTPS and may require token-based authentication |

For developers who use GitHub often, SSH is usually convenient once configured.

---

## Pushing to GitHub

`git push` uploads your local commits to the remote repository.

First push for a new repo:

```bash
git push -u origin main
```

The `-u` connects your local branch to the remote branch.

After that, you can usually run:

```bash
git push
```

### Push Mental Model

```text
Local repository → GitHub repository
```

Pushing does not create a commit.

You must commit first.

Common flow:

```bash
git status
git add .
git commit -m "Add starter files"
git push
```

---

## Cloning an Existing Repository

Cloning downloads a repository from GitHub to your computer.

SSH example:

```bash
git clone git@github.com:username/repository-name.git
```

HTTPS example:

```bash
git clone https://github.com/username/repository-name.git
```

This creates a folder containing the project and its Git history.

Then move into it:

```bash
cd repository-name
```

Check status:

```bash
git status
```

### Clone Mental Model

```text
GitHub repository → your computer
```

---

## Pulling Changes

`git pull` brings changes from the remote repository into your current local branch.

Run:

```bash
git pull
```

This is common when working with teammates.

A good habit before starting work:

```bash
git pull
```

That helps make sure you are starting from the latest shared version.

### Pull Mental Model

```text
GitHub repository → local repository → working files
```

### Important

If you have local changes and someone else changed the same files, pulling may create a merge conflict.

That is normal.

It means Git needs a human to decide which version is correct.

---

## Fetching vs. Pulling

`git fetch` and `git pull` are related, but they are not the same.

### Fetch

```bash
git fetch
```

Fetch checks the remote for new commits and downloads information about them.

It does not automatically merge those changes into your current work.

### Pull

```bash
git pull
```

Pull fetches changes and then integrates them into your current branch.

Simple version:

```text
git fetch = look and download remote history
git pull = fetch plus integrate into my branch
```

Beginners often use `git pull` more often, but `git fetch` is useful when you want to inspect changes before integrating them.

---

## Branches

A branch is a separate line of work.

Branches let you work on a feature or fix without directly changing the main branch.

View branches:

```bash
git branch
```

Create a branch:

```bash
git branch feature-navbar
```

Switch to a branch:

```bash
git switch feature-navbar
```

Create and switch in one command:

```bash
git switch -c feature-navbar
```

Older tutorials may use:

```bash
git checkout -b feature-navbar
```

`checkout` still exists, but `switch` is easier for beginners because it is more specific.

### Branch Mental Model

```text
main branch = stable shared line
feature branch = separate workspace for a specific task
```

Example:

```text
main
 └── feature-navbar
```

You can work safely on `feature-navbar`, commit changes there, and later merge it back into `main`.

---

## Merging Branches

Merging combines changes from one branch into another.

Common example:

You finished work on `feature-navbar` and want to merge it into `main`.

First, switch to `main`:

```bash
git switch main
```

Pull latest changes:

```bash
git pull
```

Merge the feature branch:

```bash
git merge feature-navbar
```

If there are no conflicts, Git completes the merge.

Then push:

```bash
git push
```

### Merge Mental Model

```text
Bring the completed work from this branch into my current branch.
```

The current branch matters.

If you run:

```bash
git merge feature-navbar
```

while standing on `main`, Git merges `feature-navbar` into `main`.

If you run it while standing on some other branch, Git merges into that branch instead.

Always check:

```bash
git branch
```

The branch with `*` next to it is your current branch.

---

## Merge Conflicts

A merge conflict happens when Git cannot automatically combine changes.

Example:

Two people edit the same line of the same file.

Git may mark the file like this:

```text
<<<<<<< HEAD
Your version of the line
=======
Their version of the line
>>>>>>> feature-branch
```

This means Git needs you to choose what the final version should be.

### How to Resolve a Merge Conflict

1. Open the conflicted file.
2. Find the conflict markers.
3. Edit the file so it contains the correct final version.
4. Remove the conflict markers.
5. Save the file.
6. Stage the resolved file.
7. Commit the merge.

Commands:

```bash
git status
git add conflicted-file.txt
git commit
```

Sometimes Git creates the merge commit message for you.

### Conflict Rule

Do not panic.

A merge conflict does not mean you broke Git.

It means Git found a decision that requires human judgment.

---

## Removing or Changing a Remote

View current remotes:

```bash
git remote -v
```

### Remove a Remote

```bash
git remote remove origin
```

### Add a Remote Again

```bash
git remote add origin git@github.com:username/repository-name.git
```

### Change a Remote URL

```bash
git remote set-url origin git@github.com:username/new-repository-name.git
```

Check:

```bash
git remote -v
```

This is useful if:

- you added the wrong GitHub URL
- the repo was renamed
- you want to switch from HTTPS to SSH
- you forked a repository
- you are moving a project to a new remote

---

## Common Team Workflow

A beginner-friendly team workflow might look like this.

### Starting Work

```bash
git switch main
git pull
git switch -c feature-login-form
```

Meaning:

```text
Go to main.
Get the latest changes.
Create a new branch for my work.
```

### During Work

```bash
git status
git add .
git commit -m "Build login form layout"
```

Meaning:

```text
Check changes.
Stage changes.
Save a checkpoint.
```

### Sharing Work

```bash
git push -u origin feature-login-form
```

Meaning:

```text
Upload my branch to GitHub.
```

### After Review

A team may merge the branch through a GitHub pull request.

Or locally:

```bash
git switch main
git pull
git merge feature-login-form
git push
```

---

## Common Beginner Confusions

### Confusion 1: “I saved my file. Why does Git still need commit?”

Saving a file updates the file on your computer.

Committing saves a Git checkpoint in the project history.

```text
Save = file system change
Commit = Git history checkpoint
```

### Confusion 2: “Does git add upload my code?”

No.

`git add` only stages changes locally.

It does not upload anything.

### Confusion 3: “Does git commit upload my code?”

No.

`git commit` saves a local checkpoint.

Use `git push` to upload commits to GitHub.

### Confusion 4: “Why does GitHub not show my latest work?”

Most likely, you committed locally but did not push.

Run:

```bash
git status
git log --oneline
git push
```

### Confusion 5: “Why did git pull cause a conflict?”

Because your local work and the remote work touched the same area of a file.

Git needs you to decide how to combine them.

### Confusion 6: “What is origin?”

`origin` is just the common nickname for the main remote repository.

It usually points to GitHub.

### Confusion 7: “What is main?”

`main` is commonly the default branch.

Older repos may use `master`.

### Confusion 8: “Why does Git say there is nothing to commit?”

Because Git does not see any unstaged or staged changes that need to be committed.

Run:

```bash
git status
```

Trust status first.

### Confusion 9: “Why can’t I push?”

Common reasons:

- no remote is configured
- wrong remote URL
- SSH key is not set up
- you do not have permission
- your local branch is behind the remote branch
- you need to pull first
- you are on the wrong branch

### Confusion 10: “Why does Git ask me to set upstream?”

When pushing a new local branch for the first time, Git may not know which remote branch it should connect to.

Use:

```bash
git push -u origin branch-name
```

After that, `git push` and `git pull` usually know what to do.

---

## Beginner Safety Rules

### Rule 1: Run `git status` Often

Before and after important commands, run:

```bash
git status
```

This is your dashboard.

### Rule 2: Commit Small, Meaningful Changes

Avoid one giant commit that changes everything.

Better:

```text
Add navbar
Fix mobile layout
Create fetch helper
Update README
```

Worse:

```text
final project
everything
stuff
```

### Rule 3: Pull Before Starting Work

On team projects:

```bash
git switch main
git pull
```

Start from the latest shared version.

### Rule 4: Use Branches

Do not do all work directly on `main` if the team expects branch-based work.

Use:

```bash
git switch -c feature-name
```

### Rule 5: Do Not Copy Random Git Commands Blindly

Some Git commands rewrite history or delete work.

Be careful with commands like:

```bash
git reset --hard
git clean -fd
git push --force
```

Do not use those unless you understand exactly what they do.

### Rule 6: Do Not Commit Secrets

Never commit:

- passwords
- API keys
- private tokens
- `.env` files with real secrets
- SSH private keys

Use `.gitignore` for files that should not be tracked.

### Rule 7: Read the Error Message

Git error messages can be intimidating, but they usually contain clues.

Look for words like:

```text
remote
branch
upstream
conflict
permission denied
not a git repository
```

---

## Practice Lab

This lab walks through a local Git repo.

You can do this in a safe practice folder.

### Step 1: Create a Folder

```bash
mkdir git-practice
cd git-practice
```

### Step 2: Initialize Git

```bash
git init -b main
```

If that does not work:

```bash
git init
git branch -M main
```

### Step 3: Create a README

```bash
echo "# Git Practice" > README.md
```

### Step 4: Check Status

```bash
git status
```

You should see `README.md` as untracked.

### Step 5: Stage the File

```bash
git add README.md
```

### Step 6: Commit the File

```bash
git commit -m "Add README"
```

### Step 7: Create a Branch

```bash
git switch -c add-notes
```

### Step 8: Add Another File

```bash
echo "These are practice notes." > notes.txt
git status
git add notes.txt
git commit -m "Add notes file"
```

### Step 9: Merge the Branch

Switch back to main:

```bash
git switch main
```

Merge:

```bash
git merge add-notes
```

### Step 10: View the History

```bash
git log --oneline
```

### Optional Step: Connect to GitHub

Create an empty GitHub repository.

Then add the remote:

```bash
git remote add origin git@github.com:username/git-practice.git
```

Push:

```bash
git push -u origin main
```

---

## Quick Reference

| Command | Meaning |
|---|---|
| `git --version` | Check Git version |
| `git config --global user.name "Name"` | Set Git username |
| `git config --global user.email "email"` | Set Git email |
| `git init` | Create a Git repo |
| `git init -b main` | Create repo with main as default branch |
| `git status` | Show current repo state |
| `git add file` | Stage one file |
| `git add .` | Stage current folder changes |
| `git commit -m "message"` | Commit staged changes |
| `git log --oneline` | Show compact commit history |
| `git remote -v` | List remotes |
| `git remote add origin URL` | Add a remote |
| `git remote remove origin` | Remove a remote |
| `git remote set-url origin URL` | Change remote URL |
| `git push -u origin main` | First push and set upstream |
| `git push` | Push commits |
| `git pull` | Fetch and integrate remote changes |
| `git fetch` | Fetch remote changes without merging |
| `git branch` | List branches |
| `git branch name` | Create branch |
| `git switch name` | Switch branch |
| `git switch -c name` | Create and switch branch |
| `git merge branch-name` | Merge branch into current branch |
| `git clone URL` | Download a repo |
| `ssh-keygen -t ed25519 -C "email"` | Generate SSH key |
| `ssh -T git@github.com` | Test GitHub SSH connection |

---

## Common Command Flows

### New Local Project to GitHub

```bash
mkdir my-project
cd my-project
git init -b main
echo "# My Project" > README.md
git add .
git commit -m "Initial commit"
git remote add origin git@github.com:username/my-project.git
git push -u origin main
```

### Existing GitHub Repo to Local Computer

```bash
git clone git@github.com:username/repository-name.git
cd repository-name
git status
```

### Daily Solo Workflow

```bash
git status
git add .
git commit -m "Describe the change"
git push
```

### Daily Team Workflow

```bash
git switch main
git pull
git switch -c feature-name
# make changes
git status
git add .
git commit -m "Describe the change"
git push -u origin feature-name
```

### Merge a Finished Branch Locally

```bash
git switch main
git pull
git merge feature-name
git push
```

---

## Further Reading

### Git Official Documentation

The official Git documentation explains every Git command in detail.

Useful topics:

- `git init`
- `git status`
- `git add`
- `git commit`
- `git remote`
- `git push`
- `git pull`
- `git branch`
- `git merge`

### Pro Git Book

The Pro Git book is a free online book that explains Git deeply.

Good beginner chapters:

- Getting Started
- Git Basics
- Git Branching

### GitHub Docs

GitHub Docs are useful for:

- creating repositories
- setting up SSH
- adding SSH keys
- testing SSH connections
- working with branches
- using pull requests

---

## Final Mental Model

Git is not just a list of commands.

Git is a system for managing project history.

Keep this flow in your head:

```text
Working files
   ↓ git add
Staging area
   ↓ git commit
Local Git history
   ↓ git push
GitHub remote repository
```

And when working with teammates:

```text
GitHub remote repository
   ↓ git pull
Your local repository
   ↓ edit files
Working files
   ↓ git add
Staging area
   ↓ git commit
Local Git history
   ↓ git push
GitHub remote repository
```

When lost, run:

```bash
git status
```

That is the first command to reach for.
