# 🗓️ Day 14 — DNS

Today you'll learn how the Domain Name System maps names to IPs — and how to troubleshoot it when things break.

## Goal

Trace a DNS resolution from browser to root to TLD to authoritative server, and use `dig` / `nslookup` to debug DNS issues.

## Key Learnings

- DNS resolution chain: resolver → root → TLD → authoritative
- Record types: A, AAAA, CNAME, MX, TXT, NS
- `dig` / `nslookup` — DNS troubleshooting tools
- `/etc/hosts` — local override
- Caching and TTL

## Hands-on Lab

👉 **[Complete today's lab](./lab-session/task.md)**

## Why This Matters

DNS is the most common root cause of "it doesn't work" — misconfigured records, slow propagation, DNS hijacking. Knowing how to check every record type and trace the resolution path is a superpower.

## Submission

1. Complete the tasks in `lab-session/task.md`
2. Save your proof of work in `lab-session/solution/`
3. Commit with message: `Day 14 — DNS`

## Learn in Public

Share your favorite `dig` one-liner with **#LearnDevOpsIn90Days** (e.g., `dig +short MX gmail.com`).
