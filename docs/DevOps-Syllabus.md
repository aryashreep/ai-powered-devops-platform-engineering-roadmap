# AI Powered DevOps & Platform Engineering Syllabus
### The Agentic & AI-Native Revision

This builds directly on your 2026 syllabus, which was already ahead of most curricula. Rather than rewrite it, this edition does three things:

1. **Applies the 2026 deltas** the field actually moved on since the last edition — the shift from AI *copilots* to AI *agents*, the arrival of non-human identity as a first-class security problem, AI/ML supply-chain security, and daemonless containers.
2. **Sharpens the security-architect track** — where your threat-modeling instinct is now a hard differentiator, not a nice-to-have.
3. **Adds a delivery/pedagogy layer** — the analogy-first teaching structure, capstones, and "escape hatches" that separate a syllabus people *finish* from one they abandon on Day 18.

> **The one-line summary of what changed:** In the last edition, "AI for DevOps" meant a human using Copilot to write a YAML file. In this edition, AI *agents* open PRs, remediate incidents, and act inside guardrails you design — which means **the DevOps engineer's job is shifting from writing automation to designing the control loops and trust boundaries that autonomous systems run inside.** That is squarely security-architect work.

---

## What's new (the deltas worth your time)

If you already know the 2026 edition, read only this section plus Phases 9, 14, and 15.

- **AIOps 2.0 — from suggestion to action.** The industry has moved past the "copilot" phase into the "agentic" phase: agents now have *delegated authority* and execute fixes within pre-defined boundaries, not just suggest them. Roughly three-quarters of DevOps teams had some AI in their CI/CD pipelines by 2025, and that share is climbing. The role is reframing from "pipeline mechanic" to "system designer" who defines guardrails, policies, and control loops.
- **Platform engineering is now the operating model, not a trend.** Analysts put internal developer platform (IDP) adoption at ~80% of large software orgs by end of 2026, up from ~55% a year earlier. Platform engineers reportedly earn ~27% more than generalist DevOps engineers; DevSecOps specialists more still.
- **Non-Human Identity (NHI) governance is the new IAM frontier.** The guidance is explicit: assign every AI agent a non-human identity, grant least privilege, and audit its tool calls and MCP queries exactly as you would a human developer's access. The most damaging 2025–2026 incidents came through over-privileged agents and stolen agent OAuth tokens, not password guessing.
- **The software supply chain now includes AI.** Securing it in 2026 means controlling source + dependencies, build pipelines, binary artifacts, *AI model weights (MLSecOps)*, and *MCP servers*. Model serialization attacks (malicious pickle files) and forged SLSA/Sigstore attestations were live, in-the-wild techniques in 2026 — including self-spreading worms targeting AI coding tools. This is your home turf; go deepest here.
- **Daemonless / rootless containers.** Podman and rootless runtimes gained real ground as a security-without-overhead default. Worth knowing alongside Docker.
- **Pipeline-as-specification & collaborative remediation loops.** Pipelines are starting to validate implementations against *specifications* (acceptance criteria), and to feed failures back to the agent that authored the code for an automatic fix-and-re-verify cycle — human intervention only when the agent can't resolve it within N attempts.

Everything else in the 2026 edition still holds. Linux, networking, and systems thinking still don't change.

---

## How to use this (unchanged, still true)

- **Complete beginner:** ~12–18 months, every phase in order.
- **Experienced dev / sysadmin / architect (you):** ~4–9 months. Skim Phase 0–1, go deep on the **★ Differentiators**: Platform Engineering (11), GitOps (7), DevSecOps + AI Supply Chain (9), and Agentic Ops (14–15).
- **Principle:** master the *pattern*, pick one tool per category. Tools churn; patterns don't.

> **Security-architect fast path:** You own Phase 0, IAM, and most of Phase 12 already. Your highest-ROI, highest-differentiation work is: **Phase 9 (supply chain + AI supply chain), Phase 15 (agent governance / NHI), Phase 7 (GitOps), and Phase 11 (platform engineering).** These are the four places where "designs the platform" pulls away from "operates the platform."

---

## Phase 0 — Foundations *(fast-track if experienced)*

**Focus:** the substrate everything runs on. Skipping this is why engineers burn hours on a `CrashLoopBackOff` others fix in minutes.

