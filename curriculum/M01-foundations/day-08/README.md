# 🗓️ Day 07 — User & Group Management

Today you'll learn to manage identities on a Linux system — a fundamental skill for access control and security.

## Goal

Create, modify, and delete users and groups; understand the password and shadow files; configure sudo access.

## Key Learnings

- `useradd` / `usermod` / `userdel` — user lifecycle
- `groupadd` / `gpasswd` — group management
- `/etc/passwd`, `/etc/shadow`, `/etc/group` — the identity files
- `passwd`, `su`, `sudo` — authentication and switching
- Sudoers configuration via `visudo`

## Hands-on Lab

👉 **[Complete today's lab](./lab-session/task.md)**

## Why This Matters

Multi-user Linux is the norm. Creating service accounts, managing team access, and auditing who can sudo are daily tasks for any platform engineer. Misconfigured sudo = security incident waiting to happen.

## Submission

1. Complete the tasks in `lab-session/task.md`
2. Save your proof of work in `lab-session/solution/`
3. Commit with message: `Day 07 — User & Group Management`

## Learn in Public

Share 3 user management best practices with **#LearnDevOpsIn90Days** (e.g., why service accounts shouldn't have login shells).
