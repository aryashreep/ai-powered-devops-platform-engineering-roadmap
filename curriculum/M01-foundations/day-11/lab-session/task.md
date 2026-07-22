# 🧪 Lab: Troubleshooting Toolkit

> **Goal:** Diagnose production issues using system-level tracing and monitoring tools.

## Target Objectives

- Trace system calls with strace
- Open file tracking with lsof
- Network connection analysis with ss/netstat
- Resource monitoring with vmstat/iostat
- Kernel message debugging with dmesg

---

## Hands-on Challenges

### 1. strace Deep Dive
```bash
# Trace a simple command
strace ls /tmp/
# Count system calls by type
strace -c ls /tmp/
# Trace file operations
strace -e trace=open,openat,read,write ls /tmp/ 2>&1 | head -30
# Trace a running process
sudo strace -p $(pgrep nginx | head -1) -e trace=network -c
```

### 2. Open File Investigation with lsof
```bash
# Who's listening on port 80
sudo lsof -i :80
# All open files by a user
lsof -u $USER
# Which process has /var/log/syslog open
lsof /var/log/syslog
# List all network connections
lsof -i
```

### 3. Network Connections with ss
```bash
# All TCP connections
ss -t
# All listening sockets
ss -tlnp
# Socket statistics
ss -s
# Compare with netstat
netstat -tlnp
```

### 4. Resource Monitoring
```bash
# CPU and I/O stats every 2 seconds
vmstat 2 5
# Disk I/O per device
iostat -x 2 3
# Memory pressure
sudo dmesg | grep -i "oom\|out of memory"
```

### 5. Build a Troubleshooting Runbook
Create `solution/troubleshooting-runbook.md` with a flow chart for:
- "Server is slow" → check CPU/memory/disk/I/O
- "Service won't start" → check logs, ports, systemd status
- "Connection refused" → check firewall, service, port binding

---

## Proof of Work

Save `solution/diagnosis-report.md` with:
- strace output showing file operations
- All processes listening on TCP ports
- vmstat output showing system health
- Your troubleshooting runbook flow
