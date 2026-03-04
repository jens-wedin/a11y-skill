---
name: a11y-dev
description: >
  Use when writing or reviewing front-end components, forms, modals, or interactive
  widgets, or when asked to "make this accessible", "add ARIA", "fix accessibility",
  "a11y-dev", "write accessible code", "WCAG", "screen reader support", "keyboard
  navigation", "focus management", "semantic HTML", "skip link", or "aria-label".
  Activate proactively when implementing any interactive UI element.
---

# Accessibility for Developers

You are helping a front-end developer write accessible code from the start. Follow the
principle: **native HTML first, ARIA second, custom solutions last**.

---

## 1. Semantic HTML First

Use the right element for the job — native elements bring keyboard support, focus
management, and ARIA semantics for free.

```tsx
// GOOD — native elements do the heavy lifting
<button onClick={handleClick}>Submit</button>
<nav aria-label="Main navigation">...</nav>
<main id="main-content">...</main>
<article>...</article>
<aside>...</aside>
<header>...</header>
<footer>...</footer>

// BAD — divs require manual ARIA + keyboard handling
<div onClick={handleClick}>Submit</div>
<div className="nav">...</div>
```

**Rule of thumb:** If a native element exists for the purpose, use it.

---

## 2. ARIA Patterns

Use ARIA only when native semantics are insufficient. Every ARIA attribute must have a
reason. Wrong ARIA is worse than no ARIA.

### Icon-Only Buttons

Every interactive control needs an accessible name. If there is no visible text, use
`aria-label`.

```tsx
<Button variant="ghost" size="icon" aria-label="Close dialog">
  <X size={16} aria-hidden="true" />
</Button>

<Button variant="ghost" size="icon" aria-label="Open menu">
  <Menu size={16} aria-hidden="true" />
</Button>
```

> Always add `aria-hidden="true"` to decorative icons so screen readers skip them.

### Form Fields

Associate every input with a label. Use `aria-describedby` for helper text and errors.

```tsx
<div>
  <Label htmlFor="email">Email</Label>
  <Input
    id="email"
    type="email"
    aria-describedby="email-hint email-error"
    aria-invalid={!!error}
    aria-required="true"
  />
  <p id="email-hint" className="text-sm text-muted-foreground">
    We'll never share your email.
  </p>
  {error && (
    <p id="email-error" className="text-sm text-destructive" role="alert">
      {error}
    </p>
  )}
</div>
```

### Loading States

Use `aria-live` so screen readers announce dynamic changes without requiring focus.

```tsx
// Container announces its content changes
<div aria-live="polite" aria-busy={isLoading}>
  {isLoading ? <Skeleton /> : <Content />}
</div>

// Button that changes state during submission
<Button disabled={isLoading} aria-disabled={isLoading}>
  {isLoading ? (
    <>
      <Loader className="animate-spin" aria-hidden="true" />
      <span className="sr-only">Loading, please wait</span>
    </>
  ) : (
    'Submit'
  )}
</Button>
```

### Expandable Content (Accordion / Disclosure)

```tsx
function Accordion({ title, children, isOpen, onToggle }) {
  const contentId = useId();

  return (
    <div>
      <button
        aria-expanded={isOpen}
        aria-controls={contentId}
        onClick={onToggle}
      >
        {title}
      </button>
      <div id={contentId} hidden={!isOpen}>
        {children}
      </div>
    </div>
  );
}
```

### Tabs

Full tablist/tab/tabpanel implementation with roving tabindex keyboard behaviour →
`references/aria-patterns.md`

---

## 3. Keyboard Navigation

All interactive elements must be reachable and operable by keyboard alone.

### Custom Interactive Elements

Only use this pattern when you cannot use a native `<button>`. Native buttons handle
`Enter`, `Space`, and focus automatically.

```tsx
<div
  role="button"
  tabIndex={0}
  onClick={handleClick}
  onKeyDown={(e) => {
    if (e.key === 'Enter' || e.key === ' ') {
      e.preventDefault();
      handleClick();
    }
  }}
>
  Custom interactive element
</div>
```

### Modal Focus Trap

When a modal opens, focus must stay inside it. When it closes, return focus to the
trigger element. Full implementation with Tab/Shift+Tab trapping and Escape handling →
`references/aria-patterns.md`

---

## 4. Focus Management

### Visible Focus Styles

