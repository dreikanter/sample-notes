---
title: Web accessibility basics — practical notes
slug: accessibility-web-basics
tags: [accessibility, html, web, frontend]
description: Practical accessibility patterns for web development.
public: true
---

# Web accessibility basics — practical notes

Notes on making web content accessible. Full reference: [MDN Accessibility guide](https://developer.mozilla.org/en-US/docs/Web/Accessibility) and [WCAG 2.1 guidelines](https://www.w3.org/WAI/standards-guidelines/wcag/).

## Semantic HTML first

Most accessibility wins come from using the right element. A `<button>` is already keyboard-focusable, has the right ARIA role, and triggers on Enter/Space. A `<div onClick>` requires manual work to replicate all of that.

```html
<!-- Wrong -->
<div onclick="submit()" class="button">Submit</div>

<!-- Right -->
<button type="submit">Submit</button>
```

## Images

```html
<!-- Informative image: describe what it shows -->
<img src="graph.png" alt="Bar chart showing revenue growth from £2m in Q1 to £3.5m in Q4">

<!-- Decorative image: empty alt to skip it -->
<img src="divider.png" alt="">
```

## Forms

```html
<!-- Always associate labels explicitly -->
<label for="email">Email address</label>
<input id="email" type="email" autocomplete="email" aria-required="true">

<!-- Error states -->
<input aria-invalid="true" aria-describedby="email-error">
<span id="email-error" role="alert">Please enter a valid email address</span>
```

## Focus management

Keyboard users navigate with Tab. Ensure:
- Focus is visible — never `outline: none` without a replacement
- Modal dialogs trap focus while open and return focus when closed
- Dynamic content changes are announced to screen readers

## ARIA landmarks

Use landmark roles to give screen reader users navigation structure:
```html
<header role="banner">...</header>
<nav role="navigation">...</nav>
<main role="main">...</main>
<aside role="complementary">...</aside>
<footer role="contentinfo">...</footer>
```

Modern semantic HTML elements imply these roles, so `<main>` already has `role="main"`.

## Testing

Test with keyboard navigation only — Tab, Shift+Tab, Enter, Space, arrow keys. Run [axe DevTools browser extension](https://www.deque.com/axe/devtools/) to catch automated issues. Test with a screen reader — VoiceOver on Mac, NVDA on Windows.

Automated tools catch about 30–40% of accessibility issues; manual testing is irreplaceable.
