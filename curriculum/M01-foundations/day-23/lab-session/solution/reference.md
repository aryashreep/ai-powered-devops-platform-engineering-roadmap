# Day 22 — Reference Solution

## Merge vs Rebase Decision
```
Feature branch → main
├── Merge: Preserves history, explicit merge commits
│   "git merge feature" → creates merge commit
└── Rebase: Linear history, cleaner log
    "git rebase main && git checkout main && git merge feature"
    → fast-forward, no merge commit
```

## Conflict Resolution Steps
```bash
# 1. Identify conflicted files
git status
# 2. Open file, resolve markers
# <<<<<<< HEAD
# change from main
# =======
# change from branch
# >>>>>>> conflicting-branch
# 3. After resolving:
git add <resolved-file>
git commit -m "Resolve merge conflict in <file>"
```

## Interactive Rebase Cheatsheet
```
pick c12345 Commit message 1    → Keep as-is
reword c23456 Commit message 2 → Edit message only
squash c34567 Commit message 3 → Combine with previous
fixup c45678 Commit message 4  → Combine, discard message
drop c56789 Commit message 5   → Delete commit
```

## Common Workflow: Feature Branch
```bash
git checkout -b feature/xyz main
# ... work, commit ...
git rebase main          # Keep up-to-date
# ... more work ...
git checkout main
git merge feature/xyz    # Fast-forward after rebase
```