Never remove focus outlines without a visible replacement. Use `:focus-visible` so
mouse users are not affected.

```tsx
// Tailwind — ring on keyboard focus only
<Button className="focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2">
  Accessible focus
</Button>

// Plain CSS
.interactive-element:focus-visible {
  outline: 2px solid #005fcc;
  outline-offset: 2px;
}
```

### Skip Link

Add a skip link as the very first element in the page `<body>` so keyboard users can
jump past repeated navigation.

```tsx
// First element in layout
<a
  href="#main-content"
  className="sr-only focus:not-sr-only focus:absolute focus:top-4 focus:left-4 focus:z-50 focus:px-4 focus:py-2 focus:bg-background focus:text-foreground focus:rounded"
>
  Skip to main content
</a>

// Main content area
<main id="main-content" tabIndex={-1}>
  ...
</main>
```

---

## 5. Screen Reader Only Content

Use a visually hidden class to provide context that is obvious visually but not in the DOM.

```tsx
// Tailwind sr-only
<span className="sr-only">Opens in new tab</span>

// Plain CSS equivalent
.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border: 0;
}
```

**Common uses:**
- Clarify link destinations: `<a href="/reports">Download <span className="sr-only">annual report PDF</span></a>`
- Announce icon button purpose supplement
- Provide table captions

---

## 6. Color and Contrast

| Text type | Minimum contrast ratio (WCAG AA) |
|-----------|----------------------------------|
| Normal text (< 18pt / < 14pt bold) | 4.5:1 |
| Large text (≥ 18pt or ≥ 14pt bold) | 3:1 |
| UI components and focus indicators | 3:1 |

```tsx
// Use semantic tokens with built-in contrast
<p className="text-foreground">Primary — high contrast</p>
<p className="text-muted-foreground">Secondary — still WCAG AA</p>
<p className="text-destructive">Error text</p>

// Never use color as the ONLY indicator of state
// BAD: red text alone
<span className="text-red-500">Required</span>

// GOOD: icon + color + text
<span className="text-destructive flex items-center gap-2">
  <AlertCircle size={16} aria-hidden="true" />
  Required field
</span>
```

**Tools:** axe DevTools browser extension, Colour Contrast Analyser, Figma A11y plugin.

---

## 7. Common ARIA Roles Reference

| Role | Use for |
|------|---------|
| `button` | Clickable non-`<button>` elements |
| `dialog` | Modal dialogs |
| `alert` | Important messages that interrupt (e.g., errors) |
| `status` | Polite status updates (e.g., "Saved") |
| `navigation` | Navigation landmark (prefer `<nav>`) |
| `tablist` / `tab` / `tabpanel` | Tab interface |
| `menu` / `menuitem` | Dropdown command menus |
| `combobox` | Autocomplete / searchable select |
| `progressbar` | Progress indicators |
| `tooltip` | Supplementary hover/focus labels |

---

## 8. Heading Hierarchy

Headings create document structure that screen reader users navigate with shortcut keys.
Never skip levels for visual effect — use CSS instead.

```tsx
// GOOD — logical hierarchy
<h1>Page Title</h1>
  <h2>Section</h2>
    <h3>Subsection</h3>

// BAD — skips h2 for styling
<h1>Page Title</h1>
  <h3>This looks smaller</h3>   // Screen readers announce a skipped level

// Fix: style h2 to look smaller instead
<h2 className="text-sm font-medium">Styled as small but correct level</h2>
```

---

## 9. Descriptive Links and Buttons

Every link and button must have a name that makes sense out of context.

```tsx
// BAD — meaningless in a link list
<a href="/report.pdf">Click here</a>
<button>Submit</button>  // submit what?

// GOOD
<a href="/report.pdf">Download 2024 Annual Report (PDF)</a>
<button>Submit order</button>

// GOOD — sr-only to add context
<a href="/report.pdf">
  Annual Report
  <span className="sr-only">(PDF, 2.4 MB)</span>
</a>
```

---

## 10. Anti-Patterns to Avoid

