# 🧪 Lab: Permissions & Ownership Mastery

> **Goal:** Set, interpret, and troubleshoot Linux permissions.

## Target Objectives

- Read and change permission bits
- Use octal and symbolic notation interchangeably
- Apply special permission bits (SUID, SGID, sticky)
- Use ACLs for group collaboration

---

## Hands-on Challenges

### 1. Permission Basics
```bash
# Create a test file
touch /tmp/permissions-test.txt
ls -la /tmp/permissions-test.txt
# Change permissions
chmod 644 /tmp/permissions-test.txt
chmod u+x /tmp/permissions-test.txt
chmod go-rwx /tmp/permissions-test.txt
# Observe changes with ls -la after each
```

### 2. Octal Reference Table
Create `solution/permissions-table.md` with a complete 0-777 octal permission table.

### 3. Ownership
```bash
sudo chown nobody:nogroup /tmp/permissions-test.txt
sudo chmod 600 /tmp/permissions-test.txt
# Can a different user read it? Test with sudo -u
sudo -u nobody cat /tmp/permissions-test.txt
```

### 4. Special Bits
```bash
# SUID — file runs as owner
chmod u+s /usr/bin/myapp
ls -la /usr/bin/passwd  # Note the 's' in owner execute

# SGID — new files inherit group
chmod g+s /shared/directory

# Sticky bit — only owner can delete files
chmod +t /tmp
ls -la / | grep tmp  # Note the 't'
```

### 5. ACL Practice
```bash
# Enable ACL on a test directory
setfacl -m u:devops-user:rwx /tmp/shared-dir
getfacl /tmp/shared-dir
```

---

## Proof of Work

Save `solution/permissions-report.md` with:
- Your octal permission table (0-777)
- Demonstration of each special bit
- ACL configuration example
- Why `passwd` has SUID set (`ls -la /usr/bin/passwd`)
