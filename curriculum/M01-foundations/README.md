# M01 — Foundations

**Duration:** 25 days (4 hrs/day recommended)  
**Cert alignment:** Linux / networking / Git baseline  
**LF Reference:** Aligned with Linux Foundation LFS101 (Introduction to Linux)

---

## 🎯 Learning Outcomes

By the end of this module you will be able to:

- Operate confidently at a Linux shell — navigate, manage processes, troubleshoot
- Edit server files with vim and nano without panic
- Explain what a container IS at the kernel level (namespaces + cgroups)
- Reason about a network path end-to-end — from DNS to TCP to TLS
- Use Git beyond the happy path — rebase, sign commits, resolve conflicts

---

## 📚 Module Structure (25 Days)

### 🐧 Linux Internals (Day 01–13) — Aligned with LFS101

| Day | Ch | LF Ref | Topic | Lesson | Status |
|-----|----|--------|-------|--------|--------|
| 01 | 1-2 | LFS101 Ch 1-2 | The Linux Foundation & Architecture | Kernel vs user space, onion model, systemd, LF history | ✅ Restructured |
| 02 | 7-8 | LFS101 Ch 7-8 | Command Line Ops & Documentation | Navigation, file ops, man pages | 📋 Original |
| 03 | 10 | LFS101 Ch 10 | File System Hierarchy & File Ops | FHS, /etc, /var, /proc, /sys | 📋 Original |
| 04 | 11 | LFS101 Ch 11 | **Text Editors (vim & nano)** | vim modes, nano basics, sudoedit | 🆕 New |
| 05 | 13 | LFS101 Ch 13 | Manipulating Text & I/O Redirection | grep, sed, pipes, redirection, tee | 📋 Original |
| 06 | 3 | LFS101 Ch 3 | Linux Basics & System Startup | UEFI → GRUB2 → kernel → initramfs → systemd | 📋 Original |
| 07 | 9 | LFS101 Ch 9 | Processes | Lifecycle, ps/top, signals, zombies | 📋 Original |
| 08 | 12 | LFS101 Ch 12 | User Environment | User & group management, env vars | 📋 Original |
| 09 | 18 | LFS101 Ch 18 | Local Security Principles | Permissions, chmod/chown, sudo, ACLs | 📋 Original |
| 10 | 14 | LFS101 Ch 14 | Network Operations | SSH, key auth, tunneling, networking | 📋 Original |
| 11 | — | — | Diagnosis & Troubleshooting | strace, lsof, ss, vmstat, dmesg | 📋 Original |
| 12 | — | — | Namespaces & cgroups | Container primitives — PID/net/mnt NS | 📋 Original |
| 13 | — | — | Filesystems & LVM | ext4/xfs, mount/fstab, LVM | 📋 Original |

### 🌐 Networking (Day 14–18)

| Day | Topic | Lesson |
|-----|-------|--------|
| 14 | Models & Addressing | OSI/TCP-IP, encapsulation, CIDR, subnets, NAT |
| 15 | DNS | Resolution chain, dig, record types |
| 16 | Transport | TCP/UDP, handshake, ports, states, tcpdump |
| 17 | HTTP & TLS | Methods, status codes, headers, curl, TLS handshake |
| 18 | Edge & eBPF | L4/L7 LB, CDNs, iptables, eBPF fundamentals |

### 💻 Languages & Tooling (Day 19–21)

| Day | Topic | Lesson |
|-----|-------|--------|
| 19 | The One-Language Rule | Bash vs Python vs Go |
| 20 | Bash for Ops | Production scripts, error handling |
| 21 | Python for Ops | APIs, automation, subprocess |

### 🔧 Git Deep (Day 22–25) — Aligned with [GitHub Foundations](https://learn.microsoft.com/en-us/credentials/certifications/github-foundations/)

| Day | Topic | Lesson | GitHub Foundations Domain |
|-----|-------|--------|--------------------------|
| 22 | Object Model | Blobs, trees, commits, refs | Domain 1 — Introduction to Git |
| 23 | Branching & History | Merges, rebase, conflicts | Domain 2 — Working with Repos |
| 24 | Hygiene | Conventional commits, hooks, stash | Domain 3 — Collaborate (PRs) |
| 25 | Signed Commits | GPG, signing, verification | Domain 6 — Security & Admin |

---

## 📖 Lesson Format

Each day follows the **45-minute framework**:

| Section | Time | Purpose |
|---------|------|---------|
| 🏛️ Foundation Context | 3 min | LF industry context (where applicable) |
| Story | 2 min | Hook — real outage or incident with Golu & Jagu |
| Real-world Problem | 3 min | What breaks without this knowledge |
| Visualization | 2 min | Diagrams & mental models |
| Theory | 5 min | Core concepts, commands, tables |
| Live Demo | 15 min | Instructor types, you follow on your VM |
| Common Mistakes | 5 min | Wrong way → right way |
| Interview Questions | 5 min | 3–5 Q&As with model answers |
| Lab Assignment | 10 min | Hands-on task with acceptance criteria |

**Every day includes:** Hands-on lab, reference solution, "Learn in Public" prompt, and a 🔥 YouTube Challenge for video viewers.

---

## 🗺️ LFS101 Mapping Reference

| LFS101 Chapter | Our Day(s) |
|----------------|------------|
| Ch 1: The Linux Foundation | Day 01 |
| Ch 2: Linux Philosophy & Concepts | Day 01 |
| Ch 3: Linux Basics & System Startup | Day 06 |
| Ch 7: Command Line Operations | Day 02 |
| Ch 8: Finding Linux Documentation | Day 02 |
| Ch 9: Processes | Day 07 |
| Ch 10: File Operations | Day 03 |
| Ch 11: Text Editors | Day 04 🆕 |
| Ch 12: User Environment | Day 08 |
| Ch 13: Manipulating Text | Day 05 |
| Ch 14: Network Operations | Day 10, 14-18 |
| Ch 18: Local Security Principles | Day 09 |

---

## 🛠 Prerequisites

- A Linux VM (Ubuntu 22.04 LTS recommended) or WSL2
- Basic comfort with a terminal
- `git`, `curl`, `gcc` (or build-essential) installed

---

## 📋 Quick Reference

| Resource | Link |
|----------|------|
| Complete cheat sheet | [CHEATSHEET.md](./CHEATSHEET.md) |
| End-of-module exam | [mastery-exam/](./mastery-exam/) |
| Lesson template | [LESSON_TEMPLATE.md](../LESSON_TEMPLATE.md) |
| Lecture framework | [LECTURE-FRAMEWORK.md](./LECTURE-FRAMEWORK.md) |

---

## 💡 Analogy Anchor

**Linux = your house** (root = master key, /etc = house manual, /var/log = CCTV, SSH = remote control)  
**Networking = the city postal system** (IP = address, DNS = contacts app, port = apartment number)

## ⚠️ Teach-the-Trap

Learners think containers are VMs. **Hammer that a container is just a Linux process with namespaces + cgroups.**
