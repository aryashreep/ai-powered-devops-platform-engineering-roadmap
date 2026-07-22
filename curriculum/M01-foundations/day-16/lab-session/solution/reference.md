# Day 15 — Reference Solution

## TCP Three-Way Handshake
```
CLIENT                    SERVER
  │    SYN (seq=x)          │
  │────────────────────────>│
  │    SYN-ACK (seq=y,ack=x+1) │
  │<────────────────────────│
  │    ACK (seq=x+1,ack=y+1)  │
  │────────────────────────>│
  │   [Connection Established]  │
```

## TCP Flags
| Flag | Name | Meaning |
|------|------|---------|
| SYN | Synchronize | Initiate connection |
| ACK | Acknowledge | Confirm receipt |
| FIN | Finish | Graceful close |
| RST | Reset | Abort connection |
| PSH | Push | Deliver immediately |
| URG | Urgent | Priority data |

## Common TCP States
| State | Meaning |
|-------|---------|
| LISTEN | Waiting for connection |
| SYN_SENT | Client sent SYN, waiting for reply |
| ESTABLISHED | Connection active |
| FIN_WAIT_1 | Waiting for FIN from remote |
| CLOSE_WAIT | Remote closed, waiting for local close |
| TIME_WAIT | Waiting before closing (2×MSL) |
| CLOSED | Connection terminated |

## Quick Port Reference
| Port | Service | Protocol |
|------|---------|----------|
| 22 | SSH | TCP |
| 80 | HTTP | TCP |
| 443 | HTTPS | TCP |
| 5432 | PostgreSQL | TCP |
| 6379 | Redis | TCP |
| 27017 | MongoDB | TCP |
| 53 | DNS | TCP + UDP |
| 161 | SNMP | UDP |
