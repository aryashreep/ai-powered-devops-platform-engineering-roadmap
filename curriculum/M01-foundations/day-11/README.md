# 🗓️ Day 10 — Diagnosis & Troubleshooting

Today you'll build the diagnostic toolkit every production engineer needs — strace, lsof, ss, and friends.

## Goal

Trace system calls, inspect open files, analyze network connections, and diagnose resource issues.

## Key Learnings

- `strace` / `lsof` — trace system calls and open files
- `netstat` / `ss` — socket and network diagnostics
- `vmstat` / `iostat` — CPU, memory, and I/O monitoring
- `dmesg` — kernel message buffer
- `/proc` deep-dive for real-time data

## Hands-on Lab

👉 **[Complete today's lab](./lab-session/task.md)**

## Why This Matters

When a service is slow, won't start, or drops connections, these tools tell you what's actually happening. They're the difference between guessing and knowing.

## Submission

1. Complete the tasks in `lab-session/task.md`
2. Save your proof of work in `lab-session/solution/`
3. Commit with message: `Day 10 — Diagnosis & Troubleshooting`

## Learn in Public

Share one "holy $#!%" moment where strace or lsof saved you with **#LearnDevOpsIn90Days**.
