# 🧪 Lab: Process Management

> **Goal:** Master process inspection, signaling, and prioritization.

## Target Objectives

- Use ps/top to inspect running processes
- Send signals safely
- Identify and handle zombie processes
- Adjust process priority

---

## Hands-on Challenges

### 1. Process Inventory
```bash
# Full process list with details
ps aux
# Process tree
ps auxf
# Find all processes by a specific user
ps -u root
```

### 2. Top/htop Exploration
Run `top` and identify:
- Total processes, running vs sleeping
- CPU and memory breakdown
- The top 3 CPU-consuming processes
- Sort by memory (press `M` in top)

### 3. Signal Practice
```bash
# Start a long-running process in background
sleep 300 &
# Send SIGTERM (graceful)
kill <PID>
# Start another, send SIGKILL (force)
sleep 300 &
kill -9 <PID>
# Send SIGHUP (reload config)
kill -HUP <PID>
```

### 4. Priority Management
```bash
# Start with low priority
nice -n 19 sleep 300 &
# Change priority of a running process
renice -n 10 -p <PID>
# Verify
ps -o pid,ni,cmd -p <PID>
```

### 5. Zombie Detection
Write a small C script or Python script that creates a zombie process, or analyze via `ps aux | grep Z` — document what zombies look like and how to clean them.

---

## Proof of Work

Save `solution/process-mgmt-report.md` with:
- Your `ps aux` output (head -20)
- Top 3 CPU-consuming processes from top
- Demo of SIGTERM vs SIGKILL behavior
- A process priority table
- What zombies look like and how to handle them
