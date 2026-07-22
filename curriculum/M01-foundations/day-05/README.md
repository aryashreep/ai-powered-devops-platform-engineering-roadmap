# 🗓️ Day 03 — Text Files & I/O Redirection

Welcome to the data-wrangler's toolkit. Today you'll learn to slice, dice, search, and redirect text — the universal interface of Linux.

## Goal

Read, filter, and transform text files using pipes, redirection, and core text utilities.

## Key Learnings

- Reading files: `cat`, `head`, `tail`, `less`
- Redirection: `>`, `>>`, `<`, `|` (pipe), `tee`
- Text processing: `grep`, `wc`, `sort`, `uniq`, `cut`
- Stream editing: `sed` basics

## Hands-on Lab

👉 **[Complete today's lab](./lab-session/task.md)**

## Why This Matters

Production debugging is text wrangling. Logs, configs, CSVs — every observability tool outputs text. The faster you can filter and transform it, the faster you find the root cause.

## Submission

1. Complete the tasks in `lab-session/task.md`
2. Save your proof of work in `lab-session/solution/`
3. Commit with message: `Day 03 — Text Files & I/O Redirection`

## Learn in Public

Share a one-liner pipeline you built today (e.g., `tail -f log \| grep ERROR \| cut -d' ' -f1`) with **#LearnDevOpsIn90Days**.
