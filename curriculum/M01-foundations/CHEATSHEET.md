# M01 Foundations — Cheat Sheet

---

## 🐧 Linux Essentials

### Navigation
| Command | Description | Example |
|---------|-------------|---------|
| `pwd` | Print working directory | `pwd` → `/home/user` |
| `ls -la` | List all files with details | `ls -la /etc/nginx` |
| `cd /path` | Change directory | `cd /var/log` |
| `tree -L 2` | Show tree 2 levels deep | `tree -L 2 /etc` |

### File Operations
| Command | Description |
|---------|-------------|
| `mkdir -p a/b/c` | Create nested directories |
| `touch file.txt` | Create empty file |
| `cp -r src/ dest/` | Copy recursively |
| `mv old new` | Move / rename |
| `rm -rf dir/` | Remove forcefully (⚠️ dangerous) |

### File Reading & Processing
| Command | Description |
|---------|-------------|
| `cat file` | Print entire file |
| `head -n 20 file` | First 20 lines |
| `tail -f log` | Follow growing file |
| `grep -r "error" /var/log` | Search recursively |
| `grep -i "timeout" app.log` | Case-insensitive search |
| `sed 's/foo/bar/g' file` | Replace all foo with bar |
| `wc -l file` | Count lines |
| `sort \| uniq -c` | Count unique occurrences |
| `cut -d',' -f1,3 data.csv` | Extract columns |

### Pipes & Redirection
| Symbol | Purpose | Example |
|--------|---------|---------|
| `\|` | Pipe output to next command | `dmesg \| grep error` |
| `>` | Redirect stdout (overwrite) | `echo "data" > file` |
| `>>` | Redirect stdout (append) | `echo "data" >> file` |
| `2>` | Redirect stderr | `cmd 2> error.log` |
| `tee` | Split to file and stdout | `cmd \| tee output.log` |

### Permissions
| Symbol | Meaning | Octal |
|--------|---------|-------|
| `r` | Read | 4 |
| `w` | Write | 2 |
| `x` | Execute | 1 |
| `u+s` | SUID | 4xxx |
| `g+s` | SGID | 2xxx |
| `+t` | Sticky bit | 1xxx |

Common: `644` (files), `755` (dirs), `600` (keys), `700` (private)

### Processes
| Command | Description |
|---------|-------------|
| `ps aux` | All processes with details |
| `ps auxf` | Process tree |
| `top` | Interactive process viewer |
| `kill -15 PID` | Graceful stop |
| `kill -9 PID` | Force kill |
| `kill -HUP PID` | Reload config |
| `nice -n 19 cmd` | Low priority |
| `renice -n 10 -p PID` | Change priority |

### SSH
```bash
ssh-keygen -t ed25519 -C "email"  # Generate key
ssh-copy-id user@host              # Deploy key
ssh -L 8080:localhost:80 host      # Port forward
```

### System Info
| Command | Shows |
|---------|-------|
| `uname -a` | Full kernel info |
| `df -h` | Disk usage |
| `free -h` | Memory usage |
| `uptime` | Load average |
| `dmesg \| tail` | Recent kernel messages |
| `journalctl -u nginx -n 50` | Service logs |
| `lsof -i :80` | Who's on port 80 |
| `ss -tlnp` | Listening TCP sockets |
| `strace -p PID` | Trace system calls |

### Namespaces & cgroups
```bash
unshare --fork --pid --mount-proc bash   # New PID NS
ip netns add my-ns                        # New net NS
mkdir -p /sys/fs/cgroup/my-ct            # cgroup v2
echo 100M > /sys/fs/cgroup/my-ct/memory.max
```

### LVM
```bash
pvcreate /dev/sdb                         # Init PV
vgcreate my-vg /dev/sdb /dev/sdc          # Create VG
lvcreate -L 10G -n my-lv my-vg            # Create LV
lvextend -L +5G /dev/my-vg/my-lv          # Extend LV
resize2fs /dev/my-vg/my-lv                # Resize FS
```

---

## 🌐 Networking

### OSI / TCP-IP
| OSI | TCP-IP | Protocols |
|-----|--------|-----------|
| App (7) | Application | HTTP, DNS, SSH |
| Transport (4) | Transport | TCP, UDP |
| Network (3) | Internet | IP, ICMP |
| Link (2-1) | Link | Ethernet, WiFi |

### CIDR / Subnets
| CIDR | Hosts | Mask |
|------|-------|------|
| /24 | 254 | 255.255.255.0 |
| /16 | 65534 | 255.255.0.0 |
| /8 | 16M | 255.0.0.0 |

### Private IPs (RFC 1918)
- `10.0.0.0/8` — large orgs
- `172.16.0.0/12` — AWS default
- `192.168.0.0/16` — home/SOHO

### DNS
| Command | Purpose |
|---------|---------|
| `dig +short A google.com` | IPv4 lookup |
| `dig MX gmail.com` | Mail servers |
| `dig +trace google.com` | Trace resolution |
| `nslookup google.com` | Legacy lookup |

### HTTP
```bash
curl -v https://example.com          # Full request/response
curl -I https://example.com           # Headers only
curl -X POST -d '{"k":"v"}' -H "Content-Type: application/json" URL
openssl s_client -connect host:443    # TLS inspection
```

### Status Codes
| Code | Meaning |
|------|---------|
| 200 | OK |
| 201 | Created |
| 301 | Moved permanently |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not found |
| 500 | Server error |
| 502 | Bad gateway |
| 503 | Service unavailable |

---

## 💻 Languages

### Bash Best Practices
```bash
#!/bin/bash
set -euo pipefail
trap cleanup EXIT

usage() { echo "Usage: $0 [-f] [-n name]"; exit 1; }
while getopts "f:n:" opt; do ... done
```

### Python Starter
```python
#!/usr/bin/env python3
import requests, sys, argparse

parser = argparse.ArgumentParser()
parser.add_argument("--host", required=True)
args = parser.parse_args()

try:
    r = requests.get(f"https://{args.host}", timeout=5)
    print(f"Status: {r.status_code}")
except requests.ConnectionError:
    print("Unreachable")
    sys.exit(1)
```

---

## 🔧 Git

### Plumbing Commands
```bash
git hash-object -w file        # Store blob
git cat-file -p <SHA>          # Read object
git write-tree                 # Create tree
git commit-tree <tree> -p <parent>  # Create commit
```

### Branching & Merging
```bash
git checkout -b feature/main   # Create branch
git merge feature              # Merge
git rebase main                # Rebase
git rebase -i HEAD~3           # Interactive rebase
git cherry-pick <SHA>          # Pick specific commit
```

### Conventional Commits
```
feat: new feature
fix: bug fix
docs: documentation
refactor: code change (no fix/feature)
chore: maintenance, deps
test: add tests
```

### Signed Commits
```bash
git config --global user.signingkey <KEY>
git config --global commit.gpgsign true
git commit -S -m "feat: signed commit"
git log --show-signature -1
```

### Hygiene
```bash
git stash push -m "WIP: message"  # Save WIP
git stash list                     # List stashes
git stash apply stash@{0}          # Apply stash
git branch --merged \| xargs git branch -d  # Clean merged branches
```

---

## Core Principles

> **"A container is just a Linux process with namespaces + cgroups."**
> **"A branch is just a pointer to a commit."**
> **"Every production problem traces through the OSI layers."**
