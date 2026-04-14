# React hooks patterns — practical reference

Things I look up more than I should. Capturing the patterns that took me the longest to internalize.

## useEffect dependency arrays

The most common source of bugs. Rules:

1. Everything used inside `useEffect` that could change should be in the dependency array.
2. Functions defined outside `useEffect` should be wrapped in `useCallback` if used inside.
3. Objects and arrays should be memoized with `useMemo` or `useCallback` — otherwise they're new references on every render.

```javascript
// Wrong: 'user' object is a new reference each render
useEffect(() => {
  fetchData(user.id);
}, [user]);  // infinite loop if user is recreated each render

// Right: depend on the primitive you actually need
useEffect(() => {
  fetchData(userId);
}, [userId]);
```

## Custom hook pattern for data fetching

```javascript
function useResource(url) {
  const [state, setState] = useState({ data: null, loading: true, error: null });

  useEffect(() => {
    let cancelled = false;
    fetch(url)
      .then(r => r.json())
      .then(data => { if (!cancelled) setState({ data, loading: false, error: null }); })
      .catch(error => { if (!cancelled) setState({ data: null, loading: false, error }); });

    return () => { cancelled = true; };  // cleanup prevents setState after unmount
  }, [url]);

  return state;
}
```

The `cancelled` flag is important and often omitted.

## useReducer for complex state

When multiple state variables change together or state transitions follow a pattern, `useReducer` is cleaner than multiple `useState`:

```javascript
function reducer(state, action) {
  switch (action.type) {
    case 'FETCH_START': return { ...state, loading: true };
    case 'FETCH_SUCCESS': return { data: action.payload, loading: false, error: null };
    case 'FETCH_ERROR': return { data: null, loading: false, error: action.error };
  }
}
```

[React hooks documentation](https://react.dev/reference/react/hooks)
