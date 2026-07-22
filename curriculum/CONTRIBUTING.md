# Contributing

## Naming
- Modules: `M##-kebab-title/`
- Units: `U##-kebab-title/`
- Lessons: `L##-kebab-title.md`

## Lesson status lifecycle
`draft -> reviewed -> recorded -> published` (set in frontmatter).

## Lesson Framework

Every lesson follows a 7-part lecture flow (total ~45 min):

| Section | Time | Purpose |
|---|---|---|
| **Story** | 2 min | Hook with a vivid analogy or real story — no jargon yet |
| **Real-world Problem** | 3 min | State exactly what breaks without this knowledge |
| **Theory** | 5 min | Core concepts — diagrams, commands, tables |
| **Live Demo** | 15 min | Step-by-step on screen, every command copy-paste ready |
| **Common Mistakes** | 5 min | 3-5 specific mistakes, wrong way then right way |
| **Interview Questions** | 5 min | 3-5 real questions with model answers |
| **Assignment** | 10 min | Independent task with acceptance criteria |

Every lesson **must also end** with these 6 closing sections (one-liners for quick reference):

- `## Today's Takeaway` — the single most important thing
- `## Today's Interview Question` — most likely interview question
- `## Today's Assignment` — hands-on task in one line
- `## Today's Challenge` — stretch goal for fast learners
- `## Today's Mistake` — #1 mistake to avoid
- `## Today's Pro Tip` — production insight a senior engineer would share

### Topic flow (Unit README)
`Problem → Story → Visualization → Theory → Hands-on → Best Practice → Interview → Assignment`

## Regenerate
`python3 generate_curriculum.py curriculum`

Edit the `C` data structure in the generator to add/reorder content, then re-run.
