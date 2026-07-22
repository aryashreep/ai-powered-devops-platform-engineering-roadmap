# Day 11 — Reference Solution

## Namespace Types Used by Docker
| Namespace | Isolates | Docker Use |
|-----------|----------|------------|
| pid | Process IDs | Container can't see host processes |
| net | Network stack | Each container gets its own IP/ports |
| mnt | Mount points | Container filesystem separate from host |
| uts | Hostname | Each container has its own hostname |
| ipc | IPC resources | Prevents cross-container shared memory |
| user | User IDs | Map container root to unprivileged host user |

## cgroup v2 Memory Limits
```bash
# Create cgroup and set limit
mkdir -p /sys/fs/cgroup/my-container
echo $((100 * 1024 * 1024)) > /sys/fs/cgroup/my-container/memory.max
echo $$ > /sys/fs/cgroup/my-container/cgroup.procs
# Test: this will be OOM-killed under 100M limit
stress --vm-bytes 200M --vm-keep -m 1
```

## Key Insight
```bash
# "Containers are just processes" — verify yourself:
docker run -d nginx
ps aux | grep nginx
# It's just a process on the host!
```

## Why Containers Can't Run Different Kernels
Containers share the host kernel. `unshare` creates new **instances** of kernel resources (namespaces), not new kernels. That's why Linux containers only run on Linux hosts.
