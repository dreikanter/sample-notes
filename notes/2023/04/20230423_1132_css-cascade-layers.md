---
title: CSS cascade layers — how they work
slug: css-cascade-layers
tags: [css, web, frontend]
---

# CSS cascade layers — how they work

Cascade layers (`@layer`) are now in all major browsers (since ~2022). They solve a long-standing specificity management problem.

**The problem they solve**

Without layers, specificity conflicts between third-party CSS, component styles, and utilities are resolved by source order and specificity — leading to escalating specificity wars (`!important` abuse, deep selectors).

**How layers work**

```css
@layer base, components, utilities;

@layer base {
  a { color: blue; }
}

@layer components {
  .nav a { color: black; }
}

@layer utilities {
  .text-red { color: red; }
}
```

Styles in later-declared layers win over earlier ones, regardless of specificity. So `.text-red` from `utilities` wins over `.nav a` from `components`, even though the latter is more specific.

**Unlayered styles win**

Styles outside any `@layer` declaration have higher priority than any layered styles. Useful for emergency overrides.

**Importing with layers**

```css
@import url('reset.css') layer(reset);
```

**When to use**

Best suited for design system architectures with clear hierarchy: resets → base → components → utilities → overrides. For small projects it's probably overkill.

See [MDN on @layer](https://developer.mozilla.org/en-US/docs/Web/CSS/@layer) and the [Miriam Suzanne talk at CSS Day 2022](https://www.youtube.com/watch?v=NDNRGW-_1EE) for background.