- **Linux internals:** processes, namespaces, cgroups (containers *are* Linux processes), systemd, filesystems, permissions, signals.
- **Networking:** TCP/IP, DNS, HTTP/1.1 vs 2 vs 3, TLS, load balancing, NAT, subnets, firewalls. Intro to **eBPF** (basis of modern networking/observability/security).
- **One-language rule:** **Go** for infra tooling, **Python** for automation and AI glue, Bash for scripting. Don't collect languages.
- **Git (deep):** branching, rebase vs merge, hooks, monorepo vs polyrepo, **signed commits** (matters more now that agents also commit — see Phase 15).

**Milestone:** provision a Linux VM, harden SSH, script a backup + log-rotation utility in Python, version it in Git with pre-commit hooks and signed commits.

---

## Phase 1 — Everything-as-Code Mindset

**Focus:** *if it isn't in Git, it doesn't exist.*

- Declarative vs imperative; idempotency; immutability.
- Config as code, docs as code, policy as code, cost as code — and now **guardrails as code** (the boundaries agents operate within).
- PR-driven change; review, approval gates, auditable history.

**Milestone:** move one manual process (a server setup, a runbook) entirely into a reviewed Git repo.

---

## Phase 2 — CI/CD (Pipeline as Code)

**Focus:** the automated path from commit to production.

- **GitHub Actions** (industry standard), GitLab CI; Jenkins (legacy but common in enterprises).
- Stages done right: **test → security scan → build → sign → deploy → health check → rollback**.
- Artifact management & versioning; reusable workflow templates.
- Matrix builds, caching, ephemeral runners, **OIDC-based cloud auth (no long-lived secrets)**.
- **★ New:** treat your CI system itself as attack surface. The 2026 attack wave included thousands of malicious GitHub Actions workflow commits and dormant `workflow_dispatch` backdoors. Pin actions to commit SHAs, not tags; review third-party actions like dependencies.

**Milestone:** a pipeline that tests every PR, builds + scans + signs a Docker image on merge, pushes to a registry, with a working rollback — and every third-party action pinned to a SHA.

---

## Phase 3 — Containers

**Focus:** packaging and running workloads consistently.

- **Docker:** multi-stage builds, layer caching, image-size optimization, **distroless / minimal base images**.
- Image hardening: non-root users, read-only filesystems, dropped capabilities.
- OCI standards, registries (ECR/GHCR/Harbor), tagging strategy, image scanning in CI.
- **★ New — daemonless/rootless:** **Podman** and rootless runtimes. Understand *why* removing the root daemon shrinks blast radius, and where Podman drops into a Docker-shaped workflow.

**Milestone:** take a bloated image, cut it 70%+ with multi-stage + distroless, hardened and CI-scanned. Then rebuild/run it rootless with Podman and note what changed.

---

## Phase 4 — Infrastructure as Code (IaC)

**Focus:** infrastructure that's reproducible, reviewable, testable.

- **Terraform / OpenTofu** (track the open-source fork), **Pulumi** (IaC in real languages).
- State management (remote state, locking, isolation), modules, workspaces, multi-env layering.
- **Ansible** for config management / mutable estates.
- Testing IaC (Terratest, `terraform plan` in CI), drift detection.
- **★ Note:** AI copilots are now decent at *generating* Terraform. That raises, not lowers, the value of being able to *review* it — you're increasingly the human gate on machine-written IaC (see Phase 14).

**Milestone:** provision a full multi-env (dev/stage/prod) stack from modular IaC, remote state, CI plan/apply gate.

---

## Phase 5 — Kubernetes (Core)

**Focus:** the operating system of the cloud.

- Pods, Deployments, ReplicaSets, Services, Ingress, ConfigMaps, Secrets, Jobs/CronJobs.
- **Helm** (packaging) and **Kustomize** (overlays).
- Autoscaling: HPA/VPA, and **Karpenter** for efficient node autoscaling.
- Resource requests/limits, probes, rolling updates.

**Milestone:** deploy a multi-service app to a real cluster (EKS/AKS/GKE or kind), Helm-packaged, with ingress, autoscaling, health probes.

---

## Phase 6 — Kubernetes (Advanced) & Cloud-Native

**Focus:** managing the cluster, not just deploying to it.

- **CRDs & Operators** — extend K8s for your domain (build a simple operator).
- **Service mesh:** Istio or Linkerd — mTLS, traffic splitting, retries.
- **eBPF networking/security:** Cilium (network policies, observability).
- Multi-tenancy, namespaces, RBAC, pod security standards.

