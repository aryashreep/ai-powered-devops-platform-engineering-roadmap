# 📗 Chapter 1: The Linux Foundation & Linux Architecture

**Day 01** · Aligned with LFS101 Chapters 1-2

Welcome to Chapter 1 of your Linux Foundations journey! Today you'll learn:
- **Who created Linux** and why it runs 96% of the cloud
- **The Linux Foundation** — the nonprofit behind the kernel
- **The onion model** — hardware → kernel → system libraries → shell → applications
- **Why architecture knowledge** makes you a better debugger

---

## 🎯 Goal

Build a mental model of Linux architecture — from the kernel at the center to the shell you'll live in — so you can reason about where any production issue lives.

## 🧠 Key Learnings

| # | Topic | LF Reference |
|---|-------|-------------|
| 1 | Linux kernel vs distribution — the most common beginner confusion | LFS101 Ch 1 |
| 2 | The Linux Foundation — history, open source, why Linux matters | LFS101 Ch 1 |
| 3 | Linux Philosophy — kernel vs user space, the onion model | LFS101 Ch 2 |
| 4 | systemd as PID 1 — the first process the kernel starts | — |
| 5 | /proc virtual filesystem — kernel's live data window | — |

## 📖 Full Lesson Content

📖 **[Ch 1: Linux Foundation & Architecture](./L01-linux-architecture.md)** — Full lesson with terminal setup guide, Story, Why Linux context, Visualization (onion model), Theory, Live Demo, Common Mistakes, Interview Questions, and Assignment.

📺 **[Interactive HTML Version](./L01-linux-architecture.html)** — Blackboard-style with Golu & Jagu dialogues, tabbed lesson/cheatsheet/lab.

## 🧪 Hands-on Lab

👉 **[Complete today's lab](./lab-session/task.md)**

## 🎬 For YouTube Viewers

**Comment prompt:** "Type 'Onion 🧅' in the comments if you drew the diagram! And tell me — what was your kernel version? Did anything from `dmesg` surprise you?"

## 🔗 LF Alignment

This lesson maps to **Linux Foundation LFS101 Chapters 1-2**:
- **Ch 1: The Linux Foundation** — History, open source philosophy, Linux's role in the cloud
- **Ch 2: Linux Philosophy & Concepts** — Kernel vs OS, onion model, system calls

## 💡 Why This Matters

Every production issue — a crash, a bottleneck, a security breach — traces back to the kernel or user-space boundary. Knowing the architecture lets you reason about **which layer** the problem is in before you start typing commands.

## 📝 Submission

1. Complete the tasks in `lab-session/task.md`
2. Save your proof of work in `lab-session/solution/`
3. Commit with message: `Day 01 — Linux Foundation & Architecture`

## 📣 Learn in Public

Share your architecture diagram on LinkedIn with **#LearnDevOpsIn90Days** and tag what surprised you about how the kernel interacts with user space!
