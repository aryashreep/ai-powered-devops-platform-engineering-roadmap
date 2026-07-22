# Curriculum — AI Powered DevOps & Platform Engineering

_Generated 07-16._

Structure: `curriculum/ → module/ → unit/ → lesson.md` (subtopics as sections).


## Module Dependency Map

```
                    ┌──────────────────────────────────────────────────────┐
                    │                   M01 Foundations                    │
                    │  (Linux · Networking · Scripting · Git)             │
                    └──────────┬───────────────────────────┬───────────────┘
                               │                           │
              ┌────────────────┼────────────────┬──────────┼──────────────┐
              ▼                ▼                ▼          ▼              ▼
       ┌──────────┐    ┌──────────┐    ┌──────────┐  ┌──────────┐  ┌──────────┐
       │  M02     │    │  M03     │    │  M04     │  │  M09     │  │  M11     │
       │  EaC     │    │  CI/CD   │    │Containers│  │Observab. │  │  Cloud   │
       └────┬─────┘    └────┬─────┘    └────┬─────┘  └────┬─────┘  └──────────┘
            │               │               │             │              │
            │        ┌──────┼──────┐        │       ┌─────┼──────┐       │
            ▼        ▼      ▼      ▼        ▼       ▼     ▼      ▼       ▼
       ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐
       │  M05     │  │  M08     │  │  M06     │  │  M13     │  │  M12     │
       │   IaC    │  │  GitOps  │  │ K8s Core │  │   SRE    │  │Platform  │
       └────┬─────┘  └────┬─────┘  └────┬─────┘  └────┬─────┘  └────┬─────┘
            │             │             │             │             │
            ▼             ▼             ▼             ▼             ▼
       ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐
       │  M11(*)  │  │  M12(*)  │  │  M07     │  │  M15     │  │  M14     │
       │  Cloud   │  │Platform  │  │K8s Adv.  │  │  AIOps   │  │  FinOps  │
       └──────────┘  └──────────┘  └──────────┘  └────┬─────┘  └──────────┘
                                                      │
                                                      ▼
                                               ┌──────────┐
                                               │  M16     │
                                               │AgenticOps│
                                               └──────────┘
```

### Prerequisites at a Glance

| Module | Prerequisites | Why |
|--------|---------------|-----|
| **M01** Foundations | — | Starting point. Linux, networking, git, and scripting basics. |
| **M02** Everything-as-Code | M01 | Declarative vs imperative, idempotency, PR-driven change. |
| **M03** CI/CD | M01, M02 | Pipelines need git fluency and as-code mindset. |
| **M04** Containers | M01 | Docker builds on Linux process/filesystem concepts. |
| **M05** IaC | M01, M02 | Terraform/Pulumi are declarative — EaC mindset required. |
| **M06** K8s Core | M01, M04 | Kubernetes orchestrates containers; needs Linux foundations. |
| **M07** K8s Advanced | M06 | CRDs, service mesh, eBPF extend core K8s. |
| **M08** GitOps | M01, M03, M04, M06 | GitOps deploys containers via pipelines onto K8s. |
| **M09** Observability | M01, M04 | OTel, metrics, logs run on containers & Linux. |
| **M10** DevSecOps | M01, M03, M04, M06 | Supply-chain security needs CI/CD, containers, and K8s context. |
| **M11** Cloud Platform | M01, M05 | Cloud provisioning uses IaC; IAM builds on Linux concepts. |
| **M12** Platform Engineering | M01, M05, M08 | IDP golden paths use IaC + GitOps patterns. |
| **M13** SRE | M01, M09 | SLOs and incident response require observability data. |
| **M14** FinOps | M05 | Cost visibility needs IaC-managed infra to tag and allocate. |
| **M15** AI-Assisted Ops | M01, M09, M13 | AIOps needs telemetry (M09) and SRE workflows (M13). |
| **M16** Agentic Ops | M01, M10, M15 | Agent governance builds on supply-chain security and AIOps. |

### Suggested Sequencing

**Complete beginner (linear path):**
```
M01 → M02 → M03 → M04 → M05 → M06 → M07 → M08 → M09 → M10
   → M11 → M12 → M13 → M14 → M15 → M16
```

**Experienced / security-architect fast path:**
```
M01(skim) → M02(skim) → M03 → M04 → M05 → M06
   → M08 (GitOps — differentiator)
   → M10 (DevSecOps + AI Supply Chain — go deepest)
   → M09 (Observability) → M12 (Platform Engineering)
   → M15 (AI-Assisted Ops) → M16 (Agentic Ops & Governance)
   → M07, M13, M14 as depth-ups
```

---

## Modules

### [M01 — Foundations](M01-foundations/) · 8-10 days · cert: Linux/networking baseline

