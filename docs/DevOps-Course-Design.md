# AI Powered DevOps & Platform Engineering — Complete Course Design
### A teachable curriculum: detailed subject index, learning objectives, labs, and assessments

This is the teaching companion to the 2026 syllabus. Where the syllabus says *what* to cover, this document breaks every phase into **modules → units → lessons → subtopics**, with learning objectives, hands-on labs, common pitfalls, and an assessment per module — so you can teach straight from it.

**How to read this:**
- **M0–M16** = teaching modules (they map 1:1 to syllabus phases).
- Each module lists **Learning Outcomes** (what the learner can *do* after), a **Detailed Subject Index** (the lecture skeleton), **Labs**, **Assessment**, **Analogy anchor** (open the module with this), and **Teach-the-trap** (the mistakes to pre-empt).
- ★ = senior differentiator. ★★ = frontier.
- Where a module maps to an industry certification, the aligned domains are noted so your content matches what the market tests.

---

## Course-level design

**Total teaching time (guideline):** ~120–160 instructional hours (lecture + lab), typically delivered as ~90 "days" of 1.5–2 hrs each, or a 16–20 week cohort.

**Delivery rhythm per lesson (use everywhere):**
`Analogy → Concept → Live demo → Hands-on lab → "Now in tech terms" recap → Micro-assessment`

**Prerequisite ladder (teach in this order; later modules assume earlier ones):**
```
M0 Foundations ─┬─> M1 Everything-as-Code ─> M2 CI/CD ─> M3 Containers ─> M4 IaC
                │
                └─> M5 K8s Core ─> M6 K8s Advanced ─> M7 GitOps
                                                        │
   M8 Observability <──────────────────────────────────┤
   M9 DevSecOps + Supply Chain ★ <──────────────────────┤
   M11 Cloud Platform <─────────────────────────────────┤
   M12 Platform Engineering ★ <── (needs M2,M4,M5,M7)
   M13 SRE <── (needs M8)
   M14 FinOps ★ <── (needs M4,M11)
   M15 AI-Assisted Ops ★ <── (needs M8)
   M16 Agentic Ops & Governance ★★ <── (needs M9,M15)
```

**Three graded capstones** gate progress: after M9 (Secure Delivery Platform), after M12+M14 (Self-Service IDP), after M16 (Agentic Incident Responder).

**Assessment mix:** 60% hands-on labs, 25% capstones, 15% concept checks. No pure multiple-choice for the technical core — the industry's own exams (CKA/CKS/CKAD) are performance-based, so mirror that.

---

# M0 — Foundations *(fast-track for experienced learners)*
**Duration:** 8–10 days · **Maps to:** general Linux/networking baseline

**Learning outcomes:** operate confidently at a Linux shell; explain what a container *is* at the kernel level; reason about a network path end-to-end; use Git beyond the happy path.

### Detailed subject index
**U0.1 — Linux internals**
- Boot & init: BIOS/UEFI → bootloader → kernel → `systemd`; unit files, targets, `journalctl`
- Processes: PID lifecycle, fork/exec, signals (SIGTERM vs SIGKILL), zombies/orphans, `nice`/priorities
- **Namespaces & cgroups** (the container substrate): pid/net/mnt/uts/ipc/user namespaces; cgroups v2 CPU/memory limits
- Filesystems: inodes, mounts, `/proc` & `/sys`, overlayfs, disk usage debugging
- Permissions: ugo/rwx, setuid/setgid/sticky, ACLs, capabilities (`CAP_NET_BIND_SERVICE`)
- Package & service management; log locations; `strace`/`lsof` for diagnosis

**U0.2 — Networking**
- OSI vs TCP/IP models; encapsulation
- IP addressing, CIDR, subnets, private ranges, NAT
- DNS resolution path: resolver → root → TLD → authoritative; record types; `dig`/`nslookup`
- TCP handshake, ports, sockets; UDP; connection states (`ss`, `netstat`)
- HTTP/1.1 vs 2 vs 3 (QUIC); headers, methods, status codes
- TLS handshake, certificates, chain of trust, SNI, mTLS preview
- Load balancing (L4 vs L7), reverse proxies, firewalls/iptables/nftables
- **eBPF intro:** what it is, why it underpins modern networking/observability/security

**U0.3 — Languages & tooling discipline**
- The one-language rule: Go for infra tooling, Python for automation/AI glue, Bash for glue scripts
- Bash: variables, conditionals, loops, functions, pipes, redirection, exit codes, `set -euo pipefail`
- Python for ops: virtualenv, requests, subprocess, argparse, working with JSON/YAML

**U0.4 — Git (deep)**
- Object model: blobs/trees/commits/refs; what a commit *is*
- Branching models (trunk-based vs GitFlow); rebase vs merge; interactive rebase
- Hooks (pre-commit framework); `.gitignore`; large-file handling
- Monorepo vs polyrepo trade-offs
- **Signed commits** (GPG/SSH signing) — set up now; it matters once agents also commit (M16)

