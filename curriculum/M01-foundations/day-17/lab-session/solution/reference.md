# Day 16 — Reference Solution

## HTTP Methods
| Method | Purpose | Idempotent | Safe |
|--------|---------|------------|------|
| GET | Retrieve resource | ✅ | ✅ |
| HEAD | Check resource (headers only) | ✅ | ✅ |
| POST | Create resource | ❌ | ❌ |
| PUT | Replace resource | ✅ | ❌ |
| PATCH | Partial update | ❌ | ❌ |
| DELETE | Remove resource | ✅ | ❌ |

## Status Code Categories
| Code | Category | Examples |
|------|----------|----------|
| 1xx | Informational | 100 Continue, 101 Switching Protocols |
| 2xx | Success | 200 OK, 201 Created, 204 No Content |
| 3xx | Redirection | 301 Moved Permanently, 302 Found, 304 Not Modified |
| 4xx | Client Error | 400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found, 429 Too Many Requests |
| 5xx | Server Error | 500 Internal Server Error, 502 Bad Gateway, 503 Service Unavailable, 504 Gateway Timeout |

## TLS Handshake (Simplified)
```
Client                     Server
  │  ClientHello (ciphers, TLS version)  │
  │─────────────────────────────────────>│
  │  ServerHello (chosen cipher, cert)   │
  │<─────────────────────────────────────│
  │  Certificate verification            │
  │  (check CA, expiry, hostname)        │
  │  Key Exchange (ECDHE)                │
  │─────────────────────────────────────>│
  │  Finished (encrypted)                │
  │<─────────────────────────────────────│
  │  [Secure Connection Established]     │
```

## Key curl Flags
| Flag | Purpose |
|------|---------|
| `-v` | Verbose (full request/response) |
| `-I` | Headers only |
| `-L` | Follow redirects |
| `-H` | Custom header |
| `-d` | Request body (POST data) |
| `-o` | Save output to file |
| `-k` | Skip TLS verification (⚠️ dev only) |