#### Linux Internals
- day-01 — L01 — Linux Foundation & Architecture
- day-02 — L02 — Essential Commands
- day-03 — L04 — File System Hierarchy
- day-04 — L04 — Text Editors
- day-05 — L03 — Text Files & Redirection
- day-06 — L05 — Boot & Init
- day-07 — L06 — Processes
- day-08 — L07 — User & Group Management
- day-09 — L08 — Permissions & Ownership
- day-10 — L09 — SSH & Remote Access
- day-11 — L10 — Diagnosis & Troubleshooting
- day-12 — L11 — Namespaces & Cgroups
- day-13 — L12 — Filesystems & LVM

#### Networking
- day-14 — L01 — Models & Addressing
- day-15 — L02 — DNS
- day-16 — L03 — Transport (TCP/UDP)
- day-17 — L04 — HTTP & TLS
- day-18 — L05 — Edge & eBPF

#### Automation & Scripting
- day-19 — L01 — The One-Language Rule
- day-20 — L02 — Bash for Ops
- day-21 — L03 — Python for Ops

#### Git
- day-22 — L01 — Git Object Model
- day-23 — L02 — Branching & History
- day-24 — L03 — Hygiene
- day-25 — L04 — Signed Commits

---

### [M02 — Everything-as-Code Mindset](M02-everything-as-code-mindset/) · 2-3 days

#### U01 — Core Principles
- L01 — Declarative vs Imperative
- L02 — Idempotency
- L03 — Immutability

#### U02 — As-Code Everywhere
- L01 — The Spectrum

#### U03 — Change Discipline
- L01 — PR-Driven Change

---

### [M03 — CI/CD (Pipeline as Code)](M03-ci-cd-pipeline-as-code/) · 5-6 days

#### U01 — CI/CD Fundamentals
- L01 — The Three CDs
- L02 — Pipeline Anatomy
- L03 — Correct Stage Order

#### U02 — Tooling
- L01 — GitHub Actions (Primary)
- L02 — Advanced Workflows
- L03 — Secrets & Auth
- L04 — GitLab CI & Jenkins

#### U03 — Artifacts & Registries
- L01 — Versioning & Promotion

#### U04 — CI as Attack Surface
- L01 — Hardening the Pipeline
- L02 — Known Attack Patterns

---

### [M04 — Containers](M04-containers/) · 4-5 days

#### U01 — Docker Fundamentals
- L01 — Core Objects
- L02 — Dockerfile Mastery
- L03 — Small Images

#### U02 — Image Hardening
- L01 — Hardening Checklist

#### U03 — OCI & Ecosystem
- L01 — Standards & Registries
- L02 — Scanning

#### U04 — Daemonless / Rootless
- L01 — Podman

---

### [M05 — Infrastructure as Code (IaC)](M05-infrastructure-as-code-iac/) · 6-7 days · cert: HashiCorp Terraform Associate (003)

#### U01 — Concepts & Terraform/OpenTofu Basics
- L01 — Why IaC & The Workflow
- L02 — HCL
- L03 — Versioning & Graph

#### U02 — State
- L01 — State Fundamentals

#### U03 — Modules & Composition
- L01 — Authoring Modules

#### U04 — Pulumi & Ansible
- L01 — Pulumi
- L02 — Ansible

#### U05 — Testing & Safety
- L01 — Guardrails
- L02 — Reviewing AI-Generated Terraform

---

### [M06 — Kubernetes (Core)](M06-kubernetes-core/) · 8-10 days · cert: CKA v1.35

#### U01 — Architecture & Installation (CKA 25%)
- L01 — Cluster Components
- L02 — Lifecycle
- L03 — Install Tooling

#### U02 — Workloads & Scheduling (CKA 15%)
- L01 — Workload Objects
- L02 — Config & Health
- L03 — Scheduling

#### U03 — Storage (CKA 10%)
- L01 — Persistence

#### U04 — Services & Networking (CKA 20%)
- L01 — Service & DNS
- L02 — Ingress & Gateway API

#### U05 — Troubleshooting (CKA 30%)
- L01 — Systematic Debugging
- L02 — Common Failures

#### U06 — Packaging
- L01 — Helm & Kustomize
- L02 — Node Autoscaling

---

### [M07 — Kubernetes (Advanced) & Cloud-Native](M07-kubernetes-advanced-and-cloud-native/) · 6-7 days · cert: feeds CKS

#### U01 — Extending Kubernetes
- L01 — CRDs & Operators

#### U02 — Service Mesh
- L01 — Mesh Fundamentals

#### U03 — eBPF Networking & Security
- L01 — Cilium

#### U04 — Multi-Tenancy & Isolation
- L01 — Isolation Boundaries

---

### [M08 — GitOps](M08-gitops/) · 4-5 days · cert: Argo CD / Flux ★ _differentiator_

#### U01 — GitOps Principles
- L01 — Core Model

#### U02 — ArgoCD & Flux
- L01 — App Management

#### U03 — Progressive Delivery
- L01 — Safe Rollouts

#### U04 — GitOps Beyond Apps
- L01 — Cloud Resources

---

### [M09 — Observability](M09-observability/) · 5-6 days

#### U01 — Three Signals & OpenTelemetry
- L01 — OTel

#### U02 — The Stack
- L01 — Metrics & Dashboards
- L02 — Logs & Traces

