# Day 23 — Reference Solution

## Conventional Commits Reference
```
<type>(<scope>): <description>

[optional body]

[optional footer(s)]
```
| Type | When to use |
|------|-------------|
| `feat` | A new feature |
| `fix` | A bug fix |
| `docs` | Documentation only |
| `style` | Formatting (no code change) |
| `refactor` | Code change (no feature/fix) |
| `test` | Adding/updating tests |
| `chore` | Maintenance, deps, config |
| `perf` | Performance improvement |

## .gitignore Template Essentials
```gitignore
# OS
.DS_Store
Thumbs.db

# IDE
.idea/
.vscode/
*.swp
*.swo

# Build
dist/
build/
*.pyc
__pycache__/

# Env
.env
.env.local
*.env

# Debs
node_modules/
vendor/
.cache/

# Secrets
*.key
*.pem
credentials*
```

## Pre-commit Hook Example (Simplified)
```bash
#!/bin/bash
# Prevents committing .env files
if git diff --cached --name-only | grep -q '\.env$'; then
    echo "ERROR: Cannot commit .env files!"
    exit 1
fi

# Check for large files
for file in $(git diff --cached --name-only); do
    size=$(stat -f%z "$file" 2>/dev/null)
    if [ "$size" -gt 10485760 ]; then  # 10MB
        echo "ERROR: $file exceeds 10MB!"
        exit 1
    fi
done
```
