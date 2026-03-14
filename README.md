# Accessibility Skills for Claude Code

Three specialized skills that help developers, QA engineers, and designers build accessible web applications.

## Skills

### [a11y-dev](a11y-dev/SKILL.md)
**Implementation & Development** — Semantic HTML patterns, ARIA attributes, keyboard navigation, focus management, and ESLint setup.
- **Triggers:** "make this accessible", "add ARIA", "keyboard navigation", "focus management", "semantic HTML"
- **References:** [aria-patterns](a11y-dev/references/aria-patterns.md), [anti-patterns](a11y-dev/references/anti-patterns.md), [sr-only](a11y-dev/references/sr-only.md), [eslint-setup](a11y-dev/references/eslint-setup.md), [pr-checklist](a11y-dev/references/pr-checklist.md)

### [a11y-qa](a11y-qa/SKILL.md)
**Testing & Auditing** — Automated scanning (axe-core, ESLint jsx-a11y), severity classification, report generation, and manual testing checklists.
- **Triggers:** "run accessibility audit", "check WCAG compliance", "axe-core scan", "before we ship"
- **References:** [auto-fix](a11y-qa/references/auto-fix.md), [report-template](a11y-qa/references/report-template.md), [eslint-config](a11y-qa/references/eslint-config.md), [manual-checklist](a11y-qa/references/manual-checklist.md), [wcag-quick-ref](a11y-qa/references/wcag-quick-ref.md)
- **Scripts:** [axe-scan.js](a11y-qa/scripts/axe-scan.js)

### [a11y-design](a11y-design/SKILL.md)
**Design & Documentation** — Accessibility annotations, contrast/target-size assessment, Figma integration, and pre-handoff checklists.
- **Triggers:** "review my design for accessibility", "annotate my Figma", "accessibility handoff", "contrast check"
- **References:** [annotation-template](a11y-design/references/annotation-template.md)

---

## Installation

Copy the skill directories to `~/.claude/skills/`:

```bash
cp -r a11y-dev a11y-qa a11y-design ~/.claude/skills/
```

---

## Project Structure

```
a11y-dev/
├── SKILL.md
└── references/
    ├── aria-patterns.md
    ├── anti-patterns.md
    ├── sr-only.md
    ├── eslint-setup.md
    └── pr-checklist.md
a11y-qa/
├── SKILL.md
├── references/
│   ├── auto-fix.md
│   ├── report-template.md
│   ├── eslint-config.md
│   ├── manual-checklist.md
│   └── wcag-quick-ref.md
└── scripts/
    └── axe-scan.js
a11y-design/
├── SKILL.md
└── references/
    └── annotation-template.md
```

## Standards

Built on WCAG 2.2 Level AA, WAI-ARIA Authoring Practices, axe-core, and eslint-plugin-jsx-a11y.
