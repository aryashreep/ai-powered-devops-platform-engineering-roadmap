# Day 10 — Reference Solution

## Diagnostic Toolkit Quick Reference
| Tool | What it shows | Common Usage |
|------|--------------|--------------|
| `strace` | System calls | `strace -p <PID}` — debug hanging process |
| `lsof` | Open files | `lsof -i :443` — who's on HTTPS |
| `ss` | Socket stats | `ss -tlnp` — listening TCP sockets |
| `vmstat` | System health | `vmstat 1` — real-time CPU/memory |
| `iostat` | Disk I/O | `iostat -x 1` — per-disk metrics |
| `dmesg` | Kernel logs | `dmesg \| tail` — recent kernel msgs |

## Troubleshooting Flow: "Service Won't Start"
```
1. systemctl status <service>      → check systemd
2. journalctl -u <service> -n 50   → check logs
3. ss -tlnp | grep <port>          → port conflict?
4. lsof /var/run/<service>.pid     → stale PID file?
5. strace -f <service>             → what syscall fails?
6. dmesg | tail                    → OOM killer? segfault?
```

## Quick Health Check Script
```bash
#!/bin/bash
echo "=== CPU ===" && uptime
echo "=== Memory ===" && free -h
echo "=== Disk ===" && df -h /
echo "=== Listeners ===" && ss -tlnp
echo "=== Top Processes ===" && ps aux --sort=-%cpu | head -5
```
