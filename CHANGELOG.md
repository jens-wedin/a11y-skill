# Changelog

## 2.0.0 — 2026-03-14

### Refactored — Lean skills based on Anthropic skill-creator best practices

**SKILL.md sizes reduced ~50%:**
- a11y-dev: 444 → 241 lines (844 words)
- a11y-qa: 332 → 142 lines (740 words)
- a11y-design: 164 → 111 lines (656 words)

**New reference files (progressive disclosure):**
- `a11y-dev/references/anti-patterns.md` — anti-pattern table + contrast code examples
- `a11y-dev/references/sr-only.md` — screen reader only CSS pattern
- `a11y-qa/references/auto-fix.md` — violation-to-fix lookup table
- `a11y-qa/references/report-template.md` — audit report template
- `a11y-qa/references/eslint-config.md` — standalone ESLint a11y config

**New scripts:**
- `a11y-qa/scripts/axe-scan.js` — axe-core browser injection (runs as black box, never loaded into context)

**Description improvements:**
- All descriptions now include proactive triggers (Claude tends to undertrigger)
- a11y-dev activates when creating interactive components even without explicit a11y mention
- a11y-qa activates on release prep, pre-merge checks, "before we ship"
- a11y-design activates when sharing design files or discussing UI specs for handoff

**Writing style:**
- Removed all ALL CAPS directives (ALWAYS, NEVER, MUST)
- Imperative form throughout
- Explanations of WHY instead of heavy-handed rules
- WCAG links updated to 2.2

**Eval sets created:**
- `a11y-skill-workspace/eval-a11y-{dev,qa,design}.json` — 20 queries each (10 should-trigger, 10 should-not) for future description optimization via skill-creator

---

## 1.0.0 — 2026-03-13

### Initial improvements from PDF guide assessment

- Fixed frontmatter: descriptions start with "Use when..."
- Removed unsupported `metadata` blocks
- Added Examples and Troubleshooting sections to all three
- Extracted large code blocks to `references/` directories
- Updated axe-core from 4.8.4 to 4.10.2
- Aligned WCAG references from 2.1 to 2.2
- Created: aria-patterns.md, eslint-setup.md, pr-checklist.md, manual-checklist.md, wcag-quick-ref.md, annotation-template.md

---

## 0.1.0 — 2026-03-12

### Initial commit

- a11y-dev, a11y-qa, a11y-design skills created
