# Day 13 — Reference Solution

## OSI ↔ TCP/IP Layer Mapping
| OSI Layer | TCP/IP Layer | Example Protocols | Data Unit |
|-----------|-------------|-------------------|-----------|
| 7 Application | Application | HTTP, DNS, SSH | Data |
| 6 Presentation | Application | TLS/SSL (crypto) | Data |
| 5 Session | Application | SOCKS, RPC | Data |
| 4 Transport | Transport | TCP, UDP | Segment |
| 3 Network | Internet | IP, ICMP | Packet |
| 2 Data Link | Link | Ethernet, WiFi | Frame |
| 1 Physical | Link | CAT6, Fiber | Bits |

## CIDR Calculations
| CIDR | Total IPs | Usable Hosts | Subnet Mask | Network | Broadcast |
|------|-----------|-------------|-------------|---------|-----------|
| /24 | 256 | 254 | 255.255.255.0 | .0 | .255 |
| /16 | 65536 | 65534 | 255.255.0.0 | .0.0 | .255.255 |
| /20 | 4096 | 4094 | 255.255.240.0 | varies | varies |

## Private IP Ranges (RFC 1918)
- **10.0.0.0/8**: 10.0.0.0 – 10.255.255.255 (large orgs)
- **172.16.0.0/12**: 172.16.0.0 – 172.31.255.255 (AWS default VPC)
- **192.168.0.0/16**: 192.168.0.0 – 192.168.255.255 (home/SOHO)

## Packet Journey: ping google.com
```
1. Application: ping generates ICMP echo request
2. Transport: ICMP embedded in IP (no TCP/UDP port)
3. Network: IP packet created with src=192.168.1.10 dst=142.250.80.14
4. Link: Ethernet frame with src/dst MAC addresses
5. Physical: Bits on wire → router → ISP → Google
6. Google responds — reverse journey
```
