# 🗓️ Day 09 — SSH & Remote Access

Today you'll learn SSH — the remote access protocol that connects you to every server you'll ever manage.

## Goal

Configure passwordless SSH access, understand key types, set up port forwarding, and harden SSH configuration.

## Key Learnings

- Password vs key-based authentication
- `ssh-keygen` — RSA vs ED25519
- `authorized_keys` and `~/.ssh/config`
- SSH tunneling (local and remote port forwarding)
- SSH hardening best practices

## Hands-on Lab

👉 **[Complete today's lab](./lab-session/task.md)**

## Why This Matters

SSH is the universal remote access protocol. Passwordless auth is the baseline. Tunnels let you reach databases through bastions. Harden it wrong, and you're compromised.

## Submission

1. Complete the tasks in `lab-session/task.md`
2. Save your proof of work in `lab-session/solution/`
3. Commit with message: `Day 09 — SSH & Remote Access`

## Learn in Public

Share one SSH config trick that saves you time daily with **#LearnDevOpsIn90Days** (e.g., config file Host aliases).
