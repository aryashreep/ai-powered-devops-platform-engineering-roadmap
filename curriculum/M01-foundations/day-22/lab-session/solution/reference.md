# Day 21 — Reference Solution

## Git Object Types
| Object | Stores | Example |
|--------|--------|---------|
| Blob | File content (binary) | `echo "hello" \| git hash-object --stdin` |
| Tree | Directory listing (blob→name→mode) | `git write-tree` |
| Commit | Snapshot + metadata | `git commit-tree <tree> -p <parent>` |
| Tag | Named reference to a commit | `git tag -a v1.0 -m "v1.0"` |

## Git Object Graph
```
commit abc123 (HEAD -> main)
├── tree def456
│   ├── blob 111aaa    README.md
│   ├── blob 222bbb    main.go
│   └── tree 333ccc    src/
│       └── blob 444ddd    utils.go
└── parent 789abc (previous commit)
```

## Branches are Pointers
```bash
# A branch is just a file containing a SHA!
cat .git/refs/heads/main
# Output: abc123def456...

# HEAD points to current branch
cat .git/HEAD
# Output: ref: refs/heads/main
```
