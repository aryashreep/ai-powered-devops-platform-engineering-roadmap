# 🧪 Lab: Essential Commands Mastery

> **Goal:** Navigate, create, and inspect a Linux system with confidence.

## Target Objectives

- Move around the filesystem using relative and absolute paths
- Create and delete files and directories
- Read system information
- Use man pages to discover flags

---

## Hands-on Challenges

### 1. Filesystem Navigation
```bash
# Start at / and explore 3 levels deep
cd / && ls -la
# Use tree (install if needed: sudo apt install tree)
tree -L 2 /etc
# Check where you are
pwd
```

### 2. File Operations
Create this structure under `/tmp/devops-foundations/`:
```
devops-foundations/
├── projects/
│   └── hello-world.txt
├── logs/
│   └── app.log
└── scripts/
```
Then copy `hello-world.txt` to `scripts/`, rename it to `hello.sh`, and remove the `logs` directory.

### 3. System Info Report
Collect and save: hostname, kernel version, uptime, current user, available memory.

### 4. Man Page Deep Dive
Run `man ls` and find:
- How to show hidden files
- How to sort by time
- How to list recursively
Write the flags you found.

### 5. Create a Cheatsheet
Create `solution/my-commands-cheatsheet.md` with a table of the 15 most useful commands you used today. Format: | Command | What it does | Example |

---

## Proof of Work

Save in `solution/essential-commands-report.md`:
- Your filesystem tree from Challenge 1
- The final state of `/tmp/devops-foundations/` after Challenge 2
- Your system info report
- The man page flags you discovered

Commit and push.
