# M04 — Containers

**Duration:** 4-5 days  

## Learning outcomes
- Build small, hardened images
- Explain OCI
- Run rootless
- Scan in CI

## Analogy anchor (open with this)
Docker = a self-contained lunchbox — food, utensils, everything packed; opens the same in any kitchen. Kills 'works on my machine'.

## Teach-the-trap
Learners ship bloated root images. Enforce a size budget + hardening checklist.

## Units
1. [Docker fundamentals](U01-docker-fundamentals/)
2. [Image hardening](U02-image-hardening/)
3. [OCI & ecosystem](U03-oci-and-ecosystem/)
4. [Daemonless / rootless (2026)](U04-daemonless-rootless-2026/)

## Assessment / milestone
_Fill in the module's graded lab or capstone gate._

---

## 🎓 Certification Alignment

This module aligns with the **[Docker Certified Associate (DCA)](https://training.mirantis.com/certification/dca-certification-exam/)** exam:

| DCA Domain | Weight | Our Coverage |
|-----------|--------|-------------|
| 1 — Orchestration (Swarm + K8s) | 25% | U3.1 (intro), covered fully in M06-M07 |
| 2 — Image Creation, Management, Registry | 20% | U3.1 (Dockerfile mastery), U3.2 (hardening), U3.3 (registries) |
| 3 — Installation and Configuration | 15% | U3.1 (core objects), U3.3 (OCI ecosystem) |
| 4 — Networking | 15% | U3.1 (basics), covered in M01 Days 14-18 |
| 5 — Security | 15% | U3.2 (image hardening), U3.4 (rootless/Podman) |
| 6 — Storage and Volumes | 10% | U3.1 (volumes, bind mounts), covered in M06 Storage |

> **Note:** DCA Domain 1 (Orchestration) is covered by our dedicated Kubernetes modules (M06-M07) rather than Swarm, reflecting industry migration to K8s. For DCA exam prep, supplement with Swarm-specific material.

**Also maps to:** [CNCF CKA](https://www.cncf.io/training/certification/cka/) — container runtime fundamentals underpin K8s troubleshooting, which is 30% of CKA.