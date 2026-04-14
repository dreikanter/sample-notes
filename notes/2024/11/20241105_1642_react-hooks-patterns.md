---
title: React hooks patterns I keep coming back to
slug: react-hooks-patterns
tags: [react, javascript, programming]
public: true
---

# React hooks patterns I keep coming back to

Not a comprehensive reference — just the patterns that solve real problems I run into often. See the [React docs on hooks](https://react.dev/reference/react) for the official API.

## useCallback for stable function references

```jsx
const handleSubmit = useCallback(
  async (data) => {
    await saveUser(data);
    navigate('/dashboard');
  },
  [navigate] // Only recreate if navigate changes
);
```

Without `useCallback`, passing this function as a prop causes unnecessary re-renders in child components that use `React.memo`.

## Custom hook for async operations

```jsx
function useAsync(asyncFn, deps = []) {
  const [state, setState] = useState({ data: null, error: null, loading: false });

  useEffect(() => {
    let cancelled = false;
    setState(s => ({ ...s, loading: true }));
    asyncFn().then(
      data => { if (!cancelled) setState({ data, error: null, loading: false }); },
      error => { if (!cancelled) setState({ data: null, error, loading: false }); }
    );
    return () => { cancelled = true; };
  }, deps); // eslint-disable-line

  return state;
}
```

The `cancelled` flag prevents setting state on unmounted components.

## useReducer for complex local state

When you have more than 2–3 related state values that change together, `useReducer` keeps the logic in one place:

```jsx
const [state, dispatch] = useReducer(reducer, initialState);

// Clear, intent-driven updates:
dispatch({ type: 'SUBMIT_START' });
dispatch({ type: 'SUBMIT_SUCCESS', payload: response });
dispatch({ type: 'SUBMIT_ERROR', payload: error });
```

## useRef for values that don't trigger re-renders

```jsx
const intervalRef = useRef(null);
// Interval ID doesn't need to be in state — changes don't affect UI
intervalRef.current = setInterval(tick, 1000);
```

Also useful for accessing DOM nodes and storing previous values.
