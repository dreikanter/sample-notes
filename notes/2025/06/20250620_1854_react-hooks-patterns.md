---
title: React hooks — patterns I find useful
slug: react-hooks-patterns
tags: [react, javascript, programming]
description: Practical hook patterns beyond the basics
---

# React hooks — patterns I find useful

Beyond the useState/useEffect basics, these are the patterns I return to regularly.

**useCallback with stable references**

```javascript
const handleSubmit = useCallback(async (data) => {
  await submitForm(data);
}, []); // stable across renders — safe to pass as prop
```

Useful when passing callbacks to child components that use React.memo, or as dependencies to other hooks.

**Custom hook for async state**

```javascript
function useAsync(asyncFn, deps = []) {
  const [state, setState] = useState({
    status: 'idle', data: null, error: null
  });

  useEffect(() => {
    setState({ status: 'loading', data: null, error: null });
    asyncFn()
      .then(data => setState({ status: 'success', data, error: null }))
      .catch(error => setState({ status: 'error', data: null, error }));
  }, deps);

  return state;
}
```

Handles loading/error/success states without repeating the pattern in every component.

**useRef for previous values**

```javascript
function usePrevious(value) {
  const ref = useRef();
  useEffect(() => { ref.current = value; });
  return ref.current;
}
```

The update happens after render, so `ref.current` always holds the previous render's value.

**Avoiding stale closures with useRef**

```javascript
const callbackRef = useRef(callback);
useEffect(() => { callbackRef.current = callback; }, [callback]);

// In a setInterval or event listener:
callbackRef.current();
```

This pattern avoids adding callback to the interval's dependency array while keeping it up to date.

Patterns reference: https://usehooks.com/
