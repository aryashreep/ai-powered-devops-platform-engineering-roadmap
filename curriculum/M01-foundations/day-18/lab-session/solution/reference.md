# Day 17 — Reference Solution

## L4 vs L7 Load Balancer Comparison
| Feature | L4 (Transport) | L7 (Application) |
|---------|----------------|-------------------|
| Operates at | TCP/UDP layer | HTTP/HTTPS layer |
| Routing based on | IP + Port | URL, headers, cookies |
| Example | AWS NLB, HAProxy TCP | AWS ALB, NGINX, HAProxy HTTP |
| Performance | Higher (less inspection) | Lower (more inspection) |
| Features | Simple, fast | Path routing, sticky sessions, SSL termination |

## CDN Request Flow
```
1. User requests example.com/image.jpg
2. DNS resolves to CDN edge POP (closest location)
3. Edge POP checks cache:
   - Cache HIT → serve immediately (if TTL valid)
   - Cache MISS → forward to origin
4. Origin returns image with Cache-Control: max-age=3600
5. Edge POP caches it for 3600 seconds
6. Edge POP serves to user
```

## eBPF Explained
**eBPF** (extended Berkeley Packet Filter) lets you run sandboxed programs in the Linux kernel without changing kernel code or loading modules.

**Tools built on eBPF:**
| Tool | What it does |
|------|-------------|
| Cilium | Kubernetes networking, security, observability |
| Falco | Runtime security (intrusion detection) |
| Pixie | In-cluster Kubernetes observability |
| bcc/bpftrace | Tracing and performance analysis |
