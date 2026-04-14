---
title: CSS grid cheatsheet
slug: css-grid-cheatsheet
tags: [css, frontend, reference]
---

# CSS grid cheatsheet

Quick reference for CSS Grid properties I use often enough to need but not often enough to memorize.

## Container properties

```css
.container {
  display: grid;
  
  /* Define columns */
  grid-template-columns: 1fr 2fr 1fr;
  grid-template-columns: repeat(3, 1fr);
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  
  /* Define rows */
  grid-template-rows: auto 1fr auto;
  
  /* Gaps */
  gap: 1rem;
  column-gap: 1.5rem;
  row-gap: 0.5rem;
  
  /* Alignment of all cells */
  justify-items: start | end | center | stretch;
  align-items: start | end | center | stretch;
  
  /* Alignment of whole grid in container */
  justify-content: start | end | center | stretch | space-between | space-around | space-evenly;
  align-content: start | end | center | stretch | space-between | space-around | space-evenly;
}
```

## Item properties

```css
.item {
  /* Span across columns */
  grid-column: 1 / 3;          /* column 1 to 3 */
  grid-column: span 2;          /* span 2 columns */
  grid-column: 1 / -1;          /* full width */
  
  /* Span across rows */
  grid-row: 2 / 4;
  grid-row: span 2;
  
  /* Self-alignment */
  justify-self: start | end | center | stretch;
  align-self: start | end | center | stretch;
}
```

## Named areas

```css
.container {
  grid-template-areas:
    "header header"
    "sidebar main"
    "footer footer";
}
.header { grid-area: header; }
.sidebar { grid-area: sidebar; }
.main { grid-area: main; }
.footer { grid-area: footer; }
```

Interactive playground: [https://cssgridgarden.com](https://cssgridgarden.com)
