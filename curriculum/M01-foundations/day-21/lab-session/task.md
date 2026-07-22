# 🧪 Lab: Python Automation Toolkit

> **Goal:** Write Python scripts for real DevOps automation tasks.

## Target Objectives

- Use requests to interact with APIs
- Parse and transform data
- Write CLI tools with argparse
- Handle errors gracefully

---

## Hands-on Challenges

### 1. API Status Checker
Write `solution/check-endpoints.py` that:
```python
#!/usr/bin/env python3
import requests
import sys

endpoints = [
    "https://httpbin.org/status/200",
    "https://httpbin.org/status/404",
    "https://httpbin.org/delay/3",
]

for url in endpoints:
    try:
        r = requests.get(url, timeout=5)
        status = "UP" if r.ok else "DOWN"
        print(f"[{status}] {url} → {r.status_code}")
    except requests.exceptions.Timeout:
        print(f"[TIMEOUT] {url}")
    except requests.exceptions.ConnectionError:
        print(f"[UNREACHABLE] {url}")
```

### 2. CLI Tool with argparse
Write `solution/server-report.py` that:
- Accepts `--host` (required), `--port` (default: 80), `--json` (flag)
- Connects to the host:port and reports if it's open
- With `--json`, outputs JSON
- Handles connection errors gracefully

### 3. Log Parser
Write `solution/parse-logs.py` that:
- Reads a log file (command-line argument)
- Counts occurrences of ERROR, WARN, INFO
- Lists the top 3 most common error messages
- Outputs a summary in the format:
```
Total lines: 1500
ERROR: 23
WARN: 45
INFO: 1432
Top errors:
  1. "timeout connecting to db" – 8 times
  2. "connection refused" – 6 times
```

### 4. System Info Script
Write `solution/sysinfo.py` that uses:
- `os` module for user/hostname
- `subprocess` for `uptime`, `df -h`
- `platform` for kernel version
- Outputs a formatted report

### 5. Virtual Environment Setup
```bash
python3 -m venv devops-env
source devops-env/bin/activate
pip install requests pyyaml
pip freeze > requirements.txt
```
Document each step and explain why venvs matter.

---

## Proof of Work

Save `solution/python-automation-report.md` documenting each script and its output.
