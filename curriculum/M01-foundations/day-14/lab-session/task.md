# 🧪 Lab: Network Models & IP Addressing

> **Goal:** Master the conceptual models and addressing schemes that underpin all networking.

## Target Objectives

- Map the OSI layers to TCP/IP layers
- Calculate subnets and network ranges
- Identify private vs public IPs
- Trace encapsulation with tcpdump/ping

---

## Hands-on Challenges

### 1. Layer Mapping
Create a table mapping OSI layers (7-1) to TCP/IP layers. For each layer, list:
- Example protocols
- Data unit name (data, segment, packet, frame, bit)
- A real-world analogy

### 2. CIDR Calculation
Given: `192.168.1.0/24`
- How many addresses? How many usable hosts?
- What's the network address? Broadcast address?
- What's the subnet mask in dotted decimal?

Repeat for: `10.0.0.0/16` and `172.16.0.0/20`

### 3. Private IP Identification
Which of these are private (RFC 1918)?
```
10.0.0.1      8.8.8.8      172.16.0.1
192.168.1.1   203.0.113.5  172.32.0.1
```
Explain why NAT is needed for the public ones.

### 4. Trace Encapsulation
```bash
# Ping a remote host with increased MTU info
ping -M do -s 1472 google.com
# Trace the route
traceroute google.com
# Capture with tcpdump (install if needed)
sudo tcpdump -i any -c 10 icmp
```
Describe what happens at each layer when you type `ping google.com`.

### 5. Your Network Profile
```bash
ip addr show
ip route show
cat /etc/resolv.conf
```
Map your machine's IP, subnet, gateway, and DNS server into the TCP/IP model.

---

## Proof of Work

Save `solution/networking-models-report.md` with:
- OSI/TCP layer mapping table
- CIDR calculations for all 3 networks
- Private IP identification answers
- Encapsulation trace description
- Your machine's network profile
