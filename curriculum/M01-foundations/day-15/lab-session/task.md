# 🧪 Lab: DNS Deep Dive

> **Goal:** Master DNS resolution, query types, and troubleshooting.

## Target Objectives

- Trace DNS resolution from local resolver to authoritative server
- Query every record type with dig
- Diagnose common DNS failures
- Configure /etc/hosts for local overrides

---

## Hands-on Challenges

### 1. Basic Resolution
```bash
# System resolver
nslookup google.com
# Direct dig query
dig google.com
# Short answer only
dig +short google.com
# Trace the resolution path
dig +trace google.com
```

### 2. Record Type Exploration
Query these record types for popular domains and document the results:
```bash
dig A google.com          # IPv4
dig AAAA google.com       # IPv6
dig MX gmail.com          # Mail servers
dig CNAME www.github.com  # Canonical name
dig TXT google.com        # Text records (SPF, DKIM)
dig NS google.com         # Name servers
```

### 3. DNS Troubleshooting
```bash
# Query a specific DNS server
dig @8.8.8.8 google.com
# Reverse DNS lookup
dig -x 8.8.8.8
# Check propagation
dig +short google.com @a.gtld-servers.net
```

### 4. /etc/hosts Configuration
```bash
# Add a local override
echo "127.0.0.1   myapp.local" | sudo tee -a /etc/hosts
# Test
ping myapp.local
nslookup myapp.local
# Remove the entry
sudo sed -i '/myapp.local/d' /etc/hosts
```

### 5. Build a DNS Diagnostics Card
Create `solution/dns-reference.md` containing a table of all record types with examples and common use cases. Also include a flowchart for troubleshooting "domain not resolving."

---

## Proof of Work

Save `solution/dns-investigation.md` with:
- dig +trace output (first 20 lines)
- At least 4 different record types with examples
- DNS troubleshooting flowchart
- The exact query to check if a domain's MX records have propagated
