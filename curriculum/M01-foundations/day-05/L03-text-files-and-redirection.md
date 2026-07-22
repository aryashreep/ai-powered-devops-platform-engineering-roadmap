---
module: "M01"
unit: "Linux internals"
lesson: "Text files & I/O redirection"
day: 3
status: draft            # draft | reviewed | recorded | published
est_minutes: 45          # Story(2) + Problem(3) + Visualization(2) + Theory(5) + Demo(13) + Mistakes(5) + Interview(5) + Assignment(10)
---

# Text file operations & I/O

> **Learning goal:** Read large files efficiently, redirect and pipe output, search text with grep, and edit files in-place with sed.

---

## Story (2 min)

### The 2GB Log Incident

**Characters:** Golu (junior) and Jagu (senior)

11:47 PM. Payment service response time hit 8 seconds. SLO breach. Alert fired.

The log file was `app.log` — size **2.1 GB**.

Golu ran `cat app.log` — terminal froze. Jagu quietly typed:

```bash
tail -f app.log | grep -i "timeout" | tee live_errors.txt
```

15 seconds later: root cause found. Downstream DB connection pool exhausted. Ticket closed. Sleep.

> After today, you'll be that senior engineer.

---

## Real-world Problem (3 min)

### What Breaks Without Text Tools?

- **cat on 2GB file:** Terminal freezes — you don't know `tail` or `less`
- **No grep:** Manually scrolling millions of lines looking for "ERROR"
- **> vs >> confusion:** Overwrite a critical config file instead of appending
- **No sed:** Need to open an editor for mass config changes across 50 servers
- **No pipes:** Create 5 temporary files for what should be a one-liner

### Topic Flow

```
Problem → Story → Visualization → Theory → Hands-on → Best Practice → Interview → Assignment
  │        │          │             │         │            │             │           │
  v        v          v             v         v            v             v           v
2GB log  tail+      I/O stream   4 pillars  4 demo       sed -i.bak    pipe vs   syslog
file     grep+tee   diagram      + tables  steps        always       redirect   analysis
```

---

## Visualization (2 min)

### I/O Stream Flow

Draw on whiteboard:

```
stdin (0) ──→ [Command] ──→ stdout (1) ──→ file / pipe
                               stderr (2) ──→ 2>&1 → stdout
                                               or → /dev/null (discard)
```

**Mental model:** Every Linux process has 3 standard streams. Think of them as pipes:
- **stdin (0):** Water input to your house
- **stdout (1):** Clean water drain (normal output)
- **stderr (2):** Sewage drain (error messages)

You can redirect where each stream flows using `>`, `>>`, `2>&1`, and connect streams between commands with `|`.

---

## Theory (5 min)

### Four Pillars: Read, Redirect, Search, Edit

#### 1. Reading Files

| Command | Best for | Avoid |
|---------|----------|-------|
| `cat file` | Small files (configs, scripts) — prints entire file | **NEVER** on files > 50MB |
| `head -20` | First N lines — check headers, verify format | — |
| `tail -f` | **Live follow** — incident response #1 tool | — |
| `less file` | Large files — paginated, memory-safe, `/pattern` search | — |

```bash
# Small files
cat /etc/hostname
cat /etc/hosts

# First lines
head -20 /var/log/syslog

# Last lines + live follow
tail -50 /var/log/syslog
tail -f /var/log/syslog          # Live stream! Ctrl+C to exit

# Any size — memory safe
less /var/log/syslog             # /ERROR to search, q to quit
```

#### 2. Redirection

