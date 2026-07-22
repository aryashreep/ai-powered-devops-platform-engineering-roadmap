# 🗓️ Day 15 — Transport (TCP & UDP)

Today you'll dive into the transport layer — TCP for reliability and UDP for speed.

## Goal

Explain how TCP and UDP work, analyze connections with tools, and understand three-way handshakes, sequence numbers, and flow control.

## Key Learnings

- TCP: connection-oriented, reliable, in-order delivery
- UDP: connectionless, best-effort, low latency
- Three-way handshake (SYN → SYN-ACK → ACK)
- Ports, sockets, and connection states
- `tcpdump` / `tshark` — packet analysis

## Hands-on Lab

👉 **[Complete today's lab](./lab-session/task.md)**

## Why This Matters

Slow connections, dropped packets, port conflicts — every transport-layer issue has a TCP-centric fix. Understanding the handshake explains half your connectivity problems.

## Submission

1. Complete the tasks in `lab-session/task.md`
2. Save your proof of work in `lab-session/solution/`
3. Commit with message: `Day 15 — Transport`

## Learn in Public

Post your TCP handshake diagram with **#LearnDevOpsIn90Days** and explain what "TIME_WAIT" means.
