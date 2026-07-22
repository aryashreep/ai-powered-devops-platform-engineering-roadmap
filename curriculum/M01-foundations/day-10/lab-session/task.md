# 🧪 Lab: SSH Configuration & Hardening

> **Goal:** Set up secure, passwordless SSH access and master port forwarding.

## Target Objectives

- Generate and deploy SSH keys
- Configure SSH client for efficiency
- Set up port forwarding tunnels
- Harden SSH server configuration

---

## Hands-on Challenges

### 1. Key Generation
```bash
# Generate ED25519 key (modern, secure)
ssh-keygen -t ed25519 -C "your-email@example.com"
# Alternative: RSA (compatibility)
ssh-keygen -t rsa -b 4096 -C "your-email@example.com"
# Examine the keys
cat ~/.ssh/id_ed25519.pub
ls -la ~/.ssh/
```

### 2. Key Deployment
```bash
# Copy to remote server
ssh-copy-id user@remote-server
# Or manually append to ~/.ssh/authorized_keys
cat ~/.ssh/id_ed25519.pub >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
chmod 700 ~/.ssh/
```

### 3. SSH Config Aliases
Create `~/.ssh/config` with:
```
Host web-prod
    HostName 192.168.1.100
    User deploy
    Port 2222
    IdentityFile ~/.ssh/prod-key
    LocalForward 8080 localhost:80

Host db-bastion
    HostName bastion.example.com
    User admin
    LocalForward 5432 db.internal:5432
```

### 4. Port Forwarding
```bash
# Local port forward: local:8080 → remote:80
ssh -L 8080:localhost:80 user@remote
# Test with curl
curl http://localhost:8080
```

### 5. SSH Hardening Audit
Check `/etc/ssh/sshd_config` for these settings and document the current values:
```
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
Port 22 (consider changing)
MaxAuthTries 3
AllowUsers your-user
```

---

## Proof of Work

Save `solution/ssh-setup-report.md` with:
- Your key fingerprint (`ssh-keygen -lf ~/.ssh/id_ed25519.pub`)
- Your SSH config file contents
- Hardening audit findings
- A tunnel command example you tested
