# M10 — DevSecOps & Supply Chain Security

> ★ **differentiator - go deepest**

**Duration:** 9-11 days  
**Cert alignment:** CKS  

## Learning outcomes
- Build security into every delivery stage
- Enforce artifact + model provenance at admission
- Threat-model the AI supply chain

## Analogy anchor (open with this)
DevSecOps = airport security end-to-end (pack safely=SAST, X-ray=DAST, metal detector=secret scanning, air marshals=runtime/Falco). Model signing = a tamper-evident seal.

## Teach-the-trap
'Signed = safe.' Show a forged-attestation scenario so they verify against a trusted identity, not just presence of a signature.

## Units
1. [Shift-left scanning](U01-shift-left-scanning/)
2. [Secrets management](U02-secrets-management/)
3. [Classic supply-chain security](U03-classic-supply-chain-security/)
4. [AI / ML supply chain (MLSecOps) - 2026](U04-ai-ml-supply-chain-mlsecops-20265/)
5. [Kubernetes hardening (CKS core)](U05-kubernetes-hardening-cks-core/)
6. [Zero Trust posture](U06-zero-trust-posture/)

## Assessment / milestone
_Fill in the module's graded lab or capstone gate._

---

## 🎓 Certification Alignment

This module is **the strongest cert-aligned module in the course**, mapping to both **[CKS](https://www.cncf.io/training/certification/cks/)** and **[OWASP DevSecOps](https://owasp.org/www-project-devsecops-guideline/)** frameworks:

### CKS (Certified Kubernetes Security Specialist)

| CKS Domain | Weight | Our Coverage |
|-----------|--------|-------------|
| 1 — Cluster Setup | 10% | U9.5 (CIS Benchmark, etcd, api-server hardening) |
| 2 — Cluster Hardening | 15% | U9.2 (RBAC, OIDC), U9.5 (service account minimization) |
| 3 — System Hardening | 15% | U9.5 (gVisor, Kata, Pod Security Standards) |
| 4 — Minimize Microservice Vulnerabilities | 20% | U9.1 (SAST, DAST, SCA, Trivy), U9.6 (default-deny NetworkPolicies) |
| 5 — Supply Chain Security | 20% | U9.3 (SBOMs, Cosign, SLSA, Kyverno), U9.4 (MLSecOps, model signing) |
| 6 — Monitoring, Logging, Runtime Security | 20% | U9.5 (Falco, eBPF), U9.6 (mTLS, runtime behavioral detection) |

### OWASP DevSecOps Frameworks

| Framework | Our Coverage |
|-----------|-------------|
| **[OWASP DevSecOps Guideline](https://owasp.org/www-project-devsecops-guideline/)** | U9.1 (SAST, DAST, SCA, IaC scanning, credential scanning), U9.2 (secrets), U9.3 (supply chain) |
| **[OWASP DSOVS](https://owasp.org/www-project-devsecops-verification-standard/)** | U9.2 (secret management), U9.3 (artifact provenance), U9.4 (AI supply chain), U9.6 (zero trust) |

> 💡 **Prerequisite:** CKS requires CKA certification first. Our M06 covers CKA; M10+M07 together cover CKS.