| Operator | Effect |
|----------|--------|
| `>` | Overwrite — **DANGEROUS**, wipes file |
| `>>` | Append — **SAFE**, adds to end of file |
| `\|` | Pipe — connect commands (Unix's greatest invention) |
| `tee` | Write to file AND display on screen |
| `2>&1` | Merge stderr into stdout |
| `/dev/null` | Discard all output (black hole) |

```bash
# > overwrite vs >> append
echo "hello" > output.txt      # Creates new file
echo "world" >> output.txt     # Appends to end

# Pipe — chain commands
cat /etc/passwd | grep "root"

# tee — see output + save it simultaneously
tail -f app.log | tee live_capture.txt

# 2>&1 — merge errors into output stream
ls /nonexistent 2>&1 | tee errors.log

# /dev/null — discard output
cron_script.sh > /dev/null 2>&1
```

#### 3. Searching & Text Processing

| Command | What it does | Key usage |
|---------|-------------|-----------|
| `grep` | Pattern search | `-i` (ignore case), `-r` (recursive), `-v` (invert), `-c` (count) |
| `wc` | Count lines/words/bytes | `-l` (lines only) |
| `sort` | Sort lines | `-rn` (reverse numeric), `-k2` (by field 2) |
| `uniq` | Remove/count duplicates | `-c` (count occurrences) — requires sorted input |
| `cut` | Extract fields | `-d:` (delimiter), `-f1` (field 1) |

```bash
# grep
grep -i "error" app.log           # Case insensitive
grep -r "DB_HOST" /etc/           # Recursive directory search
grep -v "DEBUG" app.log           # Invert — lines WITHOUT pattern
grep -c "ERROR" app.log           # Count matching lines

# wc
wc -l app.log                     # Count total lines

# Full pipeline: top error messages
cat app.log | grep "ERROR" | sort | uniq -c | sort -rn | head -10

# Extract usernames from /etc/passwd
cut -d: -f1 /etc/passwd | sort
```

#### 4. sed — Stream Editor

sed treats text files as a stream — read, transform, output. Perfect for mass config changes.

```bash
# s/old/new/g — global substitution
sed 's/localhost/db.prod.internal/g' config.env

# -i — in-place edit (modifies file directly)
sed -i 's/DEBUG/INFO/g' app.conf

# -i.bak — in-place WITH backup (BEST PRACTICE)
sed -i.bak 's/old_url/new_url/g' nginx.conf

# /pattern/d — delete matching lines
sed '/^#/d' config.conf          # Delete comment lines
sed '/^$/d' config.conf          # Delete blank lines

# Multiple operations
sed -e 's/foo/bar/g' -e '/^#/d' input.txt
```

---

## Live Demo (13 min)

### Step 1 — Generate Fake Log + Live Tail

```bash
# Generate test log
for i in $(seq 1 100); do
  echo "$(date) [INFO] Request $i processed" >> /tmp/app.log
  echo "$(date) [ERROR] DB timeout on request $i" >> /tmp/app.log
done

# Live follow
tail -f /tmp/app.log              # Ctrl+C to exit

# Specific lines
tail -20 /tmp/app.log
head -5 /tmp/app.log
```

### Step 2 — grep Patterns

```bash
# Basic search
grep "root" /etc/passwd

# Case insensitive
grep -i "BASH" /etc/passwd

# Invert match
grep -v "nologin" /etc/passwd

# Count bash users
grep -c "/bin/bash" /etc/passwd

# Recursive config search
grep -r "PermitRootLogin" /etc/ssh/
```

### Step 3 — Pipeline: wc + sort + uniq

```bash
# Total lines
wc -l /tmp/app.log

# Count ERROR lines only
grep "ERROR" /tmp/app.log | wc -l

# Full pipeline: ranked error messages
cat /tmp/app.log | grep "ERROR" | sort | uniq -c | sort -rn

# Extract usernames
cut -d: -f1 /etc/passwd | sort
```

### Step 4 — sed Substitution Live

```bash
# Create test config
cat > /tmp/test.conf <<'EOF'
DB_HOST=localhost
DB_PORT=5432
APP_ENV=development
# This is a comment
DEBUG=true
EOF

# Preview (no file change)
sed 's/localhost/db.prod.internal/g' /tmp/test.conf

# In-place with backup
sed -i.bak 's/development/production/g' /tmp/test.conf

# Delete comment lines
sed -i '/^#/d' /tmp/test.conf

# Verify
cat /tmp/test.conf
diff /tmp/test.conf /tmp/test.conf.bak
```

---

## Common Mistakes (5 min)

### Mistake 1: `>` vs `>>` Confusion

**Wrong:**
```bash
echo "new entry" > important.log   # WIPES the entire file!
```

**Right:**
```bash
echo "new entry" >> important.log  # Appends safely
```

### Mistake 2: `cat` on Huge Files

**Wrong:**
```bash
cat /var/log/syslog               # 500MB file — TERMINAL FREEZES!
```

**Right:**
```bash
less /var/log/syslog              # Paginated, memory-safe
tail -100 /var/log/syslog         # Last 100 lines only
```

### Mistake 3: `sed -i` Without Backup

**Wrong:**
```bash
sed -i 's/old/new/g' nginx.conf    # No backup. How to undo?
```

**Right:**
```bash
sed -i.bak 's/old/new/g' nginx.conf  # .bak auto-created
mv nginx.conf.bak nginx.conf          # Revert in one command
```

---

## Interview Questions (5 min)

1. **Q: What does the Linux pipe (`|`) do?**
   **A:** The pipe connects stdout of one command to stdin of another — without temporary files. Example: `cat file | grep "ERROR" | sort | uniq -c` chains 4 commands, data flows in memory. Unix philosophy: small tools, compose with pipes.

2. **Q: `grep -v` vs plain `grep` — what's the difference?**
   **A:** `grep pattern` prints lines that MATCH the pattern. `grep -v pattern` prints lines that do NOT match (invert match). Use case: `grep -v "DEBUG" app.log` — remove noise, see real errors.

3. **Q: Difference between `>` and `>>`?**
   **A:** `>` — overwrite. If file exists, it's wiped. `>>` — append. Adds to end of file. For logs and important data, ALWAYS use `>>`.

4. **Q: What does `tee` do?**
   **A:** `tee` reads from stdin and writes to BOTH stdout AND a file simultaneously. Like a T-shaped pipe — data goes two directions. Use case: `tail -f app.log | tee capture.txt` — see live output AND save it.

---

## Assignment (10 min)

**Task: /var/log/syslog Full Pipeline Analysis**

1. `wc -l /var/log/syslog` — count total lines
2. `grep -i "error" /var/log/syslog | wc -l` — count error lines
3. `grep -i "error" /var/log/syslog | sort | uniq -c | sort -rn | head -10` — top 10 errors
4. `grep -i "error" /var/log/syslog | tee ~/syslog_analysis.txt` — save results with tee
5. `sed -i.bak 's/error/ERROR/g' ~/syslog_analysis.txt` — practice sed with backup

**You know you're done when:** `~/syslog_analysis.txt` exists with a ranked list of error messages and their frequencies.

---
---

## Today's Takeaway
> Piping commands together lets you process millions of log lines without writing a single line of code.

## Today's Interview Question
> "How would you find the top 10 most frequent errors in a log file?" — `grep ERROR | sort | uniq -c | sort -rn | head -10`

## Today's Assignment
> Run a full pipeline analysis on `/var/log/syslog` — count, search, rank, and save errors.

## Today's Challenge
> Write a one-liner to find the top 5 most frequent IPs from `/var/log/nginx/access.log` using cut, sort, uniq, sort.

## Today's Mistake
> Never `cat` large files. Use `less` for reading and `tail -f` for live monitoring.

## Today's Pro Tip
> Add `grep --color=auto` to your `~/.bashrc` to always get highlighted matches. Use `-n` to show line numbers — crucial when reporting errors.
