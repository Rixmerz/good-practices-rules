---
paths:
  - "**/routes/**/*.{ts,tsx}"
  - "**/islands/**/*.{ts,tsx}"
  - "**/components/**/*.{ts,tsx}"
  - "**/main.ts"
  - "**/dev.ts"
  - "**/fresh.config.ts"
  - "**/vite.config.ts"
---
<!-- Installed by good-practices-rules. Edit the package, not this copy: the next install replaces it. -->
# Good practices: Deno Fresh (1.x and 2.x)

## Top rules
- **Export a route middleware as `handler`, never `handlers`**: a misnamed export is silently ignored (fresh-handler-export).
- **Put event handlers (`onClick`, `onInput`, ...) only in islands**: in `routes/` and `components/` they are rendered on the server and never run in the browser (fresh-server-event-handlers).
- **Call hooks only at the top level of a component**, never inside loops, conditions or nested functions (react-rules-of-hooks).
- **Don't use `dangerouslySetInnerHTML`**; render content as JSX children, and never combine it with children (react-no-danger, react-no-danger-with-children).
- Give every `<button>` an explicit `type` (jsx-button-has-type).
- Pass children as JSX children, not a `children` prop (jsx-no-children-prop); keep void elements like `<img>` and `<br>` childless (jsx-void-dom-elements-no-children).
- Write comments in JSX as `{/* ... */}`, escape `>` and `}` in text, and drop fragments that wrap a single child (jsx-no-comment-text-nodes, jsx-no-unescaped-entities, jsx-no-useless-fragment).

## More rules to follow
Write code that already complies with each one; the ones marked **critical** break builds or production.

Source: `deno lint` rules tagged `fresh` (https://docs.deno.com/lint/; enable with `"lint": { "rules": { "tags": ["fresh", "recommended"] } }`).

- fresh-handler-export: Checks correct naming for named fresh middleware export. **critical**
- fresh-server-event-handlers: Disallows event handlers in fresh server components. **critical**
- react-rules-of-hooks: Ensure that hooks are called correctly in React/Preact components. **critical**
- react-no-danger: Prevent the use of dangerouslySetInnerHTML which can lead to XSS vulnerabilities if used incorrectly.
- react-no-danger-with-children: Using JSX children together with dangerouslySetInnerHTML is invalid as they will be ignored.
- jsx-button-has-type: Enforce <button> elements to have a type attribute.
- jsx-no-children-prop: Pass children as JSX children instead of as an attribute.
- jsx-no-comment-text-nodes: JavaScript comments inside text nodes are rendered as plain text in JSX.
- jsx-no-unescaped-entities: Leaving the > or } character in JSX is often undesired and difficult to spot.
- jsx-no-useless-fragment: Fragments are only necessary at the top of a JSX "block" and only when there are multiple children.
- jsx-void-dom-elements-no-children: Ensure that void elements in HTML don't have any children as that is not valid HTML.
