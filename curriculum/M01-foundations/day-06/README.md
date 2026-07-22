# 🗓️ Day 05 — Boot & Init

Today you'll learn what happens between pressing the power button and getting a login prompt — and how to fix it when it breaks.

## Goal

Explain the Linux boot sequence (BIOS/UEFI → GRUB2 → Kernel → initramfs → systemd) and debug boot failures.

## Key Learnings

- Boot stages: BIOS/UEFI → GRUB2 → kernel → initramfs → systemd
- systemd targets vs SysV runlevels
- `systemctl` and `journalctl` for service management
- Debugging boot failures with `journalctl -xb`

## Hands-on Lab

👉 **[Complete today's lab](./lab-session/task.md)**

## Why This Matters

A server that won't boot is a server that's down. Knowing the boot sequence is how you rescue a system after a bad kernel update, a filesystem corruption, or a misconfigured `fstab`.

## Submission

1. Complete the tasks in `lab-session/task.md`
2. Save your proof of work in `lab-session/solution/`
3. Commit with message: `Day 05 — Boot & Init`

## Learn in Public

Post your boot sequence diagram with **#LearnDevOpsIn90Days** and share one `journalctl` command you learned.
