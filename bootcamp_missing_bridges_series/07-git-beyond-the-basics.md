# 07. Git Beyond the Basics

## Stop memorizing commands and start reading repository state

Git becomes much easier when you stop treating it as a collection of commands.

The central question is:

```text
What state is my repository in right now?
```

---

# 1. Three Important Areas

A practical beginner model:

```text
WORKING TREE
    ↓ git add
STAGING AREA
    ↓ git commit
REPOSITORY HISTORY
```

## Working Tree

Files as they currently exist on disk.

## Staging Area

The snapshot you are preparing for the next commit.

## Repository History

Snapshots already committed.

This explains why:

```bash
git add
```

and:

```bash
git commit
```

are separate operations.

---

# 2. `git status` Is Your Dashboard

Before doing something destructive or confusing:

```bash
git status
```

It can tell you:

```text
current branch
modified files
staged changes
untracked files
merge state
rebase state
```

Inspect before acting.

---

# 3. Commits Are Snapshots

A commit is not merely a bag of changed lines.

Conceptually it records:

```text
project snapshot
metadata
parent commit(s)
```

Git can calculate differences between snapshots.

---

# 4. Branches Are Movable Names

Start:

```text
A -- B -- C
          ↑
        main
```

Create `feature`:

```text
A -- B -- C
          ↑
        main
          ↑
       feature
```

Make a feature commit:

```text
A -- B -- C
          ↑
        main
           \
            D
            ↑
         feature
```

The branch name moved to point at the new commit.

---

# 5. HEAD Means "Where Am I?"

Usually:

```text
HEAD → current branch → current commit
```

Example:

```text
HEAD → feature → D
```

Understanding HEAD makes many Git messages much less mysterious.

---

# 6. Why Uncommitted Changes Can Follow You Between Branches

Uncommitted changes belong to the working tree.

They are not automatically owned by the branch you happened to be on when you typed them.

If Git can safely switch branches without overwriting those changes, the changes may remain visible.

This explains a common surprise:

```text
"I changed this on feature.
Why can I still see it after switching to main?"
```

Because the change was never committed into feature's history.

It was still working-tree state.

---

# 7. `restore`, `reset`, and `revert` Are Different Ideas

## `git restore`

Often used to restore file content or adjust staging.

Mental model:

```text
change file/staging state
```

## `git reset`

Moves a branch/HEAD relationship and can also affect staging or files depending on mode.

Mental model:

```text
move a history pointer,
possibly synchronize other areas
```

## `git revert`

Creates a new commit that reverses an earlier commit.

Mental model:

```text
preserve old history
and add an undo commit
```

These commands solve different problems.

---

# 8. Merge

Suppose:

```text
A -- B -- C
      \
       D -- E
```

A merge combines histories.

Sometimes Git can fast-forward.

Sometimes a merge commit is needed.

The important question is:

```text
Do these branches contain independent history that must be combined?
```

---

# 9. Rebase

Before:

```text
A -- B -- C
      \
       D -- E
```

After rebasing feature onto C:

```text
A -- B -- C -- D' -- E'
```

The changes from D and E are replayed as new commits.

That means the commit identities change.

This is why rebasing history other people already depend on deserves care.

---

# 10. Merge Conflicts

A conflict means Git cannot safely choose the correct combined content.

Git is effectively saying:

```text
I know both sides changed this area.
A human must decide the final result.
```

Typical process:

```text
inspect conflict
edit final content
stage resolved file
continue merge/rebase
```

A conflict is not Git being broken.

It is Git refusing to invent intent.

---

# 11. Stash

Stash temporarily records working changes so you can get a cleaner working tree.

Useful when you need to switch context.

But do not let stash become long-term storage you forget about.

---

# 12. Detached HEAD

Normally:

```text
HEAD → branch → commit
```

Detached:

```text
HEAD → commit
```

You can inspect and even create commits, but there is no current branch name moving with you.

If you create valuable work there, create a branch so the commits remain easy to find.

---

# 13. Remotes

Common names:

```text
origin
upstream
```

They are conventional labels for remote repositories.

`origin` is not a magical GitHub concept.

It is just a configured remote name.

---

# 14. Remote-Tracking Branches

Examples:

```text
origin/main
origin/feature
```

These are your local repository's records of where remote branches were when you last fetched relevant information.

They are not the remote server itself.

---

# 15. Pull Is a Combination Operation

Conceptually:

```text
git pull
≈
git fetch
+
integrate fetched changes
```

The integration may be a merge or rebase depending on configuration and command options.

Understanding that decomposition makes `pull` easier to reason about.

---

# 16. Inspect History Visually

A very useful command:

```bash
git log --oneline --graph --decorate --all
```

This helps you see:

```text
commits
branch pointers
HEAD
merges
remote-tracking refs
```

---

# 17. Git Recovery Starts With Observation

When something feels wrong, avoid immediately stacking more commands.

Start with:

```bash
git status
git branch
git log --oneline --graph --decorate --all
```

Then decide what state you actually want.

---

# State-First Git Checklist

```text
What branch am I on?
What does HEAD point to?
Are there staged changes?
Are there unstaged changes?
Are there untracked files?
Am I in a merge?
Am I in a rebase?
Is this history already shared?
Do I want to change:
  files,
  staging,
  history,
  or a branch pointer?
```

Then choose the command.

---

# Final Mental Model

Git becomes much easier when commands become state transitions:

```text
inspect current state
        ↓
decide desired state
        ↓
choose command
        ↓
inspect state again
```
