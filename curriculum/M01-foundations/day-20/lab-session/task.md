# 🧪 Lab: Production Bash Scripting

> **Goal:** Write robust, production-quality Bash scripts.

## Target Objectives

- Write scripts with proper error handling
- Parse command-line arguments
- Use functions for modularity
- Handle signals and cleanup

---

## Hands-on Challenges

### 1. Safe Shell Template
Create a script template `solution/safe-script.sh`:
```bash
#!/bin/bash
set -euo pipefail
IFS=$'\n\t'

# Config
readonly SCRIPT_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"

# Functions
log() {
    echo "[$(date '+%Y-%m-%d %H:%M:%S')] $*"
}

error() {
    echo "[ERROR] $*" >&2
    exit 1
}

# Cleanup
cleanup() {
    log "Cleaning up..."
}
trap cleanup EXIT
```

### 2. System Health Check Script
Write `solution/health-check.sh` that:
- Checks if nginx/sshd are running
- Checks disk usage (>90% = warning)
- Checks memory usage (>80% = warning)
- Accepts `--warn-only` flag to not exit on warnings
- Returns appropriate exit codes

### 3. Log Archiver Script
Write `solution/log-archiver.sh` that:
- Takes a directory path as argument
- Finds files older than N days (configurable, default: 7)
- Compresses them with gzip
- Moves to an archive directory
- Logs every action

### 4. Command-Line Argument Parser
```bash
#!/bin/bash
usage() {
    echo "Usage: $0 -s <source> -d <dest> [-v]"
    exit 1
}

while getopts "s:d:v" opt; do
    case $opt in
        s) SOURCE="$OPTARG" ;;
        d) DEST="$OPTARG" ;;
        v) VERBOSE=true ;;
        *) usage ;;
    esac
done
```

### 5. Error Handling Showcase
Create `solution/error-handling-demo.sh` demonstrating:
- What happens without `set -e` (catches errors)
- Using `trap` for cleanup
- Checking command existence before running
- Proper exit codes for different failure modes

---

## Proof of Work

Save `solution/bash-scripts-report.md` with all your scripts and a brief explanation of each.
