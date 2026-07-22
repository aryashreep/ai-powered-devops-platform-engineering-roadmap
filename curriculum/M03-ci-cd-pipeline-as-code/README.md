# M03 — CI/CD (Pipeline as Code)

**Duration:** 5-6 days  

## Learning outcomes
- Build a multi-stage pipeline commit-to-prod with security gates and a working rollback
- Explain the CI system as attack surface

## Analogy anchor (open with this)
CI/CD = a car-factory assembly line. Code in one end, tested/signed artifact out the other; CI stops the belt on a defect; rollback = a recall.

## Teach-the-trap
Learners over-index on 'make it deploy'. Force them to make it roll back.

## Units
1. [CI/CD fundamentals](U01-ci-cd-fundamentals/)
2. [Tooling](U02-tooling/)
3. [Artifacts & registries](U03-artifacts-and-registries/)
4. [CI as attack surface (2026)](U04-ci-as-attack-surface-2026/)

## Assessment / milestone
_Fill in the module's graded lab or capstone gate._

---

## 🎓 Certification Alignment

This module aligns with the **[GitHub Actions](https://resources.github.com/learn/pathways/)** and general CI/CD certification paths:

| Certification | Our Coverage |
|--------------|-------------|
| **GitHub Actions** — GitHub's official pathway | U2.2 (workflow syntax, events, runners, matrix builds, caching, secrets, OIDC) |
| **GitLab CI Associate** — GitLab's certification path | U2.2 (parallel coverage — patterns transfer across tools) |
| **Supply-chain security** — CI as attack surface (2026) | U2.4 (SHA-pinned actions, token least-privilege, dormant backdoors) |

> 💡 **Key principle:** Rather than cert alignment, this module teaches the **pattern** of pipeline-as-code. The patterns (stages, gates, artifacts, OIDC) transfer across GitHub Actions, GitLab CI, Jenkins, and ArgoCD Workflows. Master the pattern; the syntax is searchable.