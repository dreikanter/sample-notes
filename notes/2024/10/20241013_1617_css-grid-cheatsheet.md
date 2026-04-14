# CSS grid cheatsheet

Quick reference for CSS Grid properties I keep forgetting. Pulled from the [MDN Grid documentation](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_grid_layout) and personal usage.

## Container properties

```css
display: grid;
grid-template-columns: repeat(3, 1fr);
grid-template-rows: auto 200px auto;
grid-template-areas:
  "header header header"
  "sidebar main main"
  "footer footer footer";
gap: 16px 24px; /* row-gap column-gap */
align-items: start | center | end | stretch;
justify-items: start | center | end | stretch;
place-items: center; /* shorthand */
```

## Item properties

```css
grid-column: 1 / 3;         /* span from line 1 to line 3 */
grid-column: span 2;        /* span 2 columns */
grid-row: 2 / 4;
grid-area: header;          /* named area */
align-self: end;
justify-self: center;
```

## Useful patterns

**Full-bleed layout**: Set the grid on body with named lines, then let full-bleed items use `1 / -1`.

**Auto-fill vs auto-fit**: `auto-fill` keeps empty tracks; `auto-fit` collapses them. For responsive grids without media queries:

```css
grid-template-columns: repeat(auto-fit, minmax(240px, 1fr));
```

**Subgrid** (now widely supported): lets a child element participate in the parent grid's tracks. Useful for card layouts where inner elements need to align across cards.

```css
.card {
  display: grid;
  grid-row: span 3;
  grid-template-rows: subgrid;
}
```

Subgrid browser support is solid as of late 2024 — Safari 16+, Chrome 117+, Firefox 71+.
