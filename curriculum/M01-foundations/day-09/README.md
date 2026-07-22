# 🗓️ Day 08 — Permissions & Ownership

Today you'll master the Linux permission model — who can read, write, and execute what.

## Goal

Set and interpret file permissions using symbolic and octal notation, manage ownership, and use special permission bits.

## Key Learnings

- `chmod` — symbolic (`u+x`, `g-w`) and octal (`755`, `644`)
- `chown` / `chgrp` — change owner and group
- SUID, SGID, and sticky bit
- ACLs for fine-grained control
- `umask` — default permission mask

## Hands-on Lab

👉 **[Complete today's lab](./lab-session/task.md)**

## Why This Matters

The #1 cause of "it works on my machine" is wrong permissions. A world-readable private key, a non-executable script, or a sticky-bitless `/tmp` — each is a security or operational disaster.

## Submission

1. Complete the tasks in `lab-session/task.md`
2. Save your proof of work in `lab-session/solution/`
3. Commit with message: `Day 08 — Permissions & Ownership`

## Learn in Public

Post your permission cheat sheet (octal table + special bits) with **#LearnDevOpsIn90Days**.
