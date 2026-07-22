# 🗓️ Day 06 — Processes

Today you'll learn to think like the process scheduler — understand how Linux runs, tracks, and kills processes.

## Goal

Manage processes: list, monitor, signal, and prioritize running programs.

## Key Learnings

- Process lifecycle: fork → exec → run → exit
- `ps`, `top`, `htop` — process inspection
- Signals: SIGTERM, SIGKILL, SIGSTOP, SIGHUP
- Process states: running, sleeping, zombie, stopped
- nice/renice — CPU priority

## Hands-on Lab

👉 **[Complete today's lab](./lab-session/task.md)**

## Why This Matters

When an application hangs or consumes 100% CPU, you need to identify the PID, understand what it's doing, and terminate it safely. Every on-call engineer uses these tools weekly.

## Submission

1. Complete the tasks in `lab-session/task.md`
2. Save your proof of work in `lab-session/solution/`
3. Commit with message: `Day 06 — Processes`

## Learn in Public

Share your `ps aux` cheatsheet with **#LearnDevOpsIn90Days** — especially the difference between SIGTERM and SIGKILL.
