# Day 18 — Reference Solution

## Language Decision Tree
```
"What am I automating?"
├── A shell command sequence → Bash
├── An API or data pipeline
│   ├── Simple, one-off → Bash + curl
│   └── Complex, maintainable → Python
├── A system tool or service
│   ├── Performance-critical → Go
│   └── Prototype → Python
└── Need to run everywhere → Bash (most portable)
```

## Language × DevOps Task Matrix
| Task | Best Language | Why |
|------|--------------|-----|
| Deploy script | Bash | Just run shell commands |
| Manage cloud APIs | Python | boto3, SDK support |
| Write CLI tool | Go | Single binary, cross-compile |
| Parse logs | Bash + awk | One-liner pipeline |
| Webhook handler | Python | Quick, request lib |
| Container/mesh | Go | Kubernetes itself is Go |
| Config management | Python/Go | Ansible (Python), Terraform (Go) |

## Real-World Tools by Language
| Language | Tools |
|----------|-------|
| Go | Docker, Kubernetes, Terraform, Prometheus, Helm |
| Python | Ansible, AWS CDK, Airflow, pytest |
| Bash | Docker entrypoints, CI scripts, git hooks |
