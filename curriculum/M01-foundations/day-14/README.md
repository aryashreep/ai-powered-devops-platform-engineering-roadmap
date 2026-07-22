# 🗓️ Day 13 — Networking Models & Addressing

Welcome to the networking section! Today you'll learn the mental model you need to reason about any network path.

## Goal

Explain the OSI and TCP/IP models, understand IP addressing (CIDR, subnets, private ranges), and trace network encapsulation.

## Key Learnings

- OSI 7-layer vs TCP/IP 4-layer model
- Encapsulation: data → segments → packets → frames → bits
- IP addressing, CIDR notation (`/24`, `/16`), subnets
- Private IP ranges (10.x, 172.16-31.x, 192.168.x)
- NAT — how private IPs reach the internet

## Hands-on Lab

👉 **[Complete today's lab](./lab-session/task.md)**

## Why This Matters

Every connection — from `curl google.com` to a Kubernetes pod-to-pod communication — follows the same model. Understanding layering lets you pinpoint where a problem is: DNS? TCP? IP routing? TLS?

## Submission

1. Complete the tasks in `lab-session/task.md`
2. Save your proof of work in `lab-session/solution/`
3. Commit with message: `Day 13 — Models & Addressing`

## Learn in Public

Post your TCP/IP vs OSI cheat sheet with **#LearnDevOpsIn90Days**.
