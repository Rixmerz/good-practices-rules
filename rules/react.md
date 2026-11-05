---
paths:
  - "**/*.jsx"
  - "**/*.tsx"
---
<!-- Installed by good-practices-rules. Edit the package, not this copy: the next install replaces it. -->
# Good practices: React (also Preact)

## Top rules (critical)
- **Follow the rules of hooks** (S6440, `react-hooks/rules-of-hooks`): call hooks only at the top level of a component or custom hook, never inside conditions, loops or callbacks.
- **List every reactive value in effect and memo dependency arrays**; never silence `react-hooks/exhaustive-deps` without a comment.
- **Give list items a stable, unique `key`** from the data; never the array index or `Math.random()`.
- **Never pass user content to `dangerouslySetInnerHTML`** without sanitizing it (DOMPurify), and never combine it with `children`.

## Components and state
- Don't define a component inside another component; it remounts on every render.
- Don't call a state setter unconditionally in the render body, and don't call it with its own state variable.
- Update state from the previous value with the updater form: `setCount(c => c + 1)`.
- Never mutate state (`this.state`) or props; create new arrays and objects.
- Don't render non-boolean conditions (`0 && <X/>` renders `0`); use `count > 0 && ...`.
- Name components in `PascalCase`.
- Don't create context values inline (`value={{ a, b }}`) without `useMemo`; every consumer re-renders.
- Don't use `findDOMNode`, `isMounted`, string refs or legacy lifecycle methods.

## Accessibility
- Use real `<button>` and `<a href>` elements, not clickable `div`s; if you must, add `role`, `tabIndex` and key handlers.
- Every `<img>` has `alt`, every form control has a `<label>`.
- Don't use `autoFocus`.

## Security
- Don't put `javascript:` URLs in `href` (`react/jsx-no-script-url`).
- Add `rel="noopener noreferrer"` to `target="_blank"` links.


## More rules to follow
Write code that already complies with each one; the ones marked **critical** break builds or production.

### Bugs
- Mouse events should have corresponding keyboard events
- Member names should not be duplicated within a class or object literal
- React "render" functions should return a value
- Comments inside JSX expressions should be enclosed in curly braces
- React components should not render non-boolean condition values
- React Hooks should be properly called
- React's useState hook should not be used directly in the render function or body of a component
- React state setter function should not be called with its matching state variable
- "setState" should use a callback when referencing the previous state
- "this" should not be used in functional components
- "children" and "dangerouslySetInnerHTML" should not be used together
- "<button>" elements should have a valid "type" attribute, explicitly set when associated with a "<form>"
- Lodash and Underscore.js debounced or throttled functions should not be recreated on every React render

### Code smells
- Image, area, button with image, object and svg elements should have an alternative text
- iFrames must have a title
- Links with identical accessible names should have identical targets
- Media elements should have captions
- HTML elements should have a valid language attribute
- HTML "<table>" should not be used for layout purposes
- "<object>" tags should provide an alternative content
- Unused methods of React components should be removed
- JSX list components should have a key property
- React components should not be nested
- JSX list components should not use array indexes as key
- React Context Provider values should have stable identities
- JSX list components keys should match up between renders
- In React "this.state" should not be mutated directly
- JSX elements should not use unknown properties and attributes
- React "children" should not be passed as prop
- Redundant JSX fragments should be removed
- The return value of "ReactDOM.render" should not be used
- The return value of "useState" should be destructured and named symmetrically
- React props should be read-only
- "shouldComponentUpdate" should not be defined when extending "React.PureComponent"
- Unused React typed props should be removed
- User-defined JSX components should use Pascal case
- Spacing between inline elements should be explicit
- All "defaultProps" should have non-required PropTypes
- React's "findDOMNode" should not be used
- React's "isMounted" should not be used
- String references should not be used
- React legacy lifecycle methods should not be used
- ARIA properties in DOM elements should have valid values
- DOM elements with ARIA roles should have the required properties
- DOM elements with ARIA role should only have supported properties
- Prefer tag over ARIA role
- DOM elements with ARIA roles should have a valid non-abstract role
- No redundant ARIA role
- DOM elements with the `aria-activedescendant` property should be accessible via the tab key
- No ARIA role or property for unsupported DOM elements
- Focusable elements should not have "aria-hidden" attribute
- Anchors should contain accessible content
- DOM elements should use the "autocomplete" attribute correctly
- "tabIndex" values should be 0 or -1
- Non-interactive DOM elements should not have interactive ARIA roles
- Interactive DOM elements should not have non-interactive ARIA roles
- Anchor tags should not be used as buttons
- Non-interactive DOM elements should not have the `tabindex` attribute
- DOM elements should not use the "accesskey" property
- Non-interactive elements shouldn't have event handlers
- Non-interactive DOM elements should not have an interactive handler
- Heading elements should have accessible content
- Images should have a non-redundant alternate description
- Elements with an interactive role should support focus
- Label elements should have a text label and an associated control
- Deprecated React APIs should not be used
- DOM elements should not use the "autofocus" attribute
- The "scope" attribute should only be used on "th" elements
