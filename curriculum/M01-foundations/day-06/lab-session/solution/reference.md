# Day 05 — Reference Solution

## Boot Sequence Diagram
```
Power On → BIOS/UEFI → Boot Device Selection
    → GRUB2 (reads /boot/grub/grub.cfg)
    → Kernel + initramfs loaded into memory
    → Kernel decompresses, mounts initramfs (temporary root)
    → Kernel executes /sbin/init (symlinked to systemd)
    → systemd mounts real root from /etc/fstab
    → systemd executes default.target (multi-user.target or graphical.target)
    → Login prompt or GUI
```

## systemd Commands
```bash
# Service management
systemctl start|stop|restart|status|enable|disable <service>

# Journal queries
journalctl -u <service>         # Logs for a service
journalctl -xb                  # Boot log with explanations
journalctl --since "1 hour ago" # Recent logs
```

## Boot Recovery Plan
1. **At GRUB menu:** Press `e` to edit kernel command line
2. Add `systemd.unit=rescue.target` or `single` to kernel params
3. Press Ctrl+X to boot into single-user mode
4. Remount root as read-write: `mount -o remount,rw /`
5. Fix `/etc/fstab` or revert bad change
6. Reboot normally