**Labs:** (1) Provision a Linux VM, harden SSH (key-only, no root, fail2ban). (2) Trace a full DNS+TLS request with `dig` + `curl -v` + `openssl s_client`. (3) Write a Python backup + log-rotation utility; version it with pre-commit hooks and a signed commit.

**Assessment:** debug a deliberately broken VM (wrong permissions, dead service, full disk, DNS misconfig) against a checklist and time limit.

**Analogy anchor:** *Linux = your house* (root = master key, `/etc` = house manual, `/var/log` = CCTV, SSH = remote control). *Networking = the city postal system* (IP = address, DNS = contacts app, port = apartment number).

**Teach-the-trap:** learners think containers are VMs. Hammer that a container is *just a Linux process* with namespaces + cgroups — it's the single most clarifying idea in the course.

---

# M1 — Everything-as-Code Mindset
**Duration:** 2–3 days

**Learning outcomes:** articulate why declarative + idempotent + immutable beats manual ops; convert a manual process into a reviewed repo.

### Detailed subject index
**U1.1 — Core principles**
- Declarative vs imperative (describe the *what*, not the *how*)
- Idempotency (run it 5×, same result) and why it matters for automation
- Immutability (replace, don't mutate); cattle vs pets
**U1.2 — "…as code" everywhere**
- Config as code, docs as code, policy as code, cost as code, **guardrails as code** (the boundaries agents run inside — forward ref to M16)
**U1.3 — Change discipline**
- PR-driven change; review culture; approval gates; auditable history; conventional commits

**Lab:** take one manual runbook (e.g. "how we set up a new server") and move it entirely into a reviewed Git repo with a PR template and CODEOWNERS.

**Assessment:** peer-review exercise — critique a "works but manual" process and propose the as-code version.

**Analogy anchor:** *if it isn't in Git, it doesn't exist* — like a recipe nobody wrote down; it dies with the cook.

---

# M2 — CI/CD (Pipeline as Code)
**Duration:** 5–6 days

**Learning outcomes:** build a multi-stage pipeline from commit to production with security gates and a working rollback; explain the CI system as attack surface.

### Detailed subject index
**U2.1 — CI/CD fundamentals**
- CI vs CD (Delivery) vs CD (Deployment) — the three distinct things
- Pipeline anatomy: trigger → stages → jobs → steps → artifacts
- The correct stage order: **test → security scan → build → sign → deploy → health check → rollback**
**U2.2 — GitHub Actions (primary), GitLab CI, Jenkins (legacy)**
- Workflow syntax, events/triggers, runners (hosted vs self-hosted vs **ephemeral**)
- Matrix builds, caching, artifacts, reusable/composite workflows
- Secrets handling; environments & protection rules
- **OIDC-based cloud auth** — short-lived tokens, no long-lived secrets (teach *why* this kills a whole class of breaches)
**U2.3 — Artifacts & registries**
- Versioning strategies (semver, immutable tags), promotion between environments
**U2.4 — CI as attack surface**
- Pin actions to commit SHAs, not tags; review third-party actions like dependencies
- Dormant `workflow_dispatch` backdoors; throwaway CI-bot accounts; forged timestamps
- Least-privilege `GITHUB_TOKEN` / job permissions

**Lab:** pipeline that runs tests on every PR; on merge builds a Docker image, scans it (Trivy), signs it (Cosign — introduced properly in M9), pushes to a registry; includes a one-command rollback. Every third-party action pinned to a SHA.

**Assessment:** given a working pipeline, inject 3 supply-chain smells (unpinned action, over-broad token, missing scan) for the learner to find and fix.

**Analogy anchor:** *CI/CD = a car-factory assembly line* — raw code in one end, tested/signed artifact out the other; CI = quality check at every station that stops the belt on a defect; rollback = a recall.

**Teach-the-trap:** learners over-index on "make it deploy." Force them to make it *roll back* — that's the part production actually needs.

---

# M3 — Containers
**Duration:** 4–5 days

**Learning outcomes:** build small, hardened images; explain OCI; run rootless; scan in CI.

### Detailed subject index
**U3.1 — Docker fundamentals**
- Image vs container vs registry; layers & the union filesystem; the build cache
- Dockerfile mastery: instructions, layer ordering for cache hits, `.dockerignore`
- **Multi-stage builds**; **distroless / minimal base images**; image-size discipline
**U3.2 — Image hardening**
- Non-root `USER`; read-only root filesystem; dropped Linux capabilities; no secrets in layers; pinned base digests
**U3.3 — OCI & the ecosystem**
- OCI image/runtime specs; registries (ECR/GHCR/Harbor); tagging strategy; image scanning in CI (Trivy/Grype)
**U3.4 — Daemonless / rootless**
- **Podman** and rootless runtimes; why removing the root daemon shrinks blast radius; Podman as a drop-in for Docker workflows; `podman generate kube`

**Lab:** take a bloated app image, cut it 70%+ with multi-stage + distroless, harden it (non-root, read-only, dropped caps), scan it in CI, then rebuild/run it rootless with Podman and document what changed.

**Assessment:** grade an image against a hardening checklist + a size budget.

**Analogy anchor:** *Docker = a self-contained lunchbox* — food, utensils, everything packed; opens the same in any kitchen. Kills "works on my machine."

---

# M4 — Infrastructure as Code (IaC)
**Duration:** 6–7 days · **Maps to:** HashiCorp Terraform Associate (003) domains

**Learning outcomes:** provision reproducible multi-env infrastructure from modular code with remote state and a CI plan/apply gate; review machine-generated IaC.

### Detailed subject index
**U4.1 — IaC concepts & Terraform/OpenTofu basics** *(Assoc. domains 1–3)*
- Why IaC; declarative provisioning; the Terraform workflow: `init → plan → apply → destroy`
- HCL syntax: providers, resources, data sources, variables, outputs, locals, expressions, functions
- Provider & module versioning; the dependency graph
**U4.2 — State** *(Assoc. domain 7)*
- What state is and why it exists; **remote state** (S3+DynamoDB, TF Cloud); **locking**; workspace isolation; `terraform import`; sensitive data in state
**U4.3 — Modules & composition** *(Assoc. domain 5)*
- Authoring modules; input/output contracts; the registry; multi-environment layering (dev/stage/prod)
**U4.4 — Pulumi (alt) & Ansible (config mgmt)**
- Pulumi: IaC in real languages — when it helps
- **Ansible** for mutable estates / config management; idempotent playbooks, inventories, roles
**U4.5 — Testing & safety**
- `terraform plan` in CI as a gate; Terratest; drift detection; policy-as-code preview (OPA/Sentinel)
- **Reviewing AI-generated Terraform** — you're increasingly the human gate (forward ref M15)

**Lab:** provision a full multi-env cloud stack from modular IaC, remote state + locking, CI `plan` on PR and gated `apply` on merge.

**Assessment:** review a PR containing AI-generated Terraform with 2 subtle problems (a hardcoded secret, an over-permissive security group) and produce review comments.

**Analogy anchor:** *IaC = a blueprint + a robot builder* — hand over the plan, get an identical building every time; state = the "as-built" record the robot checks against.

**Teach-the-trap:** state corruption and manual console edits (drift). Make them break state on purpose and recover.

---

# M5 — Kubernetes (Core)
**Duration:** 8–10 days · **Maps to:** CKA v1.35 (Cluster Arch 25%, Workloads 15%, Storage 10%, Services & Networking 20%, Troubleshooting 30%)

**Learning outcomes:** deploy and operate a multi-service app on a real cluster; systematically troubleshoot (the heaviest-weighted CKA domain).

### Detailed subject index
**U5.1 — Architecture & installation** *(CKA 25%)*
- Control plane (api-server, etcd, scheduler, controller-manager) + node components (kubelet, kube-proxy, container runtime)
- `kubeadm` cluster bring-up; cluster lifecycle & upgrades; **RBAC**; Helm & Kustomize as install tooling; CRDs/Operators (intro; deep in M6)
**U5.2 — Workloads & scheduling** *(CKA 15%)*
- Pods, ReplicaSets, Deployments, StatefulSets, DaemonSets, Jobs/CronJobs
- ConfigMaps & Secrets; resource requests/limits; probes (liveness/readiness/startup)
- Scheduling: labels/selectors, affinity/anti-affinity, taints/tolerations, **HPA**; rolling updates & rollbacks
**U5.3 — Storage** *(CKA 10%)*
- Volumes; PV/PVC lifecycle; StorageClasses & dynamic provisioning; access modes; reclaim policies
**U5.4 — Services & networking** *(CKA 20%)*
- Service types (ClusterIP/NodePort/LoadBalancer); Endpoints; CoreDNS
- Ingress **and Gateway API** (new on the exam); NetworkPolicies (intro)
**U5.5 — Troubleshooting** *(CKA 30% — spend a third of the time here)*
- Systematic debugging: `get`/`describe`/`logs`/`exec`; where control-plane & kubelet logs live; `crictl`
- Pod failures (CrashLoopBackOff, ImagePullBackOff, pending/unschedulable), node issues, networking failures
**U5.6 — Packaging**
- **Helm** (charts, values, releases) and **Kustomize** (overlays); **Karpenter** for node autoscaling

**Lab:** deploy a multi-service app to a real cluster (EKS/AKS/GKE or kind), Helm-packaged, with ingress, autoscaling, and health probes. Then a timed troubleshooting drill (15 broken tasks, 2 hrs — CKA format).

**Assessment:** CKA-style performance test on a live cluster.

**Analogy anchor:** *Kubernetes = an orchestra conductor* — doesn't play an instrument; ensures 80 musicians play the right part in sync, and if one faints a backup steps in instantly (self-healing).

**Teach-the-trap:** learners memorize YAML but can't debug. Weight your labs the way the exam does — troubleshooting first.

---

# M6 — Kubernetes (Advanced) & Cloud-Native
**Duration:** 6–7 days

**Learning outcomes:** extend the cluster with a custom resource; run a service mesh with mTLS; enforce network policy with eBPF.

### Detailed subject index
**U6.1 — Extending Kubernetes**
- CRDs; the operator pattern & reconciliation loop; build a *simple* operator (Kubebuilder/Operator SDK)
**U6.2 — Service mesh**
- Istio or Linkerd; sidecar vs ambient/sidecarless; mTLS, traffic splitting, retries, timeouts, circuit breaking
**U6.3 — eBPF networking/security**
- **Cilium**: network policies, Hubble observability, replacing kube-proxy
**U6.4 — Multi-tenancy & isolation**
- Namespaces as tenancy boundaries; RBAC deep-dive; **Pod Security Standards** (privileged/baseline/restricted)

**Lab:** add a service mesh with mTLS between services; implement a canary traffic split; write a Cilium NetworkPolicy that denies-by-default and allow-lists one path.

**Assessment:** design + implement isolation for two "tenant" teams sharing a cluster; prove one can't reach the other's pods.

**Analogy anchor:** *Operator = a robot site-manager* you taught your building's rules to — it watches and keeps reality matching the blueprint without you.

---

# M7 — GitOps ★
**Duration:** 4–5 days

**Learning outcomes:** make Git the single source of truth for cluster state; implement progressive delivery with automated rollback.

### Detailed subject index
**U7.1 — GitOps principles**
- Declarative desired state in Git; continuous reconciliation; drift detection & auto-revert; pull vs push deploys
**U7.2 — ArgoCD / Flux**
- App definitions; **app-of-apps**; sync policies & waves; environment promotion via Git (not via clicking)
**U7.3 — Progressive delivery**
- **Argo Rollouts / Flagger**: canary, blue-green, metric-triggered automated rollback
**U7.4 — GitOps beyond apps**
- **Crossplane** to manage cloud resources through the same Git flow

**Lab:** a PR merge triggers ArgoCD to sync the cluster; then manually break the cluster (kubectl edit) and watch it self-heal / flag drift. Add a canary rollout that auto-rolls-back on an error-rate SLO breach.

**Assessment:** implement full environment promotion (dev→stage→prod) driven only by Git PRs.

**Analogy anchor:** *GitOps = a thermostat* — you set the desired temperature (state) in Git; the controller constantly works to make reality match, and fights you if you nudge it manually.

---

# M8 — Observability
**Duration:** 5–6 days

**Learning outcomes:** instrument a service for the three signals; define and alert on SLOs; produce the telemetry that AIOps (M15/M16) consumes.

### Detailed subject index
**U8.1 — The three signals & OpenTelemetry**
- Metrics vs logs vs traces; **OpenTelemetry** as the converged standard; instrumentation (auto + manual); the Collector; context propagation
**U8.2 — The stack**
- **Prometheus** (metrics, PromQL, exporters, service discovery); **Grafana** (dashboards, alerting); **Loki** (logs); **Tempo/Jaeger** (traces)
**U8.3 — SLIs / SLOs / error budgets**
- Choosing good SLIs; defining SLOs; **burn-rate alerting** (multi-window); the difference between symptom-based and cause-based alerts
**U8.4 — Telemetry hygiene for AIOps**
- Consistent tagging/labels, cardinality control, unified pipelines — *why scattered, untagged telemetry makes AI anomaly detection impossible*

**Lab:** instrument a service with OTel; ship traces + metrics + logs to a Prometheus/Grafana/Loki/Tempo stack; define one SLO with a burn-rate alert. **This lab's output is the input for the M15/M16 incident agent.**

**Assessment:** given a noisy system, cut alert volume by writing better SLO-based alerts and justify each.

**Analogy anchor:** *Observability = a hospital's vital-signs monitor* — not just "alive/dead" but heart rate, oxygen, trends, so you catch trouble before the flatline.

---

# M9 — DevSecOps & Supply Chain Security ★ *(go deepest — your specialty)*
**Duration:** 9–11 days · **Maps to:** CKS domains (Cluster Setup, Cluster Hardening, System Hardening, Minimize Microservice Vulnerabilities, Supply Chain Security, Monitoring/Logging & Runtime Security)

**Learning outcomes:** build security into every delivery stage; enforce artifact + model provenance at admission; threat-model the AI supply chain.

### Detailed subject index
**U9.1 — Shift-left scanning**
- **SAST**, **DAST**, **SCA** (dependency scanning), IaC scanning (tfsec/Checkov), container scanning (**Trivy**/Grype); severity gates in CI
**U9.2 — Secrets management**
- **HashiCorp Vault**, **External Secrets Operator**, **SOPS**; dynamic secrets; killing long-lived credentials via **OIDC / workload identity**
**U9.3 — Classic supply-chain security**
- **SBOMs** (generate with **Syft**; track components); dependency-confusion & typosquatting attacks
- **Artifact signing & provenance**: **Sigstore** = Cosign (sign/verify) + **Fulcio** (short-lived OIDC certs, *keyless*) + **Rekor** (transparency log); in-toto attestations
- **SLSA** build-provenance levels; field guidance: **Level 2** for most prod software, **Level 3** for high-risk/regulated (isolated signing secrets → even a malicious insider can't forge provenance)
- **Admission control / policy-as-code**: OPA/Gatekeeper, **Kyverno** — reject unsigned/unscanned images
**U9.4 — AI / ML supply chain (MLSecOps) ★**
- **Model provenance & signing**: the Open Model Signing spec (Sigstore-based signatures for model artifacts; sign a whole model dir via one signed manifest)
- **Serialization attacks**: why `pickle`/`torch.load` is a code-execution surface; prefer JSON / Protocol Buffers / ONNX / SavedModel; verify attestations at *every* consumption phase; pin to hashes not names
- **MCP servers as supply-chain components**: threat-model tool poisoning, prompt injection, over-broad tool scopes
- **Attestation forgery is real**: "signed" must mean "signed *and verified against an identity you trust*"
**U9.5 — Kubernetes hardening (CKS core)**
- CIS Benchmark review of components (etcd/kubelet/api-server); minimize service-account permissions & disable defaults
- Runtime sandboxes for multi-tenant nodes (**gVisor**, **Kata Containers**); Pod Security Standards enforcement
- **Runtime security**: **Falco** + eBPF behavioral detection of syscall/file/process anomalies
**U9.6 — Zero Trust posture**
- Least privilege everywhere; default-deny NetworkPolicies; image-provenance enforcement; mTLS (tie back to M6)

**Lab (capstone-grade):** a pipeline that generates an SBOM (Syft), signs the image (Cosign), *and* signs a bundled model artifact (Open Model Signing) — plus a cluster admission policy (Kyverno) that **rejects any unsigned image or model whose attestation doesn't verify against your trusted identity.**

**Assessment:** red-team a peer's pipeline — get an unsigned/malicious artifact to production; then patch the gap you found.

**Analogy anchor:** *DevSecOps = airport security end-to-end* — pack safely at home (SAST), X-ray at the gate (DAST), metal detector for weapons (secret scanning), air marshals in flight (runtime/Falco). *Model signing = a tamper-evident seal* — broken seal, don't consume.

**Teach-the-trap:** "signed = safe." Show a forged-attestation scenario so they learn to verify against a *trusted identity*, not merely check that a signature exists.

---

# M11 — Cloud Platform *(go deep on one)*
**Duration:** 6–8 days

**Learning outcomes:** design a production-grade, least-privilege environment in one cloud; reason about the model across all three.

### Detailed subject index
**U10.1 — Core services (pick AWS *or* Azure *or* GCP, literacy in the others)**
- Compute (VMs/instances, autoscaling groups), storage (object/block/file), networking (VPC/VNet, subnets, routing, peering, endpoints)
- **IAM deeply** — roles, policies, trust relationships, permission boundaries; *this is where breaches happen*
- Managed databases; managed Kubernetes (EKS/AKS/GKE); serverless (Lambda/Functions); managed CI/CD
**U10.2 — Architecture & governance**
- Well-Architected pillars; multi-account/multi-project org structure; landing zones; guardrails (SCPs/policies)
**U10.3 — Identity for non-humans**
- Workload identity / IRSA; **non-human identity** for agents & services (forward ref M16) — design human + NHI together

**Lab:** design and deploy a production-grade, least-privilege cloud environment for the running course app (network segmentation, IAM least privilege, managed K8s, secrets via workload identity).

**Assessment:** IAM audit exercise — find and fix over-privilege in a given account.

**Analogy anchor:** *Cloud IAM = a building's key-card system* — every person and robot gets exactly the doors they need, nothing more, and every swipe is logged.

---

# M12 — Platform Engineering ★
**Duration:** 6–7 days

**Learning outcomes:** build an Internal Developer Platform with a self-service golden path that bakes in your security + cost defaults.

### Detailed subject index
**U11.1 — IDP concepts**
- Platform-as-a-product; developers as customers; **golden paths / paved roads**; reducing cognitive load; DX as a first-class metric
**U11.2 — Backstage & the portal**
- Service catalog; software templates (scaffolding); TechDocs; plugins
**U11.3 — Self-service provisioning**
- **Crossplane** for platform APIs over cloud resources; composition; claims
**U11.4 — AI-native platforms**
- Golden paths that ship with agent hooks + policy-as-code baked in; the secure/cheap way becomes the *default* way

**Lab (capstone-grade):** a Backstage "new service" template that scaffolds repo + pipeline + manifests + observability, with your M9 security defaults (signing, scanning, NHI, default-deny network policy) and M14 cost tags baked into the golden path — so the *secure & cost-tagged* way is the *one-click* way.

**Assessment:** a "developer" (peer) provisions a compliant service via your portal in under 10 minutes without reading docs — measure the friction.

**Analogy anchor:** *Platform engineering = building highways instead of making every driver pave their own road* — everyone gets where they're going faster, and on roads you designed to be safe.

---

# M13 — Site Reliability Engineering (SRE)
**Duration:** 4–5 days

**Learning outcomes:** run reliability as an engineering discipline; execute an incident and a blameless postmortem.

### Detailed subject index
**U12.1 — SLOs as decision tools**
- Error budgets as the throttle between velocity and stability; toil identification & reduction
**U12.2 — Incident response**
- On-call, severity levels, incident commander role, comms; **blameless postmortems** & action items
**U12.3 — Chaos engineering**
- Hypothesis-driven fault injection; game days; blast-radius control
**U12.4 — Capacity & performance**
- Capacity planning; load testing; saturation signals

**Lab:** run a **game day** — inject a failure into the M8-instrumented service, respond via a runbook, write a blameless postmortem. **The runbook becomes grounding/training data for the M16 incident agent.**

**Assessment:** grade the postmortem on quality of action items, not blame.

**Analogy anchor:** *SRE = a hospital with service-level promises* — "99.9% of patients treated within 4 hours"; when the error budget runs low, elective work pauses and resources go to emergencies.

---

# M14 — FinOps ★
**Duration:** 3–4 days

**Learning outcomes:** make cost a delivery-stage signal; surface cost deltas before merge; track AI/token spend.

### Detailed subject index
**U13.1 — Cost visibility**
- Tagging/allocation; showback vs chargeback; unit economics
**U13.2 — Cost in the workflow**
- **Infracost** — monthly cost delta as a PR comment, *before* merge
- **Kubecost** — Kubernetes spend attribution; rightsizing; spot/savings plans
**U13.3 — Cost-aware architecture & AI spend**
- Cost reviews as a design gate; **tracking inference/token cost** for any AI in the pipeline (LLM spend scales nonlinearly and surprises people)

**Lab:** add Infracost to the PR pipeline so every infra change comments its monthly cost delta; add a Kubecost dashboard for the running cluster.

**Assessment:** given an over-provisioned cluster + a costly IaC change, produce a rightsizing + cost-reduction plan with dollar figures.

**Analogy anchor:** *FinOps = a shared household budget app* — everyone sees what their choices cost *before* they buy, so the bill at month-end is never a shock.

---

# M15 — AI-Assisted Ops / AIOps ★
**Duration:** 5–6 days

**Learning outcomes:** put AI to work as an *assistant* (human decides) across logs, incidents, and IaC; build a RAG-grounded incident assistant in shadow mode.

### Detailed subject index
**U14.1 — AI-assisted operations**
- LLM-driven log analysis; incident timeline/summarization; pipeline-failure diagnosis; runbook drafting; deployment-risk review
**U14.2 — Predictive / anomaly detection**
- Using metrics/traces/history to warn *before* outages; baselining "normal"
**U14.3 — AI copilots for IaC**
- Generating/reviewing Terraform, K8s manifests, Helm charts — **human validation, never blind-apply** (ties to M4 review skills)
**U14.4 — LLM integration patterns**
- **MCP** (structured context/tools) as the "USB-C for AI"; **RAG over your own configs/runbooks/logs** so the model uses *your* environment
- **MLOps/LLMOps basics**: model/endpoint deployment, versioning, monitoring, drift, guardrails

**Lab:** build an AI incident-assistant that pulls recent logs/metrics (RAG over the M8 telemetry) and drafts an incident summary + probable root cause — **human approves before any action ("shadow mode": the agent proposes, you dispose).**

**Assessment:** evaluate the assistant's accuracy on 5 seeded incidents; measure false-root-cause rate.

**Analogy anchor:** *AI-assisted ops = a very fast junior analyst* who reads all the logs in seconds and hands you a summary — you still make the call.

**Teach-the-trap:** blind-apply of AI-generated IaC/manifests. Make "human validates" a hard rule with a lab that includes a plausible-but-wrong AI suggestion.

---

# M16 — Agentic Ops & AI Governance ★★ *(the frontier)*
**Duration:** 6–8 days · **Where your security-architect background is an unfair advantage**

**Learning outcomes:** move from AI-assisted to *bounded-autonomous*; design the identity, guardrails, and audit that let an agent act safely; red-team an agent.

### Detailed subject index
**U15.1 — Agentic architecture**
- The three layers: **planner** (chooses next action toward a goal), **tools** (run a query, open a PR, restart a service), **memory** (what's been tried/worked)
- **ReAct loop**: Observe → Analyze → Plan → Act (via MCP within boundaries) → Reflect (verify; retry modified on failure)
- **Multi-agent coordination** (LangGraph / CrewAI / AutoGen): specialized triage / coding / security / infra agents daisy-chained vs one generalist model
- **Pipeline-as-specification & collaborative remediation**: pipeline validates against acceptance criteria; feeds failures back to the authoring agent for auto fix-and-re-verify; humans intervene only past an attempt budget
**U15.2 — Governance & security (the part most teams get wrong — your wheelhouse)**
- **Non-Human Identity (NHI)**: every agent gets its own identity, least-privilege scopes, and an audit trail of tool calls & MCP queries; role separation (remediation agent can't read customer data; observability agent can't modify infra)
- **Human-in-the-loop gates**: any action with blast radius beyond one service needs approval; agents surface recommended actions with confidence + impact
- **Circuit breakers**: fail-safes so a misjudgment can't cascade; repeated failed remediations escalate to humans
- **Treat agent inputs as hostile**: prompt injection via alert text / log entries / external data (OWASP Top 10 for LLM apps); sanitize & constrain like user input
- **Agent attestation**: cryptographically bind each agent-authored commit to the agent, model version, spec given, and the human who authorized it (ties to M0 signed commits + M9 provenance)
**U15.3 — The adoption ladder (how disciplined teams roll this out)**
1. **Shadow mode** — proposes fixes in Slack/Jira; humans execute & score accuracy
2. **Bounded autonomy** — acts on low-risk, well-defined, reversible tasks (dependency bumps, log rotation, drift remediation)
3. **Expanded autonomy** — widen scope only as measured trust grows; never remove the audit log or circuit breaker

**Lab (capstone-grade):** take the M15 shadow-mode assistant and give it *bounded* authority over exactly one low-risk, reversible task (e.g. auto-open a PR for a flagged dependency update). Give it a dedicated NHI with least privilege, log every step, gate merge on human approval, add a circuit breaker that escalates after 2 failed attempts. **Then red-team it:** try to prompt-inject it through a poisoned log line.

**Assessment:** the red-team result *is* the grade — did the learner's guardrails hold, and can they explain precisely what the agent was allowed to touch and prove it from the audit log?

**Analogy anchor:** *An AI agent = a capable new contractor.* You wouldn't hand a day-one contractor admin on every system with no review and no expiry. NHI is their badge; least privilege is their key-card zones; the audit log is the sign-in sheet; the circuit breaker is security escorting them out when they keep tripping alarms. *MCP = USB-C for AI.* *Prompt injection via logs = a forged note slipped into an inbox the agent was told to trust.*

**Teach-the-trap:** giving the agent broad permissions "just to get it working." The entire discipline is the opposite — start with almost nothing and widen on evidence.

---

# The three capstones (portfolio + assessment gates)

**Capstone 1 — End-to-end Secure Delivery Platform** *(after M9; covers M2–M9)*
App → GitHub Actions (test/scan/sign, SHA-pinned actions) → GitOps deploy via ArgoCD → K8s with service mesh → OTel observability → SLO alerting → Cosign-enforced admission that rejects unsigned artifacts.

**Capstone 2 — Self-Service IDP** *(after M12+M14)*
Backstage golden-path template that provisions a compliant, observable, cost-tagged service in one click, security defaults baked in.

**Capstone 3 — Agentic Incident Responder** *(after M16 — the flagship)*
RAG-based incident/log analysis integrated into the on-call flow, promoted from shadow mode to bounded autonomy on one reversible action — with NHI, human-approval gate, full audit trail, and a documented red-team of its prompt-injection surface. *This is the single project that proves you can design autonomous systems safely — the exact thing 2026 hiring is short on.*

---

# Certification alignment master table

Every module in this course is aligned to one or more industry certifications or frameworks. This table shows the complete mapping so learners know which certs each module prepares them for.

## Primary Certification Alignments

| Module | Tool / Topic | Aligned Cert / Framework | Coverage | Gaps |
|--------|-------------|-------------------------|----------|------|
| **M01** | Linux | **[LFS101](https://training.linuxfoundation.org/training/introduction-to-linux/)** — Linux Foundation Intro to Linux | ✅ Full (Ch 1-18) | None |
| **M02** | Everything-as-Code | None (conceptual foundation) | — Not applicable | No cert exists for this mindset module |
| **M01** | Git (Days 22-25) | **[GitHub Foundations](https://learn.microsoft.com/en-us/credentials/certifications/github-foundations/)** | 🟡 4 of 7 domains | Domains 4 (Actions — covered in M03), 5 (Projects), 7 (Community) |
| **M03** | CI/CD | **[GitHub Actions](https://resources.github.com/learn/pathways/)** learning pathway | ✅ U2.2 full coverage | None for Actions; CI patterns transfer to GitLab CI |
| **M03** | CI/CD | **GitLab CI Associate** | 🟡 Patterns transfer across tools | Lab uses Actions, not GitLab |
| **M04** | Containers (Docker) | **[DCA](https://training.mirantis.com/certification/dca-certification-exam/)** — Docker Certified Associate | 🟡 5 of 6 domains | Domain 1 (Swarm — covered by K8s in M06-M07) |
| **M05** | IaC (Terraform) | **[Terraform Associate (003/004)](https://developer.hashicorp.com/terraform/tutorials/certification)** | ✅ All 9 domains | TF Cloud depth (add if pursuing cert) |
| **M06** | Kubernetes Core | **[CKA](https://www.cncf.io/training/certification/cka/)** (v1.35) | ✅ All 5 domains (Arch 25%, Workloads 15%, Storage 10%, Networking 20%, Troubleshooting 30%) | None |
| **M06** | Kubernetes (dev focus) | **CKAD** (optional alt) | 🟡 De-emphasizes troubleshooting, adds design | Choose one track |
| **M07 + M10** | K8s Security | **[CKS](https://www.cncf.io/training/certification/cks/)** (requires CKA first) | ✅ All 6 domains (Cluster Setup 10%, Hardening 15%, System Hardening 15%, Microservice Vulns 20%, Supply Chain 20%, Runtime Security 20%) | None — M10 is our strongest cert-aligned module |
| **M08** | GitOps | **Argo CD** / **Flux** certifications | 🟡 Core concepts covered (U7.1-U7.4) | Vendor-specific CLI flags need study guide |
| **M10** | DevSecOps Pipeline | **[OWASP DevSecOps Guideline](https://owasp.org/www-project-devsecops-guideline/)** | 🟡 Framework (SAST, DAST, SCA, IaC scanning, secrets) | OWASP is a framework, not a certification |
| **M10** | DevSecOps Lifecycle | **[OWASP DSOVS](https://owasp.org/www-project-devsecops-verification-standard/)** | 🟡 Framework (secret mgmt, provenance, AI supply chain) | OWASP is a framework, not a certification |
| **M09** | Observability | **Prometheus / Grafana** certs; **[OTel](https://opentelemetry.io/certification/)** | 🟡 Covers the three signals + OTel | No single unified observability cert |
| **M11** | Cloud Platform | **AWS SAA** / **Azure AZ-104** / **GCP PCA** | 🟡 Covers core services + IAM + K8s | Cloud-specific depth; pick one |
| **M12** | Platform Engineering | None (emerging field) | 🟡 Patterns-based; no formal cert exists | Backstage/Crossplane have vendor pathways |
| **M13** | SRE | **Google SRE cert** / DevOps Institute | 🟡 Covers SLOs, incident response, chaos | Theory-heavy, less hands-on tooling coverage |
| **M14** | FinOps | **[FinOps Foundation](https://www.finops.org/certification/)** cert | 🟡 Covers cost visibility, tagging, Kubecost | No cloud billing deep-dive (varies by provider) |
| **M15** | AIOps | None (emerging field) | 🟡 Covers AI-assisted ops, RAG, MCP | Field evolving rapidly — no stable cert yet |
| **M16** | Agentic Ops | **Emerging AI-security / MCP-hardening** credentials | 🟡 New & scarce — highest signal value while rare | Field is evolving rapidly |

## Prerequisite Path for Kubernetes Certs

```
KCNA (entry level) ─→ CKA ─→ CKS
  └─→ CKAD (dev focus)     (requires CKA)
```

## Certification Value by Role

| Role | Highest-Value Certs |
|------|-------------------|
| **DevOps Engineer** | CKA + Terraform Associate + GitHub Foundations |
| **Security Architect** | CKS + OWASP DSOVS + Emerging AI-security |
| **Platform Engineer** | CKA + Terraform Associate + ArgoCD |
| **SRE** | CKA + Prometheus/Grafana certs |
| **AI/Automation Engineer** | Emerging AI-security + MCP-hardening |

> 💡 **Principle:** Master the *pattern*, pick one tool per category. Certifications validate knowledge, but the patterns (declarative IaC, observability signals, least-privilege security) transfer across tools. Focus on understanding first, cert second.

Foundational entry point for absolute beginners: **[KCNA / KCSA](https://www.cncf.io/training/certification/kcna/)** before CKA/CKS.

---

# Teaching delivery kit (apply across all modules)

**Per-module opening:** lead with the analogy anchor, *then* the concept. Close with a "now in tech terms" block mapping the analogy to exact commands.

**Retention mechanics to build into the platform:**
- "I'm stuck" escape hatches: *Simplified version* of hard labs, *Skip & come back*, progressive *Hint* button
- "Today's Challenge" banner (start-date based); module **dependency map** (the graph above) so experienced learners skip confidently
- Per-lesson GitHub Discussions thread (peer solution gallery — no backend needed)
- Companion 2–3 min concept videos; print-friendly cheatsheets per module; completion badge for LinkedIn
- "Week in Review" reflection prompt (what was hardest / how would you explain it to a friend) — doubles as interview prep

**Highest-priority content to build first (if starting from a partial course):**
1. Terraform module (M4) — nearly every JD lists it
2. Observability module (M8) — separates "knows Docker" from "DevOps engineer"
3. End-to-end capstone — the #1 interview asset
4. Then the differentiators: M9 AI supply chain, M16 agentic ops — what makes the course *current*, not just complete
