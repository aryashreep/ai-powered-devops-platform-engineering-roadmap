# AI Powered DevOps & Platform Engineering — Course Repo

A teachable curriculum. Structure:

```
curriculum/
  README.md                 <- master index + dependency map + capstones
  M##-module/
    README.md               <- outcomes, analogy anchor, teach-the-trap, unit list
    U##-unit/
      README.md              <- unit index
      L##-lesson.md          <- lesson (subtopics as sections; frontmatter: status/est_minutes)
  capstones/                <- the three graded gates
  LESSON_TEMPLATE.md
  CONTRIBUTING.md
```

- **16 modules · 65 units · 121 lessons · 346 subtopics.**
- Every lesson uses the same shape: `Analogy → Concept → Live demo → Lab → "Now in tech terms" → Concept check`.
- Lesson `status` frontmatter tracks progress: `draft → reviewed → recorded → published`.

## Regenerating / extending
The tree is generated from data, so edits are cheap and consistent:

```bash
python3 generate_curriculum.py curriculum   # rebuilds curriculum/ from the C data structure
```

Add or reorder content by editing the `C` list in `generate_curriculum.py`, then re-run.
Set `SUBTOPIC_AS_FILE = True` at the top of the generator to expand subtopics into their own
files (literal `unit/lesson/NN-subtopic.md`) instead of sections inside each lesson.

> ⚠️ Re-running deletes and rewrites `curriculum/`. Once you start writing real lesson content,
> either stop regenerating, or move your authored prose into the generator data so it's the source of truth.

## Reference docs
- `docs/DevOps-Syllabus-2026.md` — the syllabus (the "what")
- `docs/DevOps-Course-Design-2026.md` — full course design (the "how", with labs/assessments)