**Milestone:** add a service mesh with mTLS and a canary traffic split.

---

## Phase 7 — GitOps ★ *Differentiator*

**Focus:** Git as the single source of truth for cluster state.

- **ArgoCD** / **Flux** — desired-state reconciliation, drift detection, auto-revert.
- App-of-apps patterns; environment promotion via Git.
- **Progressive delivery:** Argo Rollouts / Flagger — canary, blue-green, metric-triggered rollback.
- Extending GitOps to cloud resources with **Crossplane**.

**Milestone:** a PR merge triggers ArgoCD to sync the cluster; then manually break the cluster and watch it self-heal / flag drift.

---

## Phase 8 — Observability

**Focus:** understanding system health *and customer impact*, not just "is it up." Also: observability is the fuel AIOps runs on — agents can't detect anomalies in telemetry that's scattered and untagged.

- **OpenTelemetry** (converged standard for traces/metrics/logs) — instrument an app.
- **Prometheus** (metrics), **Grafana** (dashboards), **Loki** (logs), **Tempo/Jaeger** (traces).
- Three pillars + the shift to unified telemetry.
- **SLIs / SLOs / error budgets** — define and alert on burn rate.

**Milestone:** instrument a service with OTel, ship traces + metrics + logs to a stack, define an SLO with a burn-rate alert. (This milestone becomes the *input* for the Phase 15 agent.)

---

## Phase 9 — DevSecOps & Supply Chain Security ★ *Differentiator — go deepest here*

**Focus:** security as a built-in stage of delivery, not a gate at the end. Your home turf.

**Shift-left scanning:** SAST, DAST, SCA (dependency scanning), IaC scanning (tfsec/Checkov), container scanning (Trivy/Grype).

**Secrets management:** HashiCorp Vault, External Secrets Operator, SOPS; kill long-lived credentials (OIDC / workload identity).

