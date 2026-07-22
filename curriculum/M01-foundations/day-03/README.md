# 🗓️ Day 04 — File System Hierarchy (FHS)

Today you'll learn the Linux filesystem layout — where everything lives and why.

## Goal

Navigate the Linux filesystem hierarchy with purpose: know what `/etc`, `/var`, `/proc`, `/sys`, `/tmp`, and `/usr` are for.

## Key Learnings

- `/etc` — system configuration files
- `/var` — variable data (logs, queues, spools)
- `/proc` & `/sys` — virtual filesystems for kernel data
- `/tmp` — temporary files
- `/usr`, `/opt`, `/lib` — user programs and libraries

## Hands-on Lab

👉 **[Complete today's lab](./lab-session/task.md)**

## Why This Matters

When troubleshooting, knowing where to look saves hours. Database data → `/var/lib/mysql`. Configs → `/etc`. Kernel info → `/proc`. Understanding FHS is like having a map of the production server.

## Submission

1. Complete the tasks in `lab-session/task.md`
2. Save your proof of work in `lab-session/solution/`
3. Commit with message: `Day 04 — File System Hierarchy`

## Learn in Public

Post your version of the FHS tree map on LinkedIn with **#LearnDevOpsIn90Days**.
