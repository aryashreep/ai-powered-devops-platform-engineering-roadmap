# 🧪 Lab: Linux Architecture Exploration

> **Goal:** Map the Linux architecture layers on a running system.

## Target Objectives

- Identify the kernel version and confirm PID 1
- Explore the /proc virtual filesystem and read live kernel data
- Map each command you run to its layer in the Linux onion model

---

## Hands-on Challenges

### 1. Kernel Identity
Run the following and save the output:
```bash
uname -a              # full kernel info: version, hostname, architecture
cat /proc/version     # similar info via /proc — the kernel's own version string
ls /boot/             # see the actual kernel files on disk
```

> **What to look for in `/boot/`:**
> - `vmlinuz-*` — the compressed kernel binary (the actual Linux kernel)
> - `initrd.img-*` — the temporary filesystem used during boot
> - `config-*` — the kernel build settings

> **Mac / JSLinux users:** `/boot/` does not exist on macOS or JSLinux Alpine — skip this command. The kernel files are stored differently on non-Ubuntu systems.

### 2. Explore systemd (PID 1)
```bash
# Confirm PID 1 — shows the process name, its own PID, and its parent PID
ps -p 1 -o pid,ppid,cmd

# What version of systemd is running?
# (systemctl is the service manager tool — covered fully on Day 6)
systemctl --version

# List services currently running — preview of Day 6 content
systemctl list-units --type=service --state=running
```

> **Write down:** the systemd version number, and the names of **5 services** from the list (e.g. `systemd-journald`, `sshd`, `cron`).

### 3. Walk the Onion Model — Layer by Layer
Run each command and write down which layer of the onion model it belongs to:
```bash
# Running applications (outermost layer)
ps aux | head -10

# Shell — the interpreter translating your commands
echo $SHELL

# Kernel — version and how long it has been running
uname -r
cat /proc/uptime

# Kernel live data via /proc — how much RAM?
# (head -5 shows the first 5 lines — look for the MemTotal line)
cat /proc/meminfo | head -5

# Kernel hardware event log
dmesg | tail -20
```

> **Permission note:** On Ubuntu 22.04+ and WSL2, bare `dmesg` may say `Operation not permitted`. Run `sudo dmesg | tail -20` instead.

### 4. User Space vs Kernel Space
These commands show the boundary in action — user space tools reading kernel data:
```bash
# User space tool reading kernel data — CPU info from /proc
cat /proc/cpuinfo | head -20
# Look for: model name (CPU model) and cpu cores (how many cores)

# User space tool reading kernel data — memory from /proc
cat /proc/meminfo | head -5
# Look for: MemTotal (total RAM in kB — divide by 1024 twice to get GB)

# Kernel's own log — only the kernel writes here
dmesg | tail -20   # use sudo dmesg | tail -20 if you see "Operation not permitted"
# If you see lines with "error" or "failed", that is the kernel reporting a problem
```

### 5. Architecture Diagram
Draw a text-based architecture diagram with these 5 layers (innermost to outermost):

```
Hardware → Kernel → System Libraries → Shell → Applications
```

Next to each layer, write:
- 1–2 real components (e.g. "CPU, RAM" for Hardware; "bash, zsh" for Shell)
- 1 command from today that belongs to that layer

> **Note:** System calls are the *mechanism* the libraries use to talk to the kernel — they are not a layer themselves. The layer between the kernel and the shell is **System Libraries (glibc)**.

---

## Proof of Work

Save your findings in `solution/linux-architecture-notes.md`:
- Your `uname -a` output
- The systemd version and 5 running services
- Your layer-by-layer onion exploration output (ps aux, echo $SHELL, uname -r, /proc/uptime, dmesg | tail -20)
- Your text architecture diagram with labels

Commit and push when done.
