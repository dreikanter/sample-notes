---
title: CSS container queries — practical notes
slug: css-container-queries
tags: [css, frontend, web]
description: How container queries work and when to use them
public: true
---

# CSS container queries — practical notes

Container queries landed in all major browsers in late 2023 and I've finally been using them properly in a project. Notes on what's actually useful.

## The basic problem they solve

Media queries respond to the viewport. But components often need to respond to their container — a sidebar card needs different styling depending on whether it's in a two-column layout or a full-width layout. That's what container queries enable.

## Syntax

```css
/* Define a containment context */
.card-wrapper {
  container-type: inline-size;
  container-name: card;
}

/* Query that container */
@container card (min-width: 400px) {
  .card {
    display: grid;
    grid-template-columns: 1fr 2fr;
  }
}
```

`inline-size` containment is the one you want 95% of the time — it enables queries based on width.

## Container query units

```css
/* cqw = 1% of container width */
.card-title {
  font-size: clamp(1rem, 4cqw, 1.5rem);
}
```

These are new and surprisingly useful for fluid typography within components.

## What doesn't work yet

Style queries (querying computed style values like `color-scheme`) are experimental and only in Chrome. Don't rely on them.

## When to use vs media queries

Use container queries for reusable components. Use media queries for page-level layout changes. Using both together is common and fine.

Reference: [MDN container queries](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_containment/Container_queries)