| Anti-pattern | Problem | Fix |
|---|---|---|
| `<div onClick>` | No keyboard access, no role | Use `<button>` |
| `<img>` without `alt` | Screen readers read the file name | Add `alt` text or `alt=""` for decorative |
| Icon without `aria-label` | Announced as unlabelled button | Add `aria-label` to the button |
| Redundant `aria-label` on text button | Confusing double announcement | Remove `aria-label`, rely on text |
| `outline: none` without replacement | Keyboard users lose position | Use `focus-visible` styles |
| `tabIndex={1}` or higher | Breaks natural focus order | Only use `0` and `-1` |
| `aria-hidden="true"` on focusable element | Element unreachable from keyboard | Remove from tab order first |
| Placeholder as only label | Disappears on input | Always pair with `<label>` |
| Missing `lang` on `<html>` | Screen readers use wrong voice | `<html lang="en">` |

---

## 11. ESLint Static Analysis Setup

Catch accessibility issues at development time with `eslint-plugin-jsx-a11y`.
Full setup instructions, CI integration, and false-positive guidance → `references/eslint-setup.md`.

**Quick start:**
```bash
pnpm add -D eslint-plugin-jsx-a11y
npx eslint --config eslint.a11y.mjs src/
```

---

## 12. Pre-Commit Checklist

Full checklist with screen reader spot-check instructions → `references/pr-checklist.md`.

Key items before merging any interactive UI change:
- All images have `alt`; icon-only buttons have `aria-label`
- All form inputs have an associated `<label>`
- Focus styles never removed without a visible replacement
- Keyboard navigation works end-to-end
- Dynamic changes announced via `aria-live` or `role="alert"`


---

## Examples

### Example 1: Making a new component accessible from scratch

User says: "write an accessible dropdown menu"

Actions:
1. Use `role="menu"` / `role="menuitem"` pattern with keyboard navigation
2. Implement arrow-key roving tabindex
3. Add `aria-expanded` on the trigger button
4. Trap focus when open, return focus on Escape/selection

Result: Fully keyboard-navigable, screen-reader-announced dropdown following the ARIA menu pattern.

### Example 2: Reviewing an existing form for accessibility issues

User says: "make this form accessible"

Actions:
1. Verify every `input` has an associated `<label htmlFor>`
2. Add `aria-describedby` for hint text and error messages
3. Set `aria-invalid` and `role="alert"` on error paragraphs
4. Add `aria-required="true"` on required fields
5. Check colour contrast of placeholder and label text

Result: Form where every field is labelled, errors are announced, and the state is fully communicated to assistive technology.

### Example 3: Adding a skip link and landmark structure to a page

User says: "add keyboard navigation improvements to this page"

Actions:
1. Insert skip link as first element in `<body>`
2. Wrap navigation in `<nav aria-label="Main navigation">`
3. Wrap primary content in `<main id="main-content" tabIndex={-1}>`
4. Audit heading hierarchy — fix any skipped levels

Result: Page with working skip navigation, correct landmark regions, and logical heading structure.

---

## Troubleshooting

**Screen reader announces unexpected content**
Check for elements with `aria-hidden="true"` that are still focusable. Remove from the tab order with `tabIndex={-1}` first, or restructure the DOM so the element is genuinely hidden.

**Focus trap not working in modal**
Ensure the focusable query selector covers all interactive element types including `[tabindex]:not([tabindex="-1"])`. Re-query the list after any dynamic content changes inside the modal.

**`aria-live` region not announcing**
The element must exist in the DOM before content is injected — do not mount it conditionally. Prefer `aria-live="polite"` unless the message is a critical error.

**Custom element fails keyboard test**
If you cannot use a native `<button>`, you need `role="button"`, `tabIndex={0}`, and both `onClick` and `onKeyDown` handlers for `Enter` and `Space`. But prefer the native element — it handles all this automatically.

**ESLint jsx-a11y false positives**
Component props named `role` that are not the HTML `role` attribute, and components that forward ARIA props to underlying elements, are known false positives. Suppress with `eslint-disable-next-line` and a comment explaining why.

---

## Standards Reference

- [WCAG 2.1 Guidelines](https://www.w3.org/TR/WCAG21/)
- [ARIA Authoring Practices Guide](https://www.w3.org/WAI/ARIA/apg/)
- [axe-core Rules](https://github.com/dequelabs/axe-core/blob/develop/doc/rule-descriptions.md)
- [jsx-a11y Rules](https://github.com/jsx-eslint/eslint-plugin-jsx-a11y#supported-rules)
- ESLint setup → `references/eslint-setup.md`
- Pre-commit checklist → `references/pr-checklist.md`