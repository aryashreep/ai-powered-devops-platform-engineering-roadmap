# 🧪 Lab: User & Group Management

> **Goal:** Manage Linux users and groups securely.

## Target Objectives

- Create and manage user accounts
- Create and manage groups
- Configure sudo privileges
- Understand password hashing and shadow file

---

## Hands-on Challenges

### 1. User Lifecycle
```bash
# Create a user with home directory
sudo useradd -m -s /bin/bash devops-user
# Set a password
sudo passwd devops-user
# Verify the user
id devops-user
tail -1 /etc/passwd
sudo tail -1 /etc/shadow
```

### 2. Group Management
```bash
# Create groups
sudo groupadd devops
sudo groupadd devops-admin
# Add user to groups
sudo usermod -aG devops devops-user
sudo usermod -aG devops-admin devops-user
# Verify
groups devops-user
tail -2 /etc/group
```

### 3. Sudo Configuration
```bash
# Edit sudoers safely
sudo visudo
# Add: devops-user ALL=(ALL) NOPASSWD: /usr/bin/systemctl
# Or create a file: /etc/sudoers.d/devops-user
```
Then verify: `sudo -l -U devops-user`

### 4. Service Account Pattern
```bash
# Create a service account (no login, no home)
sudo useradd -r -s /usr/sbin/nologin app-service
# Verify
grep app-service /etc/passwd
# Try to switch — what happens?
sudo -u app-service whoami
```

### 5. Audit User Access
Create `solution/user-audit.md` with:
- All human users (UID >= 1000)
- All service accounts (UID < 1000)
- Which users are in the sudo group
- Last login info (`lastlog`)

---

## Proof of Work

Save `solution/user-management-report.md` documenting every command and output.
