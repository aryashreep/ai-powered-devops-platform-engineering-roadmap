# Day 20 — Reference Solution

## API Checker Script (simplified)
```python
#!/usr/bin/env python3
"""Check endpoint health with timeout handling."""
import requests
import sys

def check_endpoint(url, timeout=5):
    try:
        r = requests.get(url, timeout=timeout)
        return "UP" if r.ok else "DOWN", r.status_code
    except requests.Timeout:
        return "TIMEOUT", None
    except requests.ConnectionError:
        return "UNREACHABLE", None

if __name__ == "__main__":
    for url in sys.argv[1:]:
        status, code = check_endpoint(url)
        print(f"[{status}] {url} → {code or 'N/A'}")
```

## Python DevOps Libraries
| Library | Purpose |
|---------|---------|
| `requests` | HTTP API calls |
| `boto3` | AWS SDK |
| `paramiko` | SSH automation |
| `pyyaml` | YAML parsing |
| `jinja2` | Template rendering |
| `pytest` | Unit testing |
| `argparse` | CLI argument parsing |
| `tabulate` | Format tables in output |

## Virtual Environment Best Practices
```bash
# WHY: Isolate project dependencies, avoid version conflicts
python3 -m venv .venv
source .venv/bin/activate
# Always include
echo "source .venv/bin/activate" >> .envrc  # For direnv
pip install -r requirements.txt
```
