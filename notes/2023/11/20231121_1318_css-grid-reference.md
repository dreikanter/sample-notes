---
title: CSS grid quick reference
slug: css-grid-reference
tags: [css, web, reference]
public: true
---

# CSS grid quick reference

A working cheatsheet I keep updating as I hit edge cases in real projects.

## Container properties

```css
display: grid;
grid-template-columns: repeat(3, 1fr);
grid-template-rows: auto 1fr auto;
gap: 1rem 2rem; /* row-gap column-gap */
grid-template-areas:
  "header header"
  "sidebar main"
  "footer footer";
```

`minmax(min, max)` is the workhorse. `minmax(0, 1fr)` prevents blowout when content is wider than the fraction allows. This trips me up consistently.

## Placement

```css
grid-column: 1 / 3;       /* span from line 1 to line 3 */
grid-column: span 2;       /* span 2 columns from current position */
grid-area: header;         /* reference named area */
```

## Auto-fit vs auto-fill

- `auto-fill`: creates as many columns as fit, keeps empty columns
- `auto-fit`: collapses empty columns to zero

```css
grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
```

This single line handles responsive card layouts without a single media query. The most useful pattern I've found in two years of using grid.

## Alignment

```css
justify-items: start | end | center | stretch;
align-items: start | end | center | stretch;
place-items: center; /* shorthand */
```

Full spec: [MDN CSS Grid Layout](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_grid_layout). The visual guides on [CSS-Tricks Complete Guide](https://css-tricks.com/snippets/css/complete-guide-grid/) are still the fastest mental model I can reach for.
