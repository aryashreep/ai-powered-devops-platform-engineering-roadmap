# Day 08 — Reference Solution

## Octal Permission Table
| Octal | Binary | rwx | Meaning |
|-------|--------|-----|---------|
| 0 | 000 | --- | No permissions |
| 1 | 001 | --x | Execute only |
| 2 | 010 | -w- | Write only |
| 3 | 011 | -wx | Write + execute |
| 4 | 100 | r-- | Read only |
| 5 | 101 | r-x | Read + execute |
| 6 | 110 | rw- | Read + write |
| 7 | 111 | rwx | All permissions |

## Common Permission Patterns
| Octal | Purpose |
|-------|---------|
| 644 | Regular files |
| 755 | Scripts/directories |
| 600 | Private keys (SSH, GPG) |
| 700 | Private directories |
| 400 | Config files with secrets |
| 444 | Read-only data |
| 777 | ⚠️ Never use (world-writable!) |

## SUID Example: /usr/bin/passwd
`passwd` needs SUID because it must write to `/etc/shadow` (owned by root) when a regular user changes their password.
```bash
ls -la /usr/bin/passwd
# -rwsr-xr-x 1 root root ... /usr/bin/passwd
# The 's' in owner execute = SUID
```
