# 🧪 Lab: Storage Management with LVM

> **Goal:** Build a flexible storage system using LVM and manage filesystems.

## Target Objectives

- Create and format filesystems
- Mount filesystems persistently with fstab
- Build LVM: PV → VG → LV
- Extend a logical volume online
- Understand filesystem checking (fsck)

---

## Hands-on Challenges

### 1. Filesystem Creation (using loopback devices)
```bash
# Create a 1GB test file
dd if=/dev/zero of=/tmp/test-disk.img bs=1M count=1000
# Set up loopback device
sudo losetup -fP /tmp/test-disk.img
sudo losetup -a
# Format with ext4
sudo mkfs.ext4 /dev/loop0
# Mount it
sudo mkdir /mnt/test-fs
sudo mount /dev/loop0 /mnt/test-fs
df -h /mnt/test-fs
```

### 2. Persistent Mount (fstab) — Simulated
Add an entry to `/etc/fstab` (just document it, don't modify on production):
```
/dev/loop0  /mnt/test-fs  ext4  defaults  0  2
```
Then test: `sudo mount -a`

### 3. LVM Setup
```bash
# Create two more loop devices
dd if=/dev/zero of=/tmp/disk2.img bs=1M count=500
dd if=/dev/zero of=/tmp/disk3.img bs=1M count=500
sudo losetup -fP /tmp/disk2.img
sudo losetup -fP /tmp/disk3.img

# Create physical volumes
sudo pvcreate /dev/loop1 /dev/loop2
# Create volume group
sudo vgcreate devops-vg /dev/loop1 /dev/loop2
# Create logical volume
sudo lvcreate -L 800M -n devops-lv devops-vg

# Format and mount
sudo mkfs.ext4 /dev/devops-vg/devops-lv
sudo mkdir /mnt/lvm-test
sudo mount /dev/devops-vg/devops-lv /mnt/lvm-test
df -h /mnt/lvm-test
```

### 4. Extend a Logical Volume
```bash
# Volume is 800M, but we need 1G
# Check current size
lvdisplay /dev/devops-vg/devops-lv
# Extend the LV
sudo lvextend -L +200M /dev/devops-vg/devops-lv
# Resize the filesystem
sudo resize2fs /dev/devops-vg/devops-lv
# Verify
df -h /mnt/lvm-test
```

### 5. Filesystem Check
```bash
# Unmount first
sudo umount /mnt/lvm-test
# Check filesystem
sudo fsck -f /dev/devops-vg/devops-lv
# Remount
sudo mount /dev/devops-vg/devops-lv /mnt/lvm-test
```

---

## Proof of Work

Save `solution/storage-report.md` with:
- LVM layout diagram (PV → VG → LV)
- Commands used for each step
- Before/after sizes for the LV extension
- fstab entry format reference
