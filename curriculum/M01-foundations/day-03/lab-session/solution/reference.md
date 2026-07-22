# Day 04 — Reference Solution

## FHS Reference Table

| Directory | Purpose | Notable contents |
|-----------|---------|------------------|
| `/` | Root — filesystem base | Everything starts here |
| `/etc` | System configuration | `sshd_config`, `hosts`, `fstab`, `nginx/` |
| `/var` | Variable data | `log/`, `lib/mysql`, `spool/cron` |
| `/var/log` | Log files | `syslog`, `auth.log`, `kern.log` |
| `/tmp` | Temporary files | Cleared on reboot |
| `/proc` | Process & kernel info | CPU, memory, running PIDs |
| `/sys` | Kernel & device info | Block devices, power management |
| `/usr` | User programs | `bin/`, `lib/`, `share/` |
| `/home` | User home directories | `~` for each user |
| `/boot` | Boot loader files | `vmlinuz`, `initrd.img`, `grub/` |
| `/opt` | Optional/third-party | vendor applications |

## Largest Log File
```bash
du -sh /var/log/* | sort -rh | head -3
# Typical: /var/log/syslog, /var/log/kern.log
```

## /proc/cpuinfo Sample
```
processor       : 0
vendor_id       : GenuineIntel
model name      : Intel(R) Core(TM) i7-8700K CPU @ 3.70GHz
cores           : 6
```
