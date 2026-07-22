# 🧪 Lab: Edge, Firewalls & eBPF

> **Goal:** Understand edge infrastructure and explore packet filtering.

## Target Objectives

- Explore iptables rules and packet flow
- Understand L4 vs L7 load balancing
- Trace traffic through edge components
- Understand what eBPF enables

---

## Hands-on Challenges

### 1. iptables Exploration
```bash
# List all rules (may need sudo)
sudo iptables -L -v -n
# List NAT rules
sudo iptables -t nat -L -v -n
# Check if any rules are set on your system
sudo iptables -S
```

### 2. Simple Firewall Rule
```bash
# Allow incoming SSH (port 22)
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
# Drop all other incoming (on a test interface)
# Document the rule chain flow
```

### 3. L4 vs L7 Load Balancing Diagram
Create a text diagram comparing:
- **L4 LB (AWS NLB, HAProxy TCP mode)**: Operates at transport layer, forwards TCP/UDP, preserves client IP
- **L7 LB (AWS ALB, NGINX, HAProxy HTTP mode)**: Operates at application layer, can inspect HTTP headers, path-based routing

### 4. CDN Flow
Document the flow of a request through a CDN:
```
User → Edge POP → (cache hit? serve) → Origin server
```
Include: cache-control headers, TTL, cache invalidation.

### 5. eBPF Research (Conceptual)
Research eBPF and answer:
- What does eBPF allow you to do that traditional kernel modules can't?
- Name 2 tools built on eBPF (e.g., Cilium, Falco, Pixie)
- How does eBPF improve Kubernetes networking?

---

## Proof of Work

Save `solution/edge-networking-report.md` with:
- Your iptables rules
- L4 vs L7 LB comparison table
- CDN traffic flow diagram
- eBPF research answers
