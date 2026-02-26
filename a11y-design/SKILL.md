---
name: a11y-design
version: "1.0"
description: >
  Accessibility design guide and toolkit for designers. Helps create a11y documentation,
  assess designs in code/Storybook, integrate with Figma, and provides educational 
  checklists to learn more about inclusive design.
  Triggers: "design accessibility", "a11y-design", "accessibility-design", "review design for a11y",
  "a11y-design-specs", "design-audit", "inclusive-design"
group: accessibility
---

# Accessibility for Designers

You are helping a designer ensure their vision is inclusive, accessible, and well-documented for developers. Your goal is to move accessibility "left" in the lifecycle — catching issues before a single line of code is written.

---

## 1. Documentation & Specs (Annotations)

Convert designs into clear technical requirements for developers. Use this when asked to "document a11y" or "create accessibility specs".

### Key Annotations to Generate:
- **Focus Order:** Map out the logical `tab` sequence for interactive elements.
- **Landmarks:** Identify `<header>`, `<nav>`, `<main>`, `<footer>`, and `<aside>` regions.
- **Text Alternatives:** Provide specific `alt` text for images and `aria-label` for icon-only buttons.
- **Heading Hierarchy:** Assign `h1` through `h6` levels to text elements regardless of their visual size.
- **State Indicators:** Define how "focus", "hover", "disabled", and "error" states look and behave.
- **ARIA Roles:** Suggest specific roles (e.g., `tablist`, `combobox`, `dialog`) for custom widgets.

---

## 2. Assessment (Code & Storybook Review)

Review the current implementation to see how well it matches the design intent.

### 2a. Visual & Cognitive Audit
- **Color Contrast:** Verify that text meets 4.5:1 (normal) or 3:1 (large) ratios.
- **Font Scaling:** Check if text remains readable and layouts don't break at 200% zoom.
- **Color Independence:** Ensure information isn't conveyed *only* through color (e.g., use icons + text for errors).
- **Target Size:** Verify interactive targets are at least 24×24px (2.2 AA) or 44×44px (recommended).

### 2b. Storybook / Live Code Check
- Use browser tools to inspect the rendered DOM.
- Verify that custom components (from the design system) are using semantic elements where possible.
- Check that "hidden" elements are actually hidden from screen readers (`aria-hidden="true"` or `display: none`).

---

## 3. Tool Integration (Figma & Beyond)

Fetch and synthesize feedback from design tools.

### 3a. Figma Input
- If working with a Figma MCP, use tools to:
    - List layers and inspect properties (colors, text styles).
    - Read comments related to accessibility.
    - Export design tokens (colors, spacing) to check against a11y standards.

### 3b. Design System Alignment
- Compare design tokens with implementation tokens.
- Flag any "one-off" colors that bypass the approved accessible palette.

---

## 4. Learn & Educate: Thinking About Accessibility

Use these questions to challenge your assumptions and learn more about inclusive design.

### General Questions to Ask Yourself:
- **"What if I couldn't use a mouse?"** (Keyboard navigation, focus indicators)
- **"What if I couldn't see the screen?"** (Screen reader semantics, logical order)
- **"What if I couldn't hear?"** (Captions, visual indicators for audio)
- **"What if I were in a bright sunlit environment or using an old monitor?"** (Contrast, brightness)
- **"What if I were easily distracted or had trouble processing complex layouts?"** (Simplicity, consistent patterns)

### Deep Dive Questions:
- [ ] Is this interaction pattern standard, or am I inventing a new one that might confuse AT users?
- [ ] Does this animation provide a "Reduced Motion" alternative?
- [ ] Are my error messages clear and helpful, or just "Invalid Input"?
- [ ] If I zoom this page to 400%, does it reflow into a single column without horizontal scrolling?

---

## 5. Design Checklist (Pre-Handoff)

Before handing off to developers, ensure:

- [ ] **Contrast:** All text and UI components (borders, icons) meet WCAG AA.
- [ ] **Hierarchy:** Headings are identified and follow a logical order.
- [ ] **Labels:** All icon-only actions have intended accessible names.
- [ ] **Targets:** No interactive element is smaller than 24×24px.
- [ ] **Focus:** Focus states are designed and clearly visible.
- [ ] **Motion:** Critical animations have a way to be paused or follow `prefers-reduced-motion`.

---

## Standards Reference

- [WCAG 2.1 Design Guide](https://www.w3.org/WAI/design-develop/designing/)
- [A11Y Project - Checklist](https://www.a11yproject.com/checklist/)
- [Figma Accessibility Guide](https://www.figma.com/education/accessibility-in-design/)
