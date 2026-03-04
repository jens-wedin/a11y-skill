---
name: a11y-design
description: >
  Use when asked to "review my design for accessibility", "annotate my Figma",
  "check my mockup", "create a11y specs", "accessibility handoff", "inclusive design
  review", "design audit", "a11y-design", "accessibility before handoff", "contrast
  check", "focus order", "target size", or "designer accessibility checklist".
---

# Accessibility for Designers

You are helping a designer ensure their vision is inclusive, accessible, and well-documented for developers. Your goal is to move accessibility "left" in the lifecycle — catching issues before a single line of code is written.

---

## 1. Documentation & Specs (Annotations)

Convert designs into clear technical requirements for developers. Use when asked to "document a11y" or "create accessibility specs". The full annotation template is in `references/annotation-template.md`.

### Key Annotations to Generate:
- **Focus Order:** Map out the logical `tab` sequence for interactive elements as a numbered list.
- **Landmarks:** Identify `<header>`, `<nav>`, `<main>`, `<footer>`, and `<aside>` regions with any required `aria-label`.
- **Text Alternatives:** Provide specific `alt` text for images and `aria-label` for icon-only buttons.
- **Heading Hierarchy:** Assign `h1` through `h6` levels to text elements regardless of their visual size — never skip levels.
- **State Indicators:** Define how "focus", "hover", "disabled", "loading", and "error" states look and behave for every interactive component.
- **ARIA Roles:** Suggest specific roles (e.g., `tablist`, `combobox`, `dialog`) for custom widgets, including expected keyboard behaviour.

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

### 3a. With Figma MCP connected
Use MCP tools to:
- List layers and inspect properties (colors, text styles, spacing).
- Read existing accessibility comments on the file.
- Export design tokens (colors, radii, spacing) and check them against WCAG thresholds.
- Identify components that lack documented focus states or disabled states.

### 3b. Without Figma MCP (screenshot or link)
Ask the user to share screenshots or export the relevant screens. You can still produce:
- A complete annotation document describing focus order, landmarks, ARIA roles, and text alternatives.
- A contrast assessment based on the visible hex values or described colours.
- A list of missing states (focus, hover, error, disabled) that need to be designed.

### 3c. Design System Alignment
- Compare design tokens with implementation tokens — flag colours used in code that don't exist in the approved palette.
- Identify "one-off" colours that may fail contrast because they bypass the accessible system palette.
- Check that interactive component variants (default, focus, hover, disabled, error) are all defined.

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

## Examples

### Example 1: Annotating a Figma screen for developer handoff

User says: "annotate my design for accessibility"

Actions:
1. Ask the user to share the design (screenshot, Figma link, or Figma MCP access)
2. Map focus order as a numbered sequence across interactive elements
3. Identify landmark regions and suggest semantic HTML elements
4. Provide `alt` text for images and `aria-label` for icon-only buttons
5. Assign heading levels (`h1`–`h6`) independent of visual size
6. Define all state variants: focus, hover, disabled, error

Result: Annotation document or comment set ready to attach to the Figma file, covering all required a11y specs for developers.

### Example 2: Reviewing a mockup for contrast and target size

User says: "check my mockup for accessibility issues"

Actions:
1. Examine all text colours against backgrounds — flag any below 4.5:1 (normal) or 3:1 (large)
2. Check all interactive elements — flag any below 24×24 px
3. Identify any information conveyed by colour alone (e.g., red error text with no icon)
4. Check that focus states are visible and designed (not just browser default)

Result: List of design issues with specific fix recommendations before a line of code is written.

### Example 3: Pre-handoff checklist review

User says: "is my design ready for accessibility handoff?"

Actions:
1. Walk through the Design Checklist (Section 5) item by item against the design
2. Flag any missing items with specific guidance
3. Confirm all ARIA roles and states are documented for custom widgets

Result: Go/no-go assessment with actionable items for anything that blocks handoff.

---

## Troubleshooting

**No Figma MCP connected**
Work from screenshots or a shared design file URL instead. Ask the user to export the relevant screens and describe the component hierarchy. You can still produce a full annotation document from visual inspection.

**Contrast ratio is borderline**
Use the exact hex values from the design and check against the 4.5:1 threshold for normal text. If the ratio is between 3:1 and 4.5:1, the text qualifies only if it is 18pt+ (24px+) or 14pt bold (approximately 18.67px bold). When in doubt, recommend adjusting — contrast issues are the most common WCAG failure.

**Designer has not defined focus states**
Flag this as a blocker for handoff. Focus styles are required under WCAG 2.4.7 and 2.4.11. Suggest a visible 2px solid ring in the brand's primary colour with at least 3:1 contrast against the adjacent background.

**Custom widget with no standard ARIA role**
If a component has no direct ARIA equivalent, use the closest role and document the expected keyboard behaviour explicitly in the annotation. Developers need to know: what keys activate it, what keys move within it, what `aria-*` attributes to expose.

---

## Standards Reference

- [WCAG 2.1 Design Guidance](https://www.w3.org/WAI/design-develop/designing/)
- [WCAG 2.2 Understanding Docs](https://www.w3.org/WAI/WCAG22/Understanding/)
- [A11Y Project Checklist](https://www.a11yproject.com/checklist/)
- [ARIA Authoring Practices Guide](https://www.w3.org/WAI/ARIA/apg/)
- Annotation template → `references/annotation-template.md`
