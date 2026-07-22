# Day 14 — Reference Solution

## DNS Record Types Reference
| Type | Purpose | Example Dig Command |
|------|---------|---------------------|
| A | IPv4 address | `dig +short A google.com` |
| AAAA | IPv6 address | `dig +short AAAA google.com` |
| CNAME | Canonical alias | `dig CNAME www.github.com` |
| MX | Mail servers | `dig MX gmail.com` |
| TXT | Text/SPF/DKIM | `dig TXT google.com` |
| NS | Name servers | `dig NS google.com` |
| SOA | Start of Authority | `dig SOA google.com` |
| PTR | Reverse DNS | `dig -x 8.8.8.8` |

## DNS Resolution Chain
```
Application (browser → curl)
  → /etc/hosts check (if match → done)
  → system resolver (/etc/resolv.conf → DNS server)
  → Recursive resolver (ISP or 8.8.8.8)
    → Root server (.) — knows where .com TLD is
    → TLD server (.com) — knows where google.com NS is
    → Authoritative server (ns1.google.com) — gives IP
  → IP returned to application
```

## Troubleshooting Flow
```
"Domain not resolving?"
  → ping <domain>              → Fails? → DNS or network
  → nslookup <domain>          → Fails? → DNS issue
  → dig +short <domain> @8.8.8.8  → Works? → local DNS resolver
  → cat /etc/resolv.conf       → Check DNS servers
  → hostname -f                → Check system DNS config
  → dig +trace <domain>        → Where does it break?
```
