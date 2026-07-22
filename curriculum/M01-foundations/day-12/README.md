# 🗓️ Day 11 — Namespaces & cgroups

Today you'll learn the kernel primitives that make containers possible — namespaces for isolation and cgroups for resource control.

## Goal

Explain how Linux namespaces and cgroups isolate and constrain processes — the technology underpinning Docker, Kubernetes, and every container runtime.

## Key Learnings

- PID, net, mount, user, UTS, IPC namespaces
- cgroups v2 — CPU, memory, and I/O limits
- How Docker uses namespaces + cgroups to create containers
- `nsenter` — enter a process's namespace
- `/sys/fs/cgroup` — cgroup filesystem walkthrough

## Hands-on Lab

👉 **[Complete today's lab](./lab-session/task.md)**

## Why This Matters

Containers are NOT VMs. They're just Linux processes with namespaces and cgroups. Understanding this difference is the most important conceptual leap in modern DevOps — it explains why containers start in ms, why host kernel exploits break all containers, and why cgroup limits prevent noisy-neighbor problems.

## Submission

1. Complete the tasks in `lab-session/task.md`
2. Save your proof of work in `lab-session/solution/`
3. Commit with message: `Day 11 — Namespaces & cgroups`

## Learn in Public

Post your own "Containers are NOT VMs" explanation with **#LearnDevOpsIn90Days** — the best way to learn is to teach.
