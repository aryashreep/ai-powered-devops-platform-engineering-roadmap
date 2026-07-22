# 🗓️ Day 21 — Git Object Model

Welcome to Git Deep! Today you'll learn what Git actually stores — blobs, trees, commits, and refs.

## Goal

Explain Git's internal data model: what a commit IS (snapshot + metadata), how objects are stored, and how branches are just pointers.

## Key Learnings

- Git objects: blobs, trees, commits, annotated tags
- What a commit actually contains (tree, parent, author, message)
- The `.git` directory structure
- `git hash-object`, `git cat-file` — plumbing commands
- Content-addressable storage (SHA-1 hashes)

## Hands-on Lab

👉 **[Complete today's lab](./lab-session/task.md)**

## Why This Matters

Most Git mistakes come from misunderstanding its data model. Once you understand that branches are just pointers and commits are snapshots, rebasing vs merging makes sense.

## Submission

1. Complete the tasks in `lab-session/task.md`
2. Save your proof of work in `lab-session/solution/`
3. Commit with message: `Day 21 — Git Object Model`

## Learn in Public

Explain what a Git commit actually IS (not what it does) with **#LearnDevOpsIn90Days**.
