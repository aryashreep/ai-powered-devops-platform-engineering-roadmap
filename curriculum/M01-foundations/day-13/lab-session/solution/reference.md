# Day 12 — Reference Solution

## LVM Architecture
```
┌─────────────────────────────────────────────┐
│  /dev/devops-vg/devops-lv  (Logical Volume) │
├─────────────────────────────────────────────┤
│         devops-vg  (Volume Group)            │
├────────────────────┬────────────────────────┤
│ /dev/loop1 (PV)    │  /dev/loop2 (PV)       │
│ 500M               │  500M                  │
└────────────────────┴────────────────────────┘
```

## Key LVM Commands
| Command | Purpose |
|---------|---------|
| `pvcreate /dev/sdb` | Initialize a disk as PV |
| `vgcreate vg-name /dev/sdb /dev/sdc` | Create VG from PVs |
| `lvcreate -L 10G -n lv-name vg-name` | Create LV |
| `vgs` / `pvs` / `lvs` | Display summaries |
| `lvextend -L +5G /dev/vg/lv` | Extend LV by 5GB |
| `resize2fs /dev/vg/lv` | Resize ext4 filesystem |
| `xfs_growfs /mount/point` | Resize xfs filesystem |

## fstab Format Reference
```
<device>       <mount-point>  <fstype>  <options>  <dump>  <pass>
/dev/sda1      /              ext4      defaults   0       1
UUID=abc-123   /boot          ext4      defaults   0       2
/dev/vg/lv     /data          xfs       defaults   0       2
```

## Filesystem Comparison
| FS | Max Size | Best For |
|----|----------|----------|
| ext4 | 1EB, 16TB file | General purpose |
| xfs | 8EB | Large files, parallel I/O |
| btrfs | 16EB | Snapshots, compression |
