# Day 02 — Reference Solution

## Filesystem Navigation
```
/etc/
├── apache2/
├── mysql/
├── nginx/
├── ssh/
└── systemd/
```

## File Operations
```bash
mkdir -p /tmp/devops-foundations/{projects,logs,scripts}
touch /tmp/devops-foundations/projects/hello-world.txt
touch /tmp/devops-foundations/logs/app.log
cp /tmp/devops-foundations/projects/hello-world.txt /tmp/devops-foundations/scripts/hello.sh
rm -r /tmp/devops-foundations/logs
```

## Man Page Flags for `ls`
| Flag | Purpose |
|------|---------|
| `-a` | Show hidden files (starting with `.`) |
| `-t` | Sort by modification time (newest first) |
| `-R` | List subdirectories recursively |

## Sample Cheatsheet Entry
| Command | What it does | Example |
|---------|--------------|---------|
| `ls -la` | List all files with details | `ls -la /etc/nginx` |
| `tree -L 2` | Show tree 2 levels deep | `tree -L 2 /var/log` |
| `man ls` | Open manual for `ls` | `man cp` |
