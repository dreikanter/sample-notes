# CSS custom properties (variables) — practical notes

CSS custom properties have replaced preprocessor variables in most of my work. See the [MDN custom properties guide](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_custom_properties) for the full reference.

## Basic syntax

```css
:root {
  --color-primary: #3b82f6;
  --color-text: #1a1a1a;
  --spacing-base: 8px;
  --border-radius: 4px;
  --font-size-base: 16px;
}

.button {
  background: var(--color-primary);
  padding: calc(var(--spacing-base) * 2) calc(var(--spacing-base) * 3);
  border-radius: var(--border-radius);
}
```

## Fallback values

```css
color: var(--color-accent, var(--color-primary, blue));
```

The second argument to `var()` is the fallback. Can nest `var()` calls.

## Scoping

Custom properties are scoped to the element they're declared on and its descendants:

```css
.dark-section {
  --color-text: #f0f0f0;
  --color-background: #1a1a1a;
}
```

This enables theming without class-swapping on every element — just change the values on a container.

## Dynamic theme switching (JS)

```javascript
document.documentElement.style.setProperty('--color-primary', '#ff6b6b');
```

Or swap a class that applies a different set of custom properties to `:root`.

## Useful pattern: component-scoped defaults

```css
.card {
  --card-padding: 16px;
  --card-border-radius: 8px;
  /* Defaults that consumers can override */
  padding: var(--card-padding);
  border-radius: var(--card-border-radius);
}

.card--compact {
  --card-padding: 8px;
}
```

## Limitations

- No `@media` query scoping directly — but you can reassign them inside a media query
- No string interpolation (you can't do `var(--prefix)-color`)
- Not supported in property names, only values — `var(--property-name): value` doesn't work
