---
module: "M01"
unit: "Linux internals"
lesson: "Essential commands"
day: 2
status: draft            # draft | reviewed | recorded | published
est_minutes: 45          # Story(2) + Problem(3) + Visualization(2) + Theory(5) + Demo(13) + Mistakes(5) + Interview(5) + Assignment(10)
---

# Essential Linux commands toolkit

> **Learning goal:** Navigate the filesystem, create and manipulate files, inspect system health, and use help commands to self-serve any new tool.

---

## Story (2 min)

### The Day Golu Lost His Files

**Characters:** Golu (junior) and Jagu (senior)

It was Golu's third day on the job. He SSHed into production and needed to clean up temp files. He typed what he thought was the right command:

```bash
rm -rf *
```

But he was in `/etc` — not `/tmp`. He had never typed `pwd` to check his location. The config files for nginx, SSH, and cron were gone. The server was unrecoverable.

> Just 5 commands cover 80% of navigation: `pwd` tells you WHERE you are. `ls` tells you WHAT is there. Never run a destructive command without checking both first.

---

## Real-world Problem (3 min)

### What Breaks Without Command Fluency?

- **Disk full:** You don't know `df -h` — the app silently crashes when disk hits 100%
- **Wrong directory:** You run `rm -rf *` without `pwd` — delete critical configs instead of temp files
- **Lost in filesystem:** You can't find log files, config files, or binaries quickly
- **Can't self-serve:** You Google every flag instead of using `man` or `--help` — 10x slower

### Topic Flow

```
Problem → Story → Visualization → Theory → Hands-on → Best Practice → Interview → Assignment
  │        │          │             │         │            │             │           │
  v        v          v             v         v            v             v           v
  Lost    rm -rf     Filesystem   4 command  4 demo      Always        pwd,       Build &
  in      in /etc    tree map    groups     steps       pwd + ls     rm -rf,     explore
  server                         + tables              first         symlinks    lab tree
```

---

## Visualization (2 min)

### Filesystem Map — Your Mental GPS

Whiteboard drawing:

```
/ (root)
├── /etc/        → Config files (nginx.conf, sshd_config, hosts)
├── /var/log/    → Log files (syslog, app logs, auth.log)
├── /home/       → User home directories (/home/golu/)
├── /tmp/        → Temp files (cleared on reboot)
├── /proc/       → Virtual FS — kernel state (cpuinfo, meminfo)
└── /dev/        → Device files (sda, tty, null)
```

**Mental model:** Think of Linux filesystem as a tree. You are a bird sitting on a branch. `pwd` tells you which branch. `cd` moves you to another branch. `ls` shows you the leaves and smaller branches around you.

---

## Theory (5 min)

### The 4 Command Groups

#### Group 1 — Navigation

| Command | What it does | Key flags |
|---------|-------------|-----------|
| `pwd` | Print Working Directory — shows exact current path | `-L` (logical), `-P` (physical) |
| `cd /path` | Change Directory | `cd ~` (home), `cd -` (previous), `cd ..` (up) |
| `ls` | List directory contents | `-la` (all + long), `-lh` (human sizes), `-lt` (by time) |
| `tree` | Visual directory tree | `-L 2` (depth limit), `-d` (dirs only) |

```bash
# Where am I?
pwd

# What's here? (always use -la)
ls -la

# Navigate
cd /etc
tree -L 2
```

#### Group 2 — File Operations

| Command | What it does | Key flags |
|---------|-------------|-----------|
| `mkdir dir` | Make directory | `-p` (parent dirs, no error if exists) |
| `touch file` | Create file or update timestamp | `-t` (specific timestamp) |
| `cp src dst` | Copy | `-r` (recursive dirs), `-i` (interactive), `-v` (verbose) |
| `mv src dst` | Move / Rename | `-i` (interactive), `-n` (no overwrite) |
| `rm file` | Remove | `-r` (recursive), `-f` (force), `-i` (interactive) |
| `find /path` | Search files by name/size/date | `-name`, `-size +100M`, `-mtime -7` |
| `ln -s src dst` | Symbolic link | `-s` (soft link) |

```bash
# Create nested dirs in one shot
mkdir -p ~/devops-lab/m00/u01/{configs,scripts,logs}

# Copy with interactive prompt (SAFE)
cp -i config.yaml config.yaml.bak

# Find files > 100MB
find / -size +100M -type f 2>/dev/null

# Create symlink
ln -s /etc/nginx/nginx.conf ~/nginx.conf
```

#### Group 3 — System Info

| Command | What it shows |
|---------|--------------|
| `df -h` | Disk free per filesystem (human readable) |
| `du -sh *` | Disk usage per file/dir (summary) |
| `free -h` | RAM and swap usage |
| `top` / `htop` | Live process monitor |
| `uptime` | Load averages — 1, 5, 15 min |
| `whoami` | Current logged-in user |
| `hostname -I` | All IP addresses of this machine |