**Supply-chain security (classic):**
- **SBOMs** — generate with Syft; track components.
- **Artifact signing & provenance** — Sigstore / **Cosign** (keyless signing via Fulcio OIDC certs, logged in Rekor's transparency log), in-toto attestations.
- **SLSA framework** — build-provenance levels. Rule of thumb from the field: Level 2 for most production software, Level 3 for high-risk/regulated. Level 3's isolated signing secrets mean even a malicious insider with build-config access can't forge provenance.
- **Admission control / policy-as-code** — OPA/Gatekeeper, **Kyverno**.

**Runtime security:** Falco, eBPF-based detection.

**★ New sub-track — AI / ML supply chain (MLSecOps):** *This is the 2026 expansion and it's the sharpest edge of your specialty.*
- **Model provenance & signing:** the Open Model Signing spec (Sigstore-based signatures for model artifacts — the ML equivalent of code signing; can sign a whole model directory via one signed manifest).
- **Serialization attacks:** why `pickle` / `torch.load` is a code-execution surface, and why JSON / Protocol Buffers / ONNX / SavedModel are safer. Verify attestations at *every* consumption phase; pin to hashes, not names.
- **MCP server security:** MCP servers are now supply-chain components. Threat-model tool poisoning, prompt injection, and over-broad tool scopes.
- **Attestation forgery is real:** treat "signed" as "signed *and verified against an identity you trust*." Forged SLSA/Sigstore attestations were used in the wild in 2026 to make malicious extensions look legitimate.

**Zero Trust, least privilege, network policies, image-provenance enforcement.**

**Milestone (revised):** a pipeline that generates an SBOM, signs the image with Cosign, *and* signs a bundled model artifact with the Open Model Signing flow — plus a cluster admission policy that **rejects any unsigned image or model whose attestation doesn't verify against your trusted identity.**

---

## Phase 10 — Cloud Platform *(go deep on one)*

**Focus:** deep competence in one cloud, literacy in the model.

- **AWS / Azure / GCP** core: compute, storage, networking (VPC/VNet), **IAM** (deeply — where breaches happen), managed databases.
- Managed Kubernetes (EKS/AKS/GKE), serverless (Lambda/Functions), managed CI/CD.
- Well-Architected principles; multi-account/multi-project org structure.
- **★ Note:** IAM now has a sibling problem — *non-human* identity for agents and workloads (Phase 15). Design them together.

**Milestone:** design and deploy a production-grade, least-privilege cloud environment for your app.

---

## Phase 11 — Platform Engineering ★ *Differentiator*

**Focus:** building an Internal Developer Platform — treating engineers as customers. Now the mainstream operating model, not an experiment.

- **IDP** concepts; **golden paths / paved roads** ("build highways instead of letting every driver pave their own road").
- **Backstage** (developer portal), service catalogs, software templates.
- Self-service provisioning; **Crossplane** for platform APIs over cloud resources.
- **Developer Experience (DX)** as a first-class metric; reducing cognitive load — the point of the whole discipline.
- **★ New:** platforms are going "AI-native" — golden paths increasingly ship with agent hooks and policy-as-code baked in. Your security defaults become the paved road *everyone* is nudged onto.

**Milestone:** a Backstage portal with a "new service" template that scaffolds repo + pipeline + manifests + observability — with your security defaults (signing, scanning, NHI, network policy) baked into the golden path so the *secure* way is the *easy* way.

---

## Phase 12 — Site Reliability Engineering (SRE)

**Focus:** reliability as an engineering discipline.

- SLOs / error budgets as decision tools; toil identification and reduction.
- Incident response: on-call, severity levels, comms, blameless postmortems.
- **Chaos engineering** (fault injection, game days).
- Capacity planning, load testing.

**Milestone:** run a game day — inject a failure, respond via runbook, write a blameless postmortem. (This runbook becomes training data / grounding for the Phase 15 incident agent.)

---

## Phase 13 — FinOps ★ *Differentiator*

**Focus:** cost as shared engineering responsibility — now sharper because **AI/LLM spend scales nonlinearly** and surprises people.

- Cloud cost visibility, tagging/allocation, showback/chargeback.
- **Infracost** — surface a change's cost *in the PR, before merge*.
- **Kubecost** for K8s spend; rightsizing, spot/savings plans.
- Cost-aware architecture reviews; **track inference/token cost** for any AI you put in the pipeline.

**Milestone:** add Infracost to your PR pipeline so every infra change comments its monthly cost delta.

---

## Phase 14 — AI-Assisted Ops / AIOps ★ *Differentiator*

**Focus:** AI embedded in daily operations. *This phase is the "assisted" tier — human decides, AI accelerates. Phase 15 is the "autonomous" tier.*

- **AI-assisted operations:** LLM-driven log analysis, incident timeline/summarization, pipeline-failure diagnosis, runbook drafting, deployment-risk review.
- **Predictive / anomaly detection:** analyze metrics/traces/history to warn *before* outages.
- **AI copilots for IaC:** generate/review Terraform, K8s manifests, Helm charts — with human validation, never blind-apply.
- **LLM integration patterns:** MCP (structured context/tools), **RAG over your own configs/runbooks/logs** so the model uses *your* environment, not just training data.
- **MLOps / LLMOps basics:** model/endpoint deployment, versioning, monitoring, drift, guardrails.

**Milestone:** build an AI incident-assistant that pulls recent logs/metrics (RAG over your Phase 8 telemetry) and drafts an incident summary + probable root cause — **human approves before any action.** This is "shadow mode": the agent proposes, you dispose.

---

## Phase 15 — Agentic Ops & AI Governance ★★ *New — the frontier*

**Focus:** the leap from AI-assisted to AI-*autonomous*. Agents with delegated authority that observe → analyze → plan → act → reflect, inside boundaries you design. **This is the phase where a security architect has an unfair advantage over a generalist DevOps engineer — the whole discipline is trust boundaries and blast radius.**

**Agentic architecture:**
- The three-layer pattern: **planner** (decides next action toward a goal), **tools** (concrete capabilities — run a query, open a PR, restart a service), **memory** (what's been tried and what worked).
- **ReAct loops:** Observe (read the failure) → Analyze (root-cause the stack trace) → Plan → Act (execute via MCP within boundaries) → Reflect (verify; retry with a modified approach if it failed).
- **Multi-agent coordination** (LangGraph / CrewAI / AutoGen): specialized agents daisy-chained — triage agent, coding agent, security agent, infra agent — instead of one generalist model owning everything.
- **Pipeline-as-specification & collaborative remediation:** the pipeline validates against acceptance criteria and feeds failures back to the authoring agent for automatic fix-and-re-verify; humans intervene only past an attempt budget.

**Governance & security (the part most teams get wrong — your wheelhouse):**
- **Non-Human Identity (NHI):** every agent gets its own identity, least-privilege scopes, and an audit trail of its tool calls and MCP queries. The remediation agent can't read customer data; the observability agent can't modify infra.
- **Human-in-the-loop gates:** any action with blast radius beyond a single service needs human approval; agents surface recommended actions with confidence scores and impact analysis.
- **Circuit breakers:** automatic fail-safes so an agent can't cascade a misjudgment into an outage; repeated failed remediations escalate to humans.
- **Treat agent inputs as hostile:** prompt injection through alert text, log entries, or external data can steer agents into unintended actions (OWASP Top 10 for LLM apps). Sanitize and constrain, exactly like user input.
- **Agent attestation:** cryptographically bind each agent-authored commit to the agent, the model version, the spec it was given, and the human who authorized the task.

**Adoption ladder (how disciplined teams actually roll this out):**
1. **Shadow mode** — agent proposes fixes in a Slack/Jira message; humans execute and score accuracy.
2. **Bounded autonomy** — agent acts on low-risk, well-defined, easily-reversible tasks (dependency bumps, log rotation, drift remediation).
3. **Expanded autonomy** — widen scope only as measured trust grows; never remove the audit log or the circuit breaker.

**Milestone:** take your Phase 14 shadow-mode assistant and give it *bounded* authority over exactly one low-risk, reversible task (e.g. auto-open a PR for a flagged dependency update). Give it a dedicated NHI with least privilege, log every step, put a human-approval gate on merge, and add a circuit breaker that escalates after 2 failed attempts. Then red-team it: try to prompt-inject it through a poisoned log line.

---

## Capstone Projects (portfolio)

1. **End-to-end secure delivery platform:** app → GitHub Actions (test/scan/sign, SHA-pinned actions) → GitOps deploy via ArgoCD → K8s with service mesh → OTel observability → SLO alerting → Cosign-enforced admission. *Demonstrates Phases 2–9.*
2. **Self-service IDP:** Backstage + golden-path template that provisions a compliant, observable, cost-tagged service in one click, security defaults baked in. *Phases 11 + 13.*
3. **Agentic incident responder (the flagship):** RAG-based incident/log analysis integrated into your on-call flow, promoted from shadow mode to *bounded autonomy* on one reversible action — with NHI, human-approval gate, full audit trail, and a documented red-team of its prompt-injection surface. *Phases 8, 14, 15.* This is the single project that proves you can design autonomous systems *safely* — the exact thing 2026 hiring is short on.

---

## Certifications (optional; signal value)

- **Kubernetes:** CKA, **CKS (security — highest value for you)**, CKAD.
- **Cloud:** AWS Solutions Architect / DevOps Engineer Pro, or Azure/GCP equivalents.
- **IaC:** HashiCorp Terraform Associate.
- **GitOps:** Argo CD / Flux certifications.
- **Security:** supply-chain / cloud-security creds complement CKS; watch for emerging **AI-security / MCP-hardening** credentials — they're new and scarce, which is exactly when a cert is worth most.

---

## Learning Principles (unchanged, still the whole game)

- **Learn by doing.** You'll learn more debugging a broken deploy than reading slides.
- **Master one tool per category**, understand the pattern beneath it.
- **Fundamentals over hype** — Kubernetes will change; Linux and networking won't.
- **Security and cost are delivery stages**, not afterthoughts.
- **Use AI as a force multiplier, validated by your judgment** — and now, *design the boundaries that keep other people's AI honest too.*

---

## Suggested Sequencing for an Experienced Security Architect

```
Phase 0–1 (skim) → 2 → 3 → 4 → 5 → 7 (GitOps)
→ 9 (DevSecOps + AI Supply Chain — go deepest)
→ 8 (Observability) → 11 (Platform Eng)
→ 14 (AI-Assisted Ops) → 15 (Agentic Ops & Governance)
→ 6, 12, 13 as depth-ups
```

The bolded path front-loads the senior differentiators while keeping the delivery backbone intact. Phases 9 and 15 are where your background compounds: everyone can wire an agent to a pipeline; very few can say *precisely* what it's allowed to touch and prove it.

---

# Part II — Delivery & Pedagogy Layer

*Everything above is the "what." This is the "how to make people actually finish it." These ideas are drawn from your second document and are what separate a free syllabus from something competitive with paid bootcamps for job-placement outcomes.*

## The teaching structure that makes concepts stick

The best technical educators use a consistent three-beat rhythm. Adopt it for every module:

**Analogy → Concept → Hands-on → "Now in tech terms."**

Open each lesson with a plain-language analogy, teach the real thing, do the lab, then map the analogy back to the exact commands. A ready-made analogy library (from your doc): Git = Google Docs version history; Linux = your house (root = master key, `/var/log` = CCTV); Networking = a city's postal system (IP = address, DNS = contacts app, port = apartment number); Docker = a self-contained lunchbox; CI/CD = a car-factory assembly line; Kubernetes = an orchestra conductor; DevSecOps = airport security (SAST = packing carefully at home, DAST = the X-ray, severity gate = "no knives on the plane"); SRE = a hospital with service-level promises.

**New analogies:**
- **AI agent / agentic ops** = *a capable new contractor.* You wouldn't hand a day-one contractor admin on every system with no review and no expiry — yet that's what an ungoverned agent looks like. NHI is their badge; least privilege is their key-card zones; the audit log is the building's sign-in sheet; the circuit breaker is security escorting them out when they keep setting off alarms.
- **MCP** = *USB-C for AI* — one standard port so any agent can safely plug into your tools instead of a custom cable for every connection.
- **Prompt injection through logs** = *a con artist slipping a forged note into the inbox* an agent is told to trust — which is why you treat agent inputs like user input.
- **Model signing / attestation** = *a tamper-evident seal on a medicine bottle*: unsigned or broken seal → don't consume.

## High-ROI, low-effort improvements (from your doc, prioritized)

**Do first (quick wins, <1 hr each):**
- `og:image` / OpenGraph meta tags so shared links show a preview (big click-through lift when learners post progress).
- Mobile-responsiveness fixes for the day-progress checkboxes and module accordion (many learners study on commutes).
- README day-number audit — some module READMEs point at wrong day folders, breaking internal links.

**Do next (the content gaps that actually cost job offers):**
- **Complete the Terraform module.** Nearly every DevOps JD lists it; without it learners can't apply to a large share of roles. Even a 5-day intro (providers, resources, state, modules, workspaces) on AWS free tier closes the gap.
- **Observability week** — Prometheus + Grafana via Docker Compose, log aggregation (Loki/ELK), alerting rules + runbooks. This is what separates "knows Docker" from "DevOps engineer."
- **End-to-end capstone** — one complete system (app → Docker → GitHub Actions → Helm → K8s → Prometheus → Terraform). The capstone repo becomes the portfolio piece and the #1 interview talking point.

**Retention mechanics (cheap, high impact on completion):**
- **"I'm stuck" escape hatches:** a *Simplified version* of hard labs, a *Skip and come back* checkbox, and a progressive *Hint* button. A learner who stalls on one day shouldn't abandon the whole course.
- **"Today's Challenge" banner** based on start date (localStorage) — accountability with no account system.
- **Module dependency map** — a simple SVG/CSS graph ("M03 Linux before M08 CI/CD") so experienced learners skip ahead confidently instead of grinding linearly out of fear.
- **Peer solution gallery** — a GitHub Discussions thread per day (no backend needed) to normalize "good enough" solutions and build community. Community accountability is the strongest predictor of completion.
- **Dark/light mode toggle** and **print-friendly cheatsheets** (`cheatsheet.html?module=M10`) as tangible keepsakes.

**Distribution / motivation:**
- Companion **2–3 min concept videos** per day (read + watch retains far better than text alone).
- A **completion badge** learners can add to LinkedIn — a trust signal and free marketing loop.
- **"Week in Review" reflection prompt** (localStorage, never sent anywhere): "What was hardest? What would you explain differently to a friend?" — writing forces understanding and doubles as interview prep.

## Suggested rollout order (from your doc)

- **Week 1:** OpenGraph tags + mobile fixes + README audit.
- **Week 2:** Terraform module (highest job demand).
- **Week 3:** capstone design + day structure.
- **Month 2:** observability module + companion videos.
- **Month 3:** community layer (Discussions, badge).

The **Terraform + capstone** combination is the pair that makes the course genuinely competitive with paid alternatives on placement outcomes. The **agentic-ops capstone** is the pair that makes it competitive on *2026* placement outcomes specifically.
