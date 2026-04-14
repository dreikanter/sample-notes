# ARIA attributes — practical reference

Documenting the attributes I actually use and how they behave. ARIA has a lot of attributes that seem useful but aren't — these are the ones that reliably work.

**`aria-label`**

Provides an accessible name when visible text is absent or insufficient.

```html
<button aria-label="Close dialog">✕</button>
<nav aria-label="Breadcrumb">...</nav>
```

**`aria-labelledby`**

Points to the ID of an element that provides the name. Preferred over `aria-label` when a visible label exists.

```html
<section aria-labelledby="section-title">
  <h2 id="section-title">Monthly report</h2>
```

**`aria-describedby`**

Supplementary description, read after the name. Good for hints and error messages.

```html
<input aria-describedby="email-hint" type="email">
<p id="email-hint">Use your work address.</p>
```

**`aria-expanded`**

State for accordions, dropdowns, disclosure widgets.

```html
<button aria-expanded="false" aria-controls="menu">Menu</button>
<ul id="menu" hidden>...</ul>
```

Toggle the value when the controlled element shows/hides.

**`aria-live`**

For dynamic content updates announced to screen readers.

```html
<div aria-live="polite" aria-atomic="true">
  <!-- Status messages injected here -->
</div>
```

`polite`: waits for user to idle. `assertive`: interrupts. Use assertive only for errors.

**Common mistakes**

- `role="button"` on a `<div>` without also adding `tabindex="0"` and keyboard handlers
- Using ARIA to fix HTML semantics problems rather than fixing the HTML
- Hiding content from all users when you mean to hide only from visual users

[MDN ARIA overview](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA)
