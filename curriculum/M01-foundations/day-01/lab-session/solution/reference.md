# Day 01 — Reference Solution

## Challenge 1: Kernel Identity

`uname -a` output (yours will differ — kernel version and hostname are unique to your system):
```
Linux myhostname 6.2.0-26-generic #26~22.04.1-Ubuntu SMP PREEMPT_DYNAMIC x86_64 GNU/Linux
```

`cat /proc/version` output:
```
Linux version 6.2.0-26-generic (buildd@...) (gcc version 11.4.0) #26~22.04.1-Ubuntu
```

Key files found in `/boot/`:
- `vmlinuz-*` — the compressed kernel image (the actual Linux kernel binary)
- `initrd.img-*` — initial RAM disk (temporary filesystem used during boot)
- `config-*` — kernel build configuration
- `System.map-*` — kernel symbol table (used for debugging)

## Challenge 2: Systemd

`ps -p 1 -o pid,ppid,cmd` output:
```
  PID  PPID CMD
    1     0 /lib/systemd/systemd --system --deserialize 31
```
- PID 1 = systemd (the first process the kernel starts)
- PPID 0 = no parent (the kernel itself started it)

Systemd version: `249+` (Ubuntu 22.04) or `252+` (Ubuntu 23.04+)

`systemctl list-units --type=service --state=running` output format (abbreviated):
```
UNIT                          LOAD   ACTIVE SUB     DESCRIPTION
systemd-journald.service      loaded active running Journal Service
systemd-resolved.service      loaded active running Network Name Resolution
systemd-udevd.service         loaded active running Rule-based Manager for Device Events
sshd.service                  loaded active running OpenSSH Daemon
cron.service                  loaded active running Regular background program processing
```
Look at the **UNIT** column — those are the service names to write down. The `.service` suffix is part of the name.

## Challenge 3: Onion Model Walk

Expected output pattern (values will differ on your system):

```bash
$ ps aux | head -10
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  0.0  0.1 167788 11832 ?        Ss   10:00   0:01 /lib/systemd/systemd
root         2  0.0  0.0      0     0 ?        S    10:00   0:00 [kthreadd]
root         3  0.0  0.0      0     0 ?        I<   10:00   0:00 [rcu_gp]
root         4  0.0  0.0      0     0 ?        I<   10:00   0:00 [rcu_par_gp]
root         9  0.0  0.0      0     0 ?        I<   10:00   0:00 [mm_percpu_wq]
root        10  0.0  0.0      0     0 ?        S    10:00   0:00 [ksoftirqd/0]
root        11  0.0  0.0      0     0 ?        I    10:00   0:00 [rcu_sched]
...
# (Your output will show different PIDs and process names — this is a representative sample)

$ echo $SHELL
/bin/bash

$ uname -r
6.2.0-26-generic

$ cat /proc/uptime
3600.42 14200.11
# First number = seconds since boot. 3600 seconds = 1 hour uptime.

$ cat /proc/meminfo | head -5
MemTotal:        8053556 kB     ← 8053556 / 1024 / 1024 ≈ 7.7 GB RAM
MemFree:         4201234 kB
MemAvailable:    5832910 kB
Buffers:          123456 kB
Cached:           987654 kB
```

**Which layer does each command belong to?**

| Command | Onion Layer |
|---|---|
| `ps aux` | Applications (outermost) |
| `echo $SHELL` | Shell |
| `uname -r`, `/proc/uptime` | Kernel |
| `cat /proc/meminfo` | Kernel (via /proc) |
| `dmesg \| tail -20` | Kernel hardware log |

## Challenge 4: User Space vs Kernel Space

`cat /proc/cpuinfo | head -20` — look for these two fields:
```
model name   : Intel(R) Core(TM) i5-1135G7 @ 2.40GHz
cpu cores    : 4
```

`cat /proc/meminfo | head -5` — look for `MemTotal`:
```
MemTotal:        8053556 kB
```
To convert: 8053556 ÷ 1024 = 7865 MB ÷ 1024 ≈ **7.7 GB**

`dmesg | tail -20` — normal output looks like:
```
[    0.000000] Linux version 6.2.0-26-generic
[    0.512345] ACPI: Core revision 20220331
[    1.234567] NET: Registered PF_INET6 protocol family
```
If you see `error`, `failed`, `oom`, or `killed` — that is a kernel warning worth noting.

## Challenge 5: Architecture Diagram
```
┌──────────────────────────────────────────┐
│  Applications  (nginx, sshd, your code)  │  ← User Space
├──────────────────────────────────────────┤
│  Shell  (bash, zsh)                      │  ← User Space
├──────────────────────────────────────────┤
│  System Libraries  (glibc)               │  ← User Space
│                          ↕ system calls  │    (boundary — not a layer)
├──────────────────────────────────────────┤
│  Kernel  (scheduler, memory mgmt, FS)    │  ← Kernel Space
├──────────────────────────────────────────┤
│  Hardware  (CPU, RAM, disk, NIC)         │  ← Physical Layer
└──────────────────────────────────────────┘
```

> **Why "System Libraries", not "System Calls"?**
> System calls (`open`, `read`, `write`, `fork`) are the *mechanism* that libraries use to cross from user space into kernel space. They are the border crossing — not a floor of the building. The layer is **System Libraries (glibc)**; the system calls are how it knocks on the kernel's door.
