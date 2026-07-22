# M05 — Infrastructure as Code (IaC)

**Duration:** 6-7 days  
**Cert alignment:** HashiCorp Terraform Associate (003)  

## Learning outcomes
- Provision reproducible multi-env infra from modular code with remote state + CI gate
- Review machine-generated IaC

## Analogy anchor (open with this)
IaC = a blueprint + a robot builder. Hand over the plan, get an identical building every time; state = the as-built record.

## Teach-the-trap
State corruption & manual console edits (drift). Make them break state on purpose and recover.

## Units
1. [Concepts & Terraform/OpenTofu basics](U01-concepts-and-terraform-opentofu-basics/)
2. [State](U02-state/)
3. [Modules & composition](U03-modules-and-composition/)
4. [Pulumi & Ansible](U04-pulumi-and-ansible/)
5. [Testing & safety](U05-testing-and-safety/)

## Assessment / milestone
_Fill in the module's graded lab or capstone gate._

---

## 🎓 Certification Alignment

This module maps to the **[HashiCorp Terraform Associate (003/004)](https://developer.hashicorp.com/terraform/tutorials/certification)** exam:

| Domain | Our Coverage |
|--------|-------------|
| 1 — Understand IaC Concepts | U4.1 (why IaC, declarative provisioning) |
| 2 — Understand Terraform vs Other Tools | U4.4 (Pulumi, Ansible comparison) |
| 3 — Understand Terraform Basics (HCL, providers) | U4.1 (HCL syntax, resources, data sources, variables, outputs) |
| 4 — Core Terraform Workflow | U4.1 (init → plan → apply → destroy) |
| 5 — Interact with Terraform Modules | U4.3 (authoring, registry, multi-env) |
| 6 — Implement and Maintain State | U4.2 (remote state, locking, workspaces, sensitive data) |
| 7 — Read, Generate, Modify Configuration | U4.1 (HCL functions, expressions, dependencies, meta-arguments) |
| 8 — Understand Terraform Cloud / Enterprise | U4.5 (CI plan/apply gate, Sentinel/OPA policy) |
| 9 — Use Terraform Outside Core Workflow | U4.2 (import), U4.5 (drift detection) |

> 💡 The Lab in U4.5 covers **CI `plan` on PR and gated `apply` on merge** — the exact workflow tested on the exam.