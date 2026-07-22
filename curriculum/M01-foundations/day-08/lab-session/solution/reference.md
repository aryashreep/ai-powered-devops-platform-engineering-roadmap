# Day 07 — Reference Solution

## User Lifecycle Commands
| Action | Command |
|--------|---------|
| Create user | `sudo useradd -m -s /bin/bash <name>` |
| Set password | `sudo passwd <name>` |
| Modify user | `sudo usermod -aG <group> <user>` |
| Delete user | `sudo userdel -r <name>` |
| Create group | `sudo groupadd <name>` |
| List users | `cat /etc/passwd` |
| List groups | `cat /etc/group` |
| Check sudo | `sudo -l -U <user>` |

## /etc/shadow Fields
```
username:$6$salt$hashed:last:min:max:warn:inactive:expire
```

## Service Account Pattern
```bash
sudo useradd -r -s /usr/sbin/nologin <service-name>
```
- `-r`: create as system account (UID < 1000)
- `-s /usr/sbin/nologin`: no login shell (can't SSH)

## Sudoers Best Practice
```bash
# File: /etc/sudoers.d/deploy
deploy-user ALL=(ALL) NOPASSWD: /usr/bin/systemctl restart nginx
```
Always use `visudo` — it validates syntax before saving.
