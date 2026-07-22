# M02 — Everything-as-Code Mindset

**Duration:** 2-3 days  
**Cert alignment:** None (conceptual foundation)

---

## 🎯 Learning Outcomes

By the end of this module you will be able to:

- Articulate why declarative + idempotent + immutable beats manual ops
- Convert a manual process into a reviewed Git repo with PR template and CODEOWNERS
- Recognize the "…as code" spectrum — from config as code to guardrails as code
- Understand why review culture and auditable history are non-negotiable in production

---

## 📚 Module Structure

| Unit | Lesson | Status |
|------|--------|--------|
| **U01 — Core principles** | [L01 — Declarative vs imperative](U01-core-principles/L01-declarative-vs-imperative.md) | ✅ Reviewed |
| | [L02 — Idempotency](U01-core-principles/L02-idempotency.md) | 🟡 Skeleton |
| | [L03 — Immutability](U01-core-principles/L03-immutability.md) | 🟡 Skeleton |
| **U02 — '…as code' everywhere** | [L01 — The spectrum](U02-as-code-everywhere/L01-the-spectrum.md) | 🟡 Skeleton |
| **U03 — Change discipline** | [L01 — PR-driven change](U03-change-discipline/L01-pr-driven-change.md) | 🟡 Skeleton |

### Status Key
| Badge | Meaning |
|-------|---------|
| ✅ Reviewed | Lesson fully written and reviewed |
| 🟡 Skeleton | Template exists, needs full content |
| 🔴 Missing | Not yet created |

---

## 💡 Analogy Anchor (Open Every Lesson With This)

> **If it isn't in Git, it doesn't exist** — like a recipe nobody wrote down; it dies with the cook.

## ⚠️ Teach-the-Trap

Learners treat 'as code' as just storing files in Git. Emphasise that **review + idempotency** are what make as-code valuable, not storage. Storing imperative commands in a file is still imperative.

---

## 📖 Lesson Format

Each lesson follows the **45-minute framework**:

| Section | Time | Purpose |
|---------|------|---------|
| 🏛️ Foundation Context | 3 min | Module anchor — the "recipe" analogy, why as-code matters |
| Story | 2 min | Hook — Golu & Jagu scenario illustrating the pain point |
| Real-world Problem | 3 min | What breaks without this concept |
| Visualization | 2 min | Diagrams & mental models (spectrum, flow charts) |
| Theory | 5 min | Core concepts, definitions, comparisons |
| Live Demo | 13 min | Concrete examples with commands and tooling |
| Common Mistakes | 5 min | Wrong way → right way with code blocks |
| Interview Questions | 5 min | 3–4 Q&As with model answers |
| Lab Assignment | 10 min | Hands-on task with acceptance criteria |

**Every lesson includes:** ✅ Concept Check (micro-assessment) · 🧭 Now in Tech Terms (analogy mapping) · 🔗 Forward References to later modules · 📝 Closing 6-card summary

---

## 🧪 Lab / Assessment

**Lab (milestone):** Take one manual runbook (e.g. "how we set up a new server") and move it entirely into a reviewed Git repo with a PR template and CODEOWNERS. The version-controlled runbook becomes the source of truth.

**Assessment (peer review):** Critique a "works but manual" process — identify what makes it imperative, fragile, and non-auditable. Propose the as-code version explaining why each piece needs declarative, idempotent, or immutable treatment.

---

## 🛠 Prerequisites

- Completion of [M01 — Foundations](../M01-foundations/)
- Basic familiarity with Git (commits, branches, pushing)
- A GitHub account (free tier)

---

## 🔗 Where This Leads

| Concept → | Used in |
|-----------|---------|
| Declarative thinking | M03 (CI/CD), M05 (Terraform), M06 (Kubernetes), M08 (GitOps) |
| Idempotency | M03 (pipelines), M05 (Ansible/Terraform), M08 (ArgoCD reconciliation) |
| Immutability | M04 (Container images), M06 (Kubernetes Deployments) |
| PR-driven change | M03 (CI gates), M08 (GitOps), M12 (Platform Engineering) |
| Guardrails as code | M10 (OPA/Kyverno), M16 (Agent boundaries) |

---

## 💬 Reference

| Resource | Link |
|----------|------|
| Lesson template | [LESSON_TEMPLATE.md](../LESSON_TEMPLATE.md) |
| Course design | [DevOps-Course-Design-2026.md](../../docs/DevOps-Course-Design-2026.md) |
| Syllabus | [DevOps-Syllabus-2026.md](../../docs/DevOps-Syllabus-2026.md) |