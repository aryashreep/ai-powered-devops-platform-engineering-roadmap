# 🧪 Lab: Git Hygiene & Best Practices

> **Goal:** Develop disciplined Git habits for professional collaboration.

## Target Objectives

- Write meaningful commit messages (Conventional Commits)
- Use .gitignore effectively
- Handle large files with Git LFS
- Sign commits with GPG verification
- Clean up stray branches

---

## Hands-on Challenges

### 1. Conventional Commits
Create a commit history demonstrating:
```bash
feat: add user authentication module
fix: resolve null pointer in login handler
docs: update API documentation
refactor: extract database helper functions
test: add unit tests for auth module
chore: update dependencies
```

### 2. .gitignore Mastery
Create `solution/.gitignore` covering:
- OS files (.DS_Store, Thumbs.db)
- IDE files (.idea/, .vscode/)
- Build artifacts (dist/, build/, *.pyc)
- Environment files (.env, *.local)
- Dependency directories (node_modules/, vendor/)
- Secrets (*.key, *.pem, credentials*)

### 3. Commit Hooks
Create a pre-commit hook (`solution/pre-commit-hook.sh`) that:
- Checks for large files (> 10MB)
- Prevents committing .env files
- Validates no TODO/FIXME in new code
- Runs basic syntax check on shell scripts

### 4. Clean Up Branches
```bash
# List local branches
git branch
# Delete merged local branches
git branch --merged | grep -v "\*\|main" | xargs -n 1 git branch -d
# Delete remote tracking branches
git remote prune origin
```

### 5. Stash & Cherry-Pick
```bash
# Save work-in-progress
git stash push -m "WIP: auth refactor"
# List stashes
git stash list
# Apply a specific stash
git stash apply stash@{0}
# Cherry-pick a specific commit to current branch
git cherry-pick <commit-hash>
```

---

## Proof of Work

Save `solution/git-hygiene-report.md` with:
- Your commit history using conventional commits
- The .gitignore template
- Your pre-commit hook script
- Stash/cherry-pick demonstration
