# CSS container queries

Container queries let you style elements based on their container's size rather than the viewport size. This is a significant change in how responsive component design works.

## Basic syntax

```css
/* Define a containment context */
.card-container {
  container-type: inline-size;
  container-name: card;
}

/* Query it */
@container card (min-width: 400px) {
  .card {
    display: grid;
    grid-template-columns: 1fr 2fr;
  }
}
```

`container-type: inline-size` creates a containment context for the inline axis (width in horizontal writing modes). `size` includes both axes.

## Why this matters

With media queries, components can't know their own context. A sidebar card and a main-content card at the same viewport width may have very different available space. Container queries let the card itself adapt.

## Shorthand

```css
.wrapper {
  container: sidebar / inline-size;
  /* shorthand for name / type */
}
```

## Query units

```css
@container (min-width: 400px) {
  .item {
    font-size: 1.2cqi;  /* 1.2% of container inline size */
  }
}
```

`cqi`, `cqb`, `cqw`, `cqh`, `cqmin`, `cqmax` — container query units analogous to viewport units.

## Browser support

Baseline 2023, meaning all major browsers support it. Safe to use with no fallback in most projects.

MDN reference: https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_containment/Container_queries
Good practical examples: https://ishadeed.com/article/css-container-query-guide/
