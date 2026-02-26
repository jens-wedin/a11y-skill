---
name: a11y-dev
version: "1.0"
description: >
  Front-end accessibility implementation guide for developers. Provides semantic HTML
  patterns, ARIA attributes, keyboard navigation, focus management, and ESLint setup.
  PROACTIVELY activate when writing or reviewing front-end components, forms, modals,
  interactive widgets, or when asked to "make this accessible".
  Triggers: "write accessible", "make this accessible", "add ARIA", "fix accessibility",
  "a11y-dev", "accessibility-dev", "WCAG", "screen reader", "keyboard navigation"
group: accessibility
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

```tsx
<div role="tablist" aria-label="Settings sections">
  <button
    role="tab"
    aria-selected={activeTab === 'general'}
    aria-controls="panel-general"
    id="tab-general"
    tabIndex={activeTab === 'general' ? 0 : -1}
    onClick={() => setActiveTab('general')}
  >
    General
  </button>
  {/* more tabs... */}
</div>

<div
  role="tabpanel"
  id="panel-general"
  aria-labelledby="tab-general"
  hidden={activeTab !== 'general'}
>
  {/* panel content */}
</div>
```

> Implement arrow key navigation within `role="tablist"` (roving tabindex pattern).

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
trigger element.

```tsx
function Modal({ isOpen, onClose, triggerRef, children }) {
  const modalRef = useRef<HTMLDivElement>(null);

  useEffect(() => {
    if (!isOpen) return;

    // Move focus into the modal
    const focusable = modalRef.current?.querySelectorAll<HTMLElement>(
      'button, [href], input, select, textarea, [tabindex]:not([tabindex="-1"])'
    );
    focusable?.[0]?.focus();

    const handleKeyDown = (e: KeyboardEvent) => {
      if (e.key === 'Escape') {
        onClose();
        triggerRef.current?.focus(); // return focus to trigger
      }
      // Trap Tab/Shift+Tab within modal
      if (e.key === 'Tab' && focusable) {
        const first = focusable[0];
        const last = focusable[focusable.length - 1];
        if (e.shiftKey && document.activeElement === first) {
          e.preventDefault();
          last.focus();
        } else if (!e.shiftKey && document.activeElement === last) {
          e.preventDefault();
          first.focus();
        }
      }
    };

    document.addEventListener('keydown', handleKeyDown);
    return () => document.removeEventListener('keydown', handleKeyDown);
  }, [isOpen, onClose, triggerRef]);

  if (!isOpen) return null;

  return (
    <div ref={modalRef} role="dialog" aria-modal="true" aria-labelledby="dialog-title">
      {children}
    </div>
  );
}
```

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

**Install:**
```bash
pnpm add -D eslint-plugin-jsx-a11y   # or npm/yarn
```

**Standalone config (ESLint 9 flat config):**
```javascript
// eslint.a11y.mjs
import jsxA11y from "eslint-plugin-jsx-a11y";
import tseslint from "typescript-eslint";

export default [
  {
    files: ["src/**/*.tsx", "src/**/*.jsx"],
    plugins: { "jsx-a11y": jsxA11y },
    languageOptions: {
      parser: tseslint.parser,
      parserOptions: { ecmaFeatures: { jsx: true } },
    },
    rules: { ...jsxA11y.flatConfigs.recommended.rules },
  },
];
```

**Run:**
```bash
npx eslint --config eslint.a11y.mjs src/
```

**Known false positives to suppress:**
- Custom component props named `role` (not the HTML `role` attribute)
- Components that forward ARIA props to a child element

---

## 12. Pre-Commit Checklist

Before merging a PR that touches interactive UI:

- [ ] All images have `alt` text (`alt=""` for decorative)
- [ ] All form inputs have an associated `<label>`
- [ ] Icon-only buttons have `aria-label`
- [ ] Focus is never removed without a visible replacement style
- [ ] New interactive elements are reachable and operable by keyboard
- [ ] No heading levels are skipped
- [ ] Color is not the only way to convey information
- [ ] Dynamic content changes are announced via `aria-live` or `role="alert"`
- [ ] Modals trap focus and restore it on close
- [ ] Tab from top to bottom of the page — focus order makes sense
- [ ] Quick screen reader check (VoiceOver on macOS: Cmd+F5, NVDA on Windows: free)


## Standards Reference

- [WCAG 2.1 Guidelines](https://www.w3.org/TR/WCAG21/)
- [axe-core Rules](https://github.com/dequelabs/axe-core/blob/develop/doc/rule-descriptions.md)
- [jsx-a11y Rules](https://github.com/jsx-eslint/eslint-plugin-jsx-a11y#supported-rules)