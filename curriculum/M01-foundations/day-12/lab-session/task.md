# 🧪 Lab: Containers Without Docker

> **Goal:** Create isolated processes using raw Linux namespaces and cgroups.

## Target Objectives

- Create a new PID namespace
- Isolate network and mount namespaces
- Set cgroup memory limits
- Use nsenter to join a namespace
- Understand how Docker builds on these primitives

---

## Hands-on Challenges

### 1. PID Namespace Isolation
```bash
# Create a new PID namespace with unshare
sudo unshare --fork --pid --mount-proc bash
# Inside: what PIDs do you see?
ps aux
# Compare with host namespace (from another terminal):
ps aux | wc -l
# Exit namespace
exit
```

### 2. Network Namespace
```bash
# Create network namespace
sudo ip netns add devops-ns
# List all net namespaces
ip netns list
# Run a command inside it
sudo ip netns exec devops-ns ip addr
# (No network interfaces!)
# Add a veth pair and assign one end to the namespace
sudo ip link add veth0 type veth peer name veth1
sudo ip link set veth1 netns devops-ns
sudo ip netns exec devops-ns ip addr add 10.0.0.1/24 dev veth1
sudo ip netns exec devops-ns ip link set veth1 up
```

### 3. Mount Namespace
```bash
# Create a mount namespace with a different /tmp
sudo unshare --mount bash
mount --bind /some/other/dir /tmp
df -h /tmp
# Exit — namespace is gone
exit
```

### 4. cgroup Memory Limit
```bash
# On a cgroup v2 system:
cd /sys/fs/cgroup
sudo mkdir my-test-container
echo "100M" | sudo tee my-test-container/memory.max
echo "50M" | sudo tee my-test-container/memory.high
# Run a process in the group
echo $$ | sudo tee my-test-container/cgroup.procs
# Now this shell is limited to 100M memory
```

### 5. Container Analogy
Write a short explanation in `solution/container-analogy.md` answering:
- What 3 namespace types does Docker use by default?
- How does Docker use cgroups?
- Why can't a container run a different kernel than the host?

---

## Proof of Work

Save `solution/namespace-exploration.md` with:
- PID namespace: PIDs inside vs outside
- Network namespace: interfaces before and after adding veth
- cgroup memory limit configuration
- Your container analogy explanation
