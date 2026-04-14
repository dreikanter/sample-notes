# CSS grid cheatsheet

## Container properties

```css
.container {
  display: grid;

  /* Define columns */
  grid-template-columns: 200px 1fr 1fr;
  grid-template-columns: repeat(3, 1fr);
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));

  /* Define rows */
  grid-template-rows: auto 1fr auto;

  /* Gaps */
  gap: 16px;           /* row and column */
  gap: 16px 24px;      /* row column */

  /* Alignment */
  justify-items: start | end | center | stretch;
  align-items: start | end | center | stretch;
  justify-content: start | end | center | stretch | space-between | space-around;
}
```

## Named areas

```css
.container {
  grid-template-areas:
    "header header header"
    "nav    main   aside"
    "footer footer footer";
}

.header { grid-area: header; }
.nav    { grid-area: nav; }
.main   { grid-area: main; }
```

## Item placement

```css
.item {
  grid-column: 1 / 3;       /* span columns 1-2 */
  grid-column: 1 / -1;      /* full width */
  grid-column: span 2;      /* span 2 from current position */

  grid-row: 2 / 4;

  /* Shorthand */
  grid-area: 1 / 1 / 3 / 4; /* row-start / col-start / row-end / col-end */
}
```

## auto-fill vs auto-fit

`auto-fill` creates empty columns if items don't fill the space. `auto-fit` collapses empty columns, letting remaining items stretch to fill. For most responsive card layouts, `auto-fit` is what you want.

Reference: https://css-tricks.com/snippets/css/complete-guide-grid/