#### U03 — SLIs / SLOs / Error Budgets
- L01 — Reliability Targets

#### U04 — Telemetry Hygiene for AIOps
- L01 — Making Telemetry AI-Ready

---

### [M10 — DevSecOps & Supply Chain Security](M10-devsecops-and-supply-chain-security/) · 9-11 days · cert: CKS ★ _differentiator — go deepest_

#### U01 — Shift-Left Scanning
- L01 — The Scan Matrix

#### U02 — Secrets Management
- L01 — Killing Long-Lived Creds

#### U03 — Classic Supply Chain Security
- L01 — SBOMs
- L02 — Signing & Provenance
- L03 — SLSA Levels
- L04 — Admission Control

#### U04 — AI/ML Supply Chain (MLSecOps)
- L01 — Model Provenance & Signing
- L02 — Serialization Attacks
- L03 — MCP Servers as Supply Chain Components
- L04 — Attestation Forgery is Real

#### U05 — Kubernetes Hardening (CKS Core)
- L01 — Cluster & Component Hardening
- L02 — Sandboxing & Runtime

#### U06 — Zero Trust Posture
- L01 — Least Privilege Everywhere

---

### [M11 — Cloud Platform (go deep on one)](M11-cloud-platform-go-deep-on-one/) · 6-8 days · cert: cloud provider assoc/pro

#### U01 — Core Services
- L01 — Compute, Storage, Network
- L02 — IAM Deeply
- L03 — Managed Services

#### U02 — Architecture & Governance
- L01 — Well-Architected & Org Structure

#### U03 — Identity for Non-Humans
- L01 — Workload & Agent Identity

---

### [M12 — Platform Engineering](M12-platform-engineering/) · 6-7 days ★ _differentiator_

#### U01 — IDP Concepts
- L01 — Platform as a Product

#### U02 — Backstage & The Portal
- L01 — Developer Portal

#### U03 — Self-Service Provisioning
- L01 — Crossplane

#### U04 — AI-Native Platforms
- L01 — Baked-In Intelligence

---

### [M13 — Site Reliability Engineering (SRE)](M13-site-reliability-engineering-sre/) · 4-5 days

#### U01 — SLOs as Decision Tools
- L01 — Error Budgets

#### U02 — Incident Response
- L01 — Running an Incident

#### U03 — Chaos Engineering
- L01 — Fault Injection

#### U04 — Capacity & Performance
- L01 — Planning

---

### [M14 — FinOps](M14-finops/) · 3-4 days ★ _differentiator_

#### U01 — Cost Visibility
- L01 — Allocation

#### U02 — Cost in the Workflow
- L01 — Shift-Left Cost

#### U03 — Cost-Aware Architecture & AI Spend
- L01 — Design Gate & AI Cost

---

### [M15 — AI-Assisted Ops / AIOps](M15-ai-assisted-ops-aiops/) · 5-6 days ★ _differentiator_

#### U01 — AI-Assisted Operations
- L01 — Daily Ops Assists

#### U02 — Predictive Anomaly Detection
- L01 — Warning Before Outages

#### U03 — AI Copilots for IaC
- L01 — Generate & Review

#### U04 — LLM Integration Patterns
- L01 — MCP & RAG
- L02 — MLOps / LLMOps Basics

---

### [M16 — Agentic Ops & AI Governance](M16-agentic-ops-and-ai-governance/) · 6-8 days · cert: emerging AI-security creds ★ _frontier_

#### U01 — Agentic Architecture
- L01 — The Three Layers
- L02 — ReAct Loop
- L03 — Multi-Agent Coordination
- L04 — Pipeline as Specification

#### U02 — Governance & Security
- L01 — Non-Human Identity (NHI)
- L02 — Human-in-the-Loop Gates
- L03 — Circuit Breakers
- L04 — Agent Inputs are Hostile
- L05 — Agent Attestation

#### U03 — The Adoption Ladder
- L01 — Shadow Mode
- L02 — Bounded Autonomy
- L03 — Expanded Autonomy

---

## Capstones (assessment gates)

- **Capstone 1 — End-to-end Secure Delivery Platform** — _after M10 (covers M03-M10)_ — App → GitHub Actions (test/scan/sign, SHA-pinned) → GitOps via ArgoCD → K8s with service mesh → OTel observability → SLO alerting → Cosign-enforced admission rejecting unsigned artifacts.
- **Capstone 2 — Self-Service IDP** — _after M12 + M13_ — Backstage golden-path template that provisions a compliant, observable, cost-tagged service in one click, security defaults baked in.
- **Capstone 3 — Agentic Incident Responder (flagship)** — _after M15_ — RAG-based incident/log analysis in the on-call flow, promoted from shadow mode to bounded autonomy on one reversible action, with NHI, human-approval gate, full audit trail, and a documented prompt-injection red-team.

## How to teach (per lesson)

`Story → Real-world Problem → Theory → Live Demo → Common Mistakes → Interview Questions → Assignment`

**Every lecture ends with:** Takeaway · Interview Question · Assignment · Challenge · Mistake · Pro Tip
