# 🧪 Lab: Language Selection & Quick Starts

> **Goal:** Understand when to use Bash, Python, and Go — and write a hello-world in each.

## Target Objectives

- Write a basic script in Bash, Python, and Go
- Compare the three languages for different tasks
- Create a decision tree for language selection

---

## Hands-on Challenges

### 1. Bash Hello-World + Practical Script
```bash
#!/bin/bash
# Task: Write a script that checks if a service is running
SERVICE="nginx"
if systemctl is-active --quiet "$SERVICE"; then
    echo "✅ $SERVICE is running"
else
    echo "❌ $SERVICE is not running"
fi
```

### 2. Python Hello-World + Automation Script
```python
#!/usr/bin/env python3
# Task: Write a script that hits an API and prints the status
import requests
r = requests.get("https://httpbin.org/status/200")
print(f"Status: {r.status_code}")
```

### 3. Go Hello-World + CLI Tool
```go
package main
import (
    "fmt"
    "os/exec"
)
func main() {
    out, _ := exec.Command("uptime").Output()
    fmt.Printf("Server uptime: %s", out)
}
```

### 4. Decision Tree
Create `solution/language-decision-tree.md` with:
- A decision tree for when to use Bash vs Python vs Go
- 3 DevOps tasks that best fit each language
- One real-world tool written in each language

### 5. Ecosystem Survey
Research and document:
- Package managers: apt (system), pip (Python), go mod (Go)
- Each language's testing tools
- Each language's CI/CD integration patterns

---

## Proof of Work

Save `solution/language-strategy.md` with:
- Your Bash, Python, and Go scripts
- Decision tree
- Ecosystem comparison table
