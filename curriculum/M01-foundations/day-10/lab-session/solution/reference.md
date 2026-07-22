# Day 09 — Reference Solution

## SSH Key Types
| Type | Security | Performance | Best For |
|------|----------|------------|----------|
| ED25519 | Excellent | Fast | Modern systems (default) |
| RSA 4096 | Good | Slower | Legacy systems / PKI |
| ECDSA | Good | Fast | Compliance requirements |
| DSA | ❌ Weak | — | Never use |

## ~/.ssh/config Example
```ssh-config
Host *
    ServerAliveInterval 60
    ServerAliveCountMax 3
    StrictHostKeyChecking ask

Host jump-box
    HostName bastion.example.com
    User admin
    ForwardAgent yes

Host internal-*
    HostName %h.internal.example.com
    User devops
    IdentityFile ~/.ssh/internal-key
    ProxyJump jump-box
```

## Hardening Checklist
- [ ] `PermitRootLogin no` — no direct root SSH
- [ ] `PasswordAuthentication no` — keys only
- [ ] `MaxAuthTries 3` — rate-limit attempts
- [ ] `PubkeyAuthentication yes`
- [ ] `UseDns no` — speed up connections
- [ ] `ClientAliveInterval 300` — idle timeout
- [ ] Restart after changes: `sudo systemctl restart sshd`
