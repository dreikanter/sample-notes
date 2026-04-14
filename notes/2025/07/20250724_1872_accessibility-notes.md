---
title: Web accessibility — practical notes from an audit
slug: accessibility-notes
tags: [accessibility, web, programming]
description: Notes from doing a real accessibility audit
public: true
---

# Web accessibility — practical notes from an audit

Spent two days doing an accessibility audit on an existing web app. These are the findings and what I learned.

**The problems that came up most**

1. Missing form labels. Many `<input>` elements had visually present labels implemented as `<div>` or `<span>` with no association to the input. Screen readers couldn't tell users what field they were focused on. Fix: use `<label for="input-id">` or `aria-label`.

2. Low color contrast. Lots of gray-on-gray text that passed visual design review but failed WCAG AA (4.5:1 ratio for normal text). The design team pushed back on this until we showed it side-by-side with a simulator. The fix was a small palette adjustment.

3. Keyboard inaccessible components. Custom dropdown menus built in JavaScript weren't reachable by keyboard. No `tabindex`, no keyboard event handlers. For anything interactive: test it with keyboard-only navigation before shipping.

4. Images without alt text. Many decorative images had no `alt` attribute at all — screen readers read the filename. Fix: `alt=""` for decorative images (explicitly empty), descriptive alt text for meaningful images.

5. Focus management. Modal dialogs opened without moving focus inside them. User's focus stayed behind the modal. Fix: move focus to the dialog's first focusable element on open; return it to the trigger on close.

**Tools that helped**

- axe DevTools (browser extension) — catches ~30-40% of issues automatically
- NVDA (Windows screen reader) — for real-world testing
- Keyboard-only navigation — the most revealing manual test

WCAG guidelines: https://www.w3.org/WAI/WCAG22/quickref/
