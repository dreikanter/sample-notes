# Search redesign — engineering notes

Started the engineering work on the search redesign. Architecture notes.

**Current state**

The search component is a single large component (~600 lines) with mixed concerns: UI state, fetch logic, URL sync, and result rendering all in one file. This makes it hard to test and hard to change.

**New architecture**

Splitting into:

1. `useSearch` hook — manages search state, debounce, and fetch. Returns `{ query, results, isLoading, error, setQuery }`.
2. `SearchInput` — controlled input, calls `setQuery`. No business logic.
3. `SearchResults` — pure display component. Receives results array, renders.
4. `SearchFilters` — manages filter state (might become its own hook).
5. `SearchPage` — assembles the above, handles URL sync with `useEffect` + `URLSearchParams`.

**URL sync**

Using `history.pushState` rather than the router's navigate function — this avoids unnecessary re-renders on filter changes. The search page reads initial state from `URLSearchParams` on mount.

**Debounce**

300 ms on the input. Using a `useEffect` with a `setTimeout` cleanup:

```typescript
useEffect(() => {
  const id = setTimeout(() => performSearch(query), 300);
  return () => clearTimeout(id);
}, [query]);
```

**Keyboard navigation**

The spec ([[20230522_1164]]) requires: Tab navigates between filter groups, Arrow keys navigate within a filter group, Enter selects, Escape closes dropdowns.

Using `aria-activedescendant` pattern for the results list rather than managing focus directly — more robust with screen readers.

**Next steps**

Get the hook working and tested. Then wire up the display components. Then the filter state. Then URL sync last (easiest to add but depends on everything else).

[React hooks patterns](https://react.dev/learn/reusing-logic-with-custom-hooks)
