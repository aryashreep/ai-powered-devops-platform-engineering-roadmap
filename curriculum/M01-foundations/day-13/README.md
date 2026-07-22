# 🗓️ Day 12 — Filesystems & LVM

Today you'll learn how Linux manages persistent storage — filesystem types, mount points, and logical volume management.

## Goal

Create, mount, and manage filesystems; use LVM to flexibly manage disk space across the fleet.

## Key Learnings

- Filesystem types: ext4, xfs, btrfs
- `mount`/`umount` and `/etc/fstab`
- LVM: physical volumes → volume groups → logical volumes
- `pvcreate`, `vgcreate`, `lvcreate`
- Resizing filesystems and LVM volumes

## Hands-on Lab

👉 **[Complete today's lab](./lab-session/task.md)**

## Why This Matters

Disk full = service down. LVM lets you add space without downtime. Knowing how to mount, extend, and troubleshoot filesystems is a core production skill.

## Submission

1. Complete the tasks in `lab-session/task.md`
2. Save your proof of work in `lab-session/solution/`
3. Commit with message: `Day 12 — Filesystems & LVM`

## Learn in Public

Share your LVM setup walkthrough with **#LearnDevOpsIn90Days** — especially how it saved you from a disk-full outage.
