# 🧪 Lab: Branching, Merging & History

> **Goal:** Master Git branching strategies and history manipulation.

## Target Objectives

- Create, merge, and delete branches
- Understand fast-forward vs 3-way merges
- Practice rebasing
- Explore and rewrite history

---

## Hands-on Challenges

### 1. Branch Workflow
```bash
# Create a repo
mkdir branching-lab && cd branching-lab
git init
echo "Initial content" > README.md
git add . && git commit -m "Initial commit"

# Create a feature branch
git checkout -b feature-auth
echo "auth logic" > auth.go
git add . && git commit -m "Add auth module"

# Back to main, make parallel changes
git checkout main
echo "main update" > main.go
git add . && git commit -m "Update main"
```

### 2. Merge Strategies
```bash
# Merge feature into main (fast-forward if possible)
git merge feature-auth
# What happened? Check log
git log --oneline --graph --all

# Create a branch that diverges
git checkout -b conflicting-branch
echo "change from branch" > README.md
git add . && git commit -m "Branch changes README"

# Main also changes same file
git checkout main
echo "change from main" > README.md
git add . && git commit -m "Main changes README"

# This will produce a conflict!
git merge conflicting-branch
```

### 3. Conflict Resolution
```bash
# After merge conflict:
cat README.md  # Shows conflict markers
# Edit to resolve, then:
git add README.md
git commit -m "Resolve merge conflict"
```

### 4. Rebase Practice
```bash
# Create a new branch off main
git checkout -b feature-timer main
echo "timer code" > timer.py
git add . && git commit -m "Add timer feature"

# Meanwhile, someone updated main
git checkout main
echo "hotfix" > hotfix.md
git add . && git commit -m "Hotfix applied"

# Rebase your feature on latest main
git checkout feature-timer
git rebase main
# Linear history! No merge commit.
git log --oneline --graph --all
```

### 5. Interactive Rebase
```bash
# Make several commits
echo "1" > file.txt && git add . && git commit -m "WIP: first"
echo "2" >> file.txt && git add . && git commit -m "WIP: second"
echo "3" >> file.txt && git add . && git commit -m "WIP: third"

# Squash the last 3 commits into 1
git rebase -i HEAD~3
# In editor: change 'pick' to 'squash' for 2nd and 3rd commits
```

---

## Proof of Work

Save `solution/git-branching-report.md` with:
- The `git log --oneline --graph` output from your merge exercise
- How you resolved a conflict
- Before/after of the rebase
- Interactive rebase squash example
