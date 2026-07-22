# Day 06 — Reference Solution

## Key Process Commands
| Command | Purpose | Example |
|---------|---------|---------|
| `ps aux` | All processes with details | `ps aux \| grep nginx` |
| `ps auxf` | Process tree | Shows parent-child |
| `top` | Interactive process viewer | Press `M` for memory sort |
| `kill <PID>` | SIGTERM (graceful shutdown) | `kill 1234` |
| `kill -9 <PID>` | SIGKILL (force kill) | `kill -9 1234` |
| `kill -HUP <PID>` | SIGHUP (reload config) | `kill -HUP $(cat /var/run/nginx.pid)` |
| `nice -n 19 <cmd>` | Start with low priority | `nice -n 19 backup.sh` |

## Signals Reference
| Signal | Number | Behavior |
|--------|--------|----------|
| SIGHUP | 1 | Hang up / reload config |
| SIGINT | 2 | Interrupt (Ctrl+C) |
| SIGTERM | 15 | Graceful termination (default kill) |
| SIGKILL | 9 | Force kill (cannot be caught) |
| SIGSTOP | 19 | Pause execution |
| SIGCONT | 18 | Resume after SIGSTOP |

## Zombie Handling
A zombie is a process that has completed but still has an entry in the process table because the parent hasn't read its exit status.
```bash
# Detect
ps aux | grep 'Z'
# Fix: kill the parent
kill -SIGCHLD <parent_pid>
# Or restart the parent process
```
