# Day 19 — Reference Solution

## Bash Best Practices
```bash
#!/bin/bash
set -euo pipefail    # Fail on: errors, undefined vars, pipe failures
IFS=$'\n\t'          # Safe word splitting
trap cleanup EXIT    # Always clean up
```

## Health Check Script (Simplified)
```bash
#!/bin/bash
set -euo pipefail

SERVICES=("nginx" "sshd")
WARN_ONLY=false

[[ "${1:-}" = "--warn-only" ]] && WARN_ONLY=true

for svc in "${SERVICES[@]}"; do
    if systemctl is-active --quiet "$svc" 2>/dev/null; then
        echo "✅ $svc is running"
    else
        echo "❌ $svc is NOT running"
        $WARN_ONLY || exit 1
    fi
done

# Disk check
DISK_USAGE=$(df / | awk 'NR==2 {print $5}' | sed 's/%//')
[[ $DISK_USAGE -gt 90 ]] && echo "⚠️ Disk at ${DISK_USAGE}%"
```

## Key Error Handling Patterns
| Pattern | What it does |
|---------|-------------|
| `set -e` | Exit on any error |
| `set -u` | Error on undefined variables |
| `set -o pipefail` | Fail on pipeline errors |
| `command -v <tool>` | Check if tool exists |
| `trap cleanup EXIT` | Always run cleanup |
| `|| die "message"` | Fail with custom message |
