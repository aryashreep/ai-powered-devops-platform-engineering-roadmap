# AI Powered DevOps & Platform Engineering Roadmap

## Project Overview
A comprehensive DevOps & Platform Engineering curriculum organized into **16 modules · 65 units · 121 lessons · 346 subtopics**. Every lesson follows the `Story → Problem → Theory → Live Demo → Common Mistakes → Interview Questions → Assignment` framework.

## Key Repo Info
- **Repo:** github.com/aryashreep/ai-powered-devops-platform-engineering-roadmap
- **Course tagline:** "AI Powered DevOps & Platform Engineering" (title case in HTML subtitles)
- **Goal:** A teachable, up-to-date curriculum covering everything from Linux foundations to Agentic Ops & AI Governance.

## Structure
```
curriculum/
  README.md            <- master index
  M##-module/
    README.md          <- module overview
    U##-unit/
      README.md        <- unit index
      L##-lesson.md    <- lesson content
  capstones/           <- 3 graded capstone projects
  LESSON_TEMPLATE.md
  CONTRIBUTING.md
docs/
  DevOps-Syllabus-2026.md           <- the syllabus
  DevOps-Course-Design-2026.md      <- full course design with labs/assessments
```

## Curriculum Modules (M01-M16)
1. **M01 — Foundations** (Linux internals, networking, Git, Bash/Python)
2. **M02 — Everything-as-Code Mindset**
3. **M03 — CI/CD Pipeline as Code**
4. **M04 — Containers** (Docker, Podman, OCI)
5. **M05 — Infrastructure as Code** (Terraform, Ansible, Pulumi)
6. **M06 — Kubernetes Core** (CKA-aligned)
7. **M07 — Kubernetes Advanced & Cloud-Native**
8. **M08 — GitOps** (ArgoCD / Flux) ⭐
9. **M09 — Observability** (OpenTelemetry, Prometheus, Grafana)
10. **M10 — DevSecOps & Supply Chain Security** ⭐⭐ (deepest track)
11. **M11 — Cloud Platform** (AWS/Azure/GCP deep-dive)
12. **M12 — Platform Engineering** ⭐
13. **M13 — Site Reliability Engineering**
14. **M14 — FinOps** ⭐
15. **M15 — AI-Assisted Ops / AIOps** ⭐
16. **M16 — Agentic Ops & AI Governance** ⭐⭐ (2026 frontier)

## HTML Output Structure (M01 Foundations)
Each day has an HTML lesson file inside `curriculum/M01-foundations/day-NN/`:
- `L0N-lesson-name.html` — the full interactive lesson page
- `lab-session/` — lab task file per day
- Each HTML page includes: subtitle branding, author header (GitHub link in footer), lesson content with tabs (lesson, lab, cheat sheet, quiz, visual)

## Content Guidelines
- All content is in **English** (no Hinglish)
- Uses an analogy-first teaching approach
- Each lesson ends with: Takeaway · Interview Question · Assignment · Challenge · Mistake · Pro Tip
- Pages use a chalkboard/classroom-style theme with dark background
