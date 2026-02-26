# Accessibility Skills for AI Agents

A collection of specialized skills designed for AI coding assistants (like Claude Code, GitHub Copilot, or Antigravity) to help developers and QA engineers build, test, and maintain accessible web applications.

## 🚀 Core Components

The project is divided into two main areas of expertise:

### 1. [a11y-dev](file:///Users/jens.wedin/Documents/Code/a11y-skill/a11y-dev/SKILL.md)
**Focus:** Implementation & Development
Provides semantic HTML patterns, ARIA guidance, keyboard navigation strategies, and focus management techniques. 
- **Triggers:** "make this accessible", "add ARIA", "fix accessibility", "keyboard navigation".
- **Best For:** Writing new components, refactoring legacy UI, and ensuring code-level compliance.

### 2. [a11y-qa](file:///Users/jens.wedin/Documents/Code/a11y-skill/a11y-qa/SKILL.md)
**Focus:** Testing & Auditing
Equips QA engineers with automated scanning tools (axe-core, ESLint) and manual testing checklists.
- **Triggers:** "run accessibility audit", "check WCAG compliance", "a11y-qa".
- **Best For:** Pre-release audits, regression testing, and creating structured compliance reports.

---

## 🛠 Installation & Setup

To use these skills with your AI assistant, point it to the respective `SKILL.md` files or the project root.

### Claude Code Setup
Reference the skills in your configuration or use them directly in a session:

```bash
# Add to your project context
/skill add ./a11y-dev/SKILL.md
/skill add ./a11y-qa/SKILL.md
```

---

## 📖 How to Use

Simply ask your AI assistant to perform an accessibility task. The skills are designed to be proactive and will guide you through the process.

**Example Prompts:**
- *"Check this component for WCAG AA compliance."*
- *"Help me make this modal keyboard accessible."*
- *"Run a runtime accessibility audit on the homepage."*
- *"What is the correct ARIA role for a tab component?"*

---

## 📜 Standards & References

These skills are built upon industry-standard accessibility guidelines:
- **WCAG 2.1 & 2.2 (Level AA)**
- **WAI-ARIA Authoring Practices**
- [axe-core Rules Reference](https://github.com/dequelabs/axe-core/blob/develop/doc/rule-descriptions.md)
- [eslint-plugin-jsx-a11y Rules](https://github.com/jsx-eslint/eslint-plugin-jsx-a11y#supported-rules)

---

## 📂 Project Structure

- `a11y-dev/`: Developer-focused patterns and implementation guides.
- `a11y-qa/`: QA-focused auditing tools and checklists.
- `References/`: Supplemental guides and original documentation.
