# 🧪 Lab: Boot & systemd Deep Dive

> **Goal:** Trace the boot sequence and master systemd service management.

## Target Objectives

- Identify boot loader configuration
- Examine kernel boot messages
- Manage systemd services
- Explore journald logs

---

## Hands-on Challenges

### 1. Boot Loader Inspection
```bash
ls /boot/
cat /boot/grub/grub.cfg | head -50
# Identify default kernel entry and kernel parameters
```

### 2. Boot Messages
```bash
# View kernel ring buffer
dmesg | head -30
dmesg | grep -i "error" | tail -10
# Time since boot
cat /proc/uptime
```

### 3. systemd Service Management
```bash
# List all services
systemctl list-units --type=service --all
# Check a service status
systemctl status sshd
# View journal for a service
journalctl -u sshd --no-pager | tail -20
```

### 4. Target vs Runlevel
```bash
# Current default target
systemctl get-default
# List all targets
systemctl list-units --type=target --all
# Compare with old runlevels
ls -la /lib/systemd/system/runlevel*.target
```

### 5. Boot Failure Simulation (Dry Run)
Document in your solution: If a system won't boot after a bad `/etc/fstab` entry, what steps would you take to recover? Include the single-user mode or recovery kernel approach.

---

## Proof of Work

Save `solution/boot-sequence-report.md` with:
- Your GRUB default kernel line
- 3 kernel errors from `dmesg`
- Status of `sshd`, `cron`, and `nginx` (if installed)
- Default systemd target
- Your boot failure recovery plan
