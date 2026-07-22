# 🧪 Lab: HTTP & TLS Exploration

> **Goal:** Master HTTP debugging with curl and understand TLS encryption.

## Target Objectives

- Use curl for full HTTP request/response inspection
- Understand status codes and methods
- Inspect TLS certificates and handshake
- Trace HTTPS connections

---

## Hands-on Challenges

### 1. HTTP Basics with curl
```bash
# Basic GET
curl -v https://example.com
# Show only response headers
curl -I https://example.com
# POST with data
curl -X POST -H "Content-Type: application/json" \
  -d '{"name":"test"}' https://httpbin.org/post
# Follow redirects
curl -L http://httpbin.org/redirect/3
```

### 2. Status Code Exploration
```bash
# 200 OK
curl -I https://httpbin.org/status/200
# 301 Moved
curl -I https://httpbin.org/status/301
# 403 Forbidden
curl -I https://httpbin.org/status/403
# 500 Server Error
curl -I https://httpbin.org/status/500
```

### 3. TLS Inspection
```bash
# Certificate details
openssl s_client -connect google.com:443 -servername google.com \
  < /dev/null 2>/dev/null | openssl x509 -text -noout
# Check certificate dates
echo | openssl s_client -connect google.com:443 -servername google.com 2>/dev/null | \
  openssl x509 -noout -dates
# TLS version negotiation
curl -v --tlsv1.2 https://google.com
```

### 4. Headers & Caching
```bash
# Request specific headers
curl -H "Accept: application/json" https://httpbin.org/headers
# Inspect cache headers
curl -I https://www.google.com
```

### 5. Build an HTTP Reference Card
Create `solution/http-reference.md` with:
- All HTTP methods and their uses
- Status code categories (1xx-5xx) with 3 examples each
- Common request/response headers (Content-Type, Authorization, Cache-Control, etc.)
- TLS handshake steps

---

## Proof of Work

Save `solution/http-tls-report.md` with:
- curl -v output with annotations explaining each part
- Certificate details for google.com (issuer, validity, subject)
- Status code examples documented
- Your HTTP reference card