```bash
# These 5 commands = your first 60 seconds on any server
whoami            # Who am I?
hostname          # What server is this?
uptime            # Load average OK?
df -h             # Disk space OK?
free -h           # Memory OK?
```

#### Group 4 — Getting Help

| Tool | Best for |
|------|----------|
| `man command` | Full manual — comprehensive reference |
| `command --help` | Quick inline summary (always available) |
| `info command` | GNU info pages (more detail than man) |
| `tldr command` | Practical examples (install: `sudo apt install tldr`) |

```bash
# Pro tip: search man pages by keyword
man -k "disk usage"
```

---

## Live Demo (13 min)

### Step 1 — Explore Where You Are

```bash
pwd                   # Where am I?
ls -la                # What's here? (including hidden files)
ls -lh /etc | head -20  # Peek at /etc with human-readable sizes
```

### Step 2 — Build Your Sandbox

```bash
mkdir -p ~/devops-lab/m00/u01/{configs,scripts,logs}
cd ~/devops-lab/m00/u01
touch configs/app.conf scripts/deploy.sh logs/app.log
tree ~/devops-lab
```

### Step 3 — Practice File Operations Safely

```bash
cp -i configs/app.conf configs/app.conf.bak
mv logs/app.log logs/app_2026.log
ls -la configs/ logs/

# Create a symlink
ln -s ~/devops-lab/m00/u01/configs/app.conf ~/app.conf
ls -la ~ | grep app.conf
```

### Step 4 — System Health Check Routine

```bash
# First 60 seconds on any server
whoami
hostname
uptime
df -h
free -h
top -bn1 | head -15
```

---

## Common Mistakes (5 min)

### Mistake 1: The `rm -rf` Catastrophe

**Wrong:**
```bash
rm -rf /    # Deletes EVERYTHING — entire filesystem!
```

**Right:**
```bash
pwd                         # Check where you are FIRST
ls -la                      # See what will be deleted
rm -rf /path/to/specific/dir  # Use absolute path
```

### Mistake 2: Silent Overwrites with `cp`

**Wrong:**
```bash
cp new.conf /etc/app/config.conf   # Silently overwrites existing file
```

**Right:**
```bash
cp -i new.conf /etc/app/config.conf  # -i asks "overwrite?" first
```

### Mistake 3: `ls` Without Flags

**Wrong:**
```bash
ls    # Shows only visible files. Misses .env, .bashrc, .ssh/!
```

**Right:**
```bash
ls -la  # ALL files including hidden, with permissions and sizes
```

---

## Interview Questions (5 min)

1. **Q: What does `pwd` stand for and when do you use it?**
   **A:** Print Working Directory. Use it constantly — before destructive commands, before writing files, and in shell scripts to validate the execution directory.

2. **Q: What's the difference between `rm` and `rm -rf`?**
   **A:** `rm file` deletes a single file. `rm -rf dir` adds `-r` (recursive — deletes directories) and `-f` (force — never prompts). Together they're extremely dangerous. Always verify with `pwd` + `ls` first.

3. **Q: How do you find files larger than 100MB?**
   **A:** `find / -size +100M -type f 2>/dev/null`. Breakdown: search from root, larger than 100MB, files only, suppress permission errors.

4. **Q: Hard link vs soft link?**
   **A:** Hard link (`ln src dst`) — another entry pointing to same inode (data). Can't cross filesystems. Soft link (`ln -s src dst`) — shortcut file containing path to target. Can cross filesystems, becomes dangling if original is deleted.

---

## Assignment (10 min)

**Task: Build + Inspect a DevOps Lab Tree**

1. Create `~/devops-lab/m00/u01/` with at least 5 files across configs/, scripts/, logs/
2. Run `ls -la` on each subdirectory
3. Run `df -h` and note total disk + available space
4. Run `free -h` and note total RAM + swap
5. Run `find ~/devops-lab -type f | wc -l` — count total files
6. Create a file `~/devops-lab/top-commands.txt` listing the 10 commands you'll use most

**You know you're done when:** You have a fully structured lab directory, can navigate it confidently, and know your VM's disk/memory specs.

---
---

## Today's Takeaway
> Master 20 Linux commands and you can handle 80% of daily DevOps tasks. Command fluency = faster incident response.

## Today's Interview Question
> "How would you find the 5 largest files on a Linux server?" — `find / -type f -size +100M -exec ls -lh {} \; | sort -k5 -rh | head -5`

## Today's Assignment
> Build a DevOps lab directory tree, practice `cp -i` and `mv`, run health check commands.

## Today's Challenge
> Time yourself: how fast can you create a nested directory structure, symlink a config, and run a full system health check?

## Today's Mistake
> Never run `rm -rf` without first running `pwd` and `ls`. One wrong directory and configs are gone forever.

## Today's Pro Tip
> Make `ls -la` and `pwd` a reflex — type them before ANY destructive command. Add `alias ll='ls -la'` to your `~/.bashrc`.
