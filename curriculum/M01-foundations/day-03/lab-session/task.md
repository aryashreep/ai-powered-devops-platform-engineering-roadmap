# 🧪 Lab: Filesystem Hierarchy Explorer

> **Goal:** Build a mental map of the Linux filesystem.

## Target Objectives

- Explore each major FHS directory
- Understand what kind of data lives where
- Read configuration files
- Explore virtual filesystems

---

## Hands-on Challenges

### 1. Map the FHS
```bash
ls -la /
ls -la /etc | head -20
ls -la /var | head -20
ls -la /usr | head -20
```
Create a tree diagram labeling: what each top-level directory stores.

### 2. Explore /etc (Configuration)
List all `.conf` files in `/etc`. Pick one (e.g., `/etc/ssh/sshd_config`) and identify 3 key settings.

### 3. Explore /var (Variable Data)
```bash
ls -la /var/log/
ls -la /var/spool/
du -sh /var/log/*
```
Find the largest log file on the system.

### 4. Explore /proc (Virtual FS)
```bash
cat /proc/cpuinfo | head -10
cat /proc/meminfo | head -5
cat /proc/uptime
ls /proc/ | head -20  # These are all running PIDs!
```

### 5. Create an FHS Reference Card
In `solution/fhs-reference.md`, create a table:

| Directory | Purpose | Notable contents |
|-----------|---------|------------------|
| /etc | Config files | sshd_config, nginx.conf, hosts |
| /var | Variable data | log/, lib/mysql, spool/ |
| ... | ... | ... |

Include at least 10 directories.

---

## Proof of Work

Save `solution/fhs-exploration-report.md` with your FHS tree, config analysis, largest log file, and `/proc` findings.
