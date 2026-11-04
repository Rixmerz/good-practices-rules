---
paths:
  - "**/*.jsx"
  - "**/*.tsx"
---
<!-- Installed by good-practices-rules. Edit the package, not this copy: the next install replaces it. -->
# Good practices: React (also Preact and Next.js)

## Top rules (critical)
- **Follow the rules of hooks**: call hooks only at the top level of a component or custom hook, never inside conditions, loops or callbacks.
- **List every reactive value in effect and memo dependency arrays**; never silence `react-hooks/exhaustive-deps` without a comment.
- **Give list items a stable, unique `key`** from the data; never the array index for lists that reorder, and never `Math.random()`.
- **Never pass user content to `dangerouslySetInnerHTML`** without sanitizing it (DOMPurify).

## Components and state
- Don't define a component inside another component; it remounts on every render.
- Don't copy props into state; derive values during render or with `useMemo`.
- Don't use an effect to compute derived state or to react to an event; do it in render or in the event handler.
- Clean up every effect that subscribes, sets a timer or opens a connection.
- Update state from the previous value with the updater form: `setCount(c => c + 1)`.
- Never mutate state or props; create new arrays and objects.
- Don't use `0 && <X/>` patterns with numbers; it renders `0`. Use `count > 0 && ...`.
- Name components in `PascalCase` and custom hooks `useSomething`.
- Don't create context values inline (`value={{ a, b }}`) without `useMemo`; every consumer re-renders.

## Accessibility
- Use real `<button>` and `<a href>` elements, not clickable `div`s; if you must, add `role`, `tabIndex` and key handlers.
- Every `<img>` has `alt`, every form control has a `<label>`.
- Don't use `autoFocus` except in dialogs.

## Security
- Don't put `javascript:` URLs from data in `href`; validate the scheme.
- Add `rel="noopener noreferrer"` to `target="_blank"` links.
- Next.js: never import server secrets in client components; only `NEXT_PUBLIC_*` variables reach the browser, so never put a secret there. Validate input in server actions and route handlers and check authorization in each one.

## More rules to follow
Write code that already complies with each one; the ones marked **critical** break builds or production.

### Bugs
- React hooks should be called in the same order on every render **(critical)**
- React "key" props should be unique and stable
- "setState" should not be called in render
- Return values from functional components should not be ignored
- React components should not render non-boolean condition values

### Code smells
- React props should be read-only
- Unused React state should be removed
- Unused React props should be removed
- React components should not be nested
