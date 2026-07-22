# 🧪 Lab: TCP & UDP Analysis

> **Goal:** Analyze TCP connections and understand transport protocols.

## Target Objectives

- Capture and analyze TCP three-way handshake
- Identify TCP connection states
- Compare TCP vs UDP behavior
- Analyze ports and sockets

---

## Hands-on Challenges

### 1. TCP Handshake Capture
```bash
# Terminal 1: Start a listener
nc -l -p 9999
# Terminal 2: Connect and capture
sudo tcpdump -i any -c 10 port 9999 -w handshake.pcap
nc localhost 9999
# Analyze (explain the SYN → SYN-ACK → ACK pattern)
```

### 2. Connection State Exploration
```bash
# Start a service that listens
nc -l -p 9999 &
# Check its state
ss -tlnp | grep 9999
# Connect to it
nc localhost 9999 &
# Check established state
ss -tnp | grep 9999
# All TCP states
ss -t -a
```

### 3. Port Scanning
```bash
# Check what ports are open on localhost
ss -tlnp
# Scan with nc
nc -zv localhost 22
nc -zv localhost 80
# Check why a port might be closed
```

### 4. TCP vs UDP
```bash
# TCP listener
nc -l -p 12345 &
# UDP listener
nc -u -l -p 12346 &
# Compare protocols
ss -tlnp | grep 12345
ss -ulnp | grep 12346
```

### 5. Build a Transport Layer Reference
Create `solution/transport-reference.md` covering:
- TCP header fields (source/dest port, seq/ack, flags)
- Connection states and transitions (SYN_SENT, ESTABLISHED, CLOSE_WAIT, TIME_WAIT)
- Common port numbers and their services (22, 80, 443, 5432, 6379, 27017)
- When to use TCP vs UDP

---

## Proof of Work

Save `solution/transport-analysis.md` with:
- tcpdump capture analysis (handshake sequence)
- Connection state descriptions
- Open ports on your system
- TCP vs UDP comparison table
