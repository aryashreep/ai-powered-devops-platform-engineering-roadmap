# 🧪 Lab: Git Internals Exploration

> **Goal:** Explore Git's internal object model using plumbing commands.

## Target Objectives

- Understand the .git directory structure
- Create and read raw Git objects
- Trace how trees and blobs form a commit
- Understand branches as pointers

---

## Hands-on Challenges

### 1. Explore .git
```bash
# Initialize a repo for exploration
mkdir git-internals-lab && cd git-internals-lab
git init
ls -la .git/
cat .git/HEAD
ls -la .git/objects/
cat .git/config
```

### 2. Create a Blob
```bash
echo "Hello Git Internals!" | git hash-object -w --stdin
# The SHA is returned
# Read it back
git cat-file -p <SHA>
# Check type
git cat-file -t <SHA>
```

### 3. Create a Tree
```bash
# Stage a file
echo "file1 content" > file1.txt
git add file1.txt
# What was created?
find .git/objects -type f
git cat-file -p HEAD
```

### 4. Explore Commits
```bash
# Make a commit
git commit -m "First commit"
# Examine it
git cat-file -p HEAD
# The commit contains:
# - tree SHA
# - parent SHA (none for first commit)
# - author/committer + timestamp
# - message
```

### 5. Manual Commit (Without git add/commit)
```bash
# Create blob manually
BLOB=$(echo "Direct content" | git hash-object -w --stdin)
# Create tree manually
TREE=$(printf "100644 blob $BLOB\tmanual.txt" | git mktree)
# Create commit manually
COMMIT=$(echo "Manual commit" | git commit-tree $TREE)
# Verify
git cat-file -p $COMMIT
git log --oneline $COMMIT
```

### 6. Diagram
Create `solution/git-object-diagram.md` showing:
- How a commit → tree → blobs are linked
- What `.git/HEAD`, `.git/refs/heads/main` contain
- Why Git calls itself "content-addressable storage"

---

## Proof of Work

Save `solution/git-objects-report.md` with the output from each challenge.
