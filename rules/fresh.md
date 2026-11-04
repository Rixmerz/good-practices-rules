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

## Top rules (critical)
- **Interactivity only works in islands.** An `onClick`, `useState`, signal or effect in `routes/` or `components/` never runs in the browser. Move it to `islands/` (or a `(_islands)/` folder).
- **Island props must be serializable**: primitives, plain objects, arrays, `Date`, signals and JSX. Never pass functions, class instances, `Map`s with non-serializable values, or secrets: everything in island props is sent to the browser.
- **Keep islands small**: make the interactive leaf an island, not the whole page. Everything an island imports goes into the client bundle.
- **Don't mix Fresh 1.x and 2.x APIs**: `$fresh/server.ts` and `fresh.gen.ts` belong to 1.x; `@fresh/core`, `App`, `define.*` and `page()` belong to 2.x. Check which version the project uses before writing code.

## Routes and data
- Load data in the route `handler` and pass it to the page: 1.x `ctx.render(data)` with `PageProps<Data>`; 2.x return `page(data)` with `define.page<typeof handler>`.
- Don't `fetch` your own API from a server-rendered page; call the function directly.
- Return proper responses: `ctx.renderNotFound()` (1.x) or `throw new HttpError(404)` (2.x) for missing data, `Response.redirect`/303 after a successful POST.
- Handle forms with a POST handler and the Post/Redirect/Get pattern; validate every field on the server.
- In 1.x, regenerate and commit `fresh.gen.ts` whenever routes or islands change (the dev server does it); never edit it by hand.
- Static routes win over dynamic ones; don't create two files that resolve to the same pattern.

## Middleware and state
- Put auth checks in `_middleware.ts` (1.x) or `app.use()`/route middleware (2.x), set the user on a typed `ctx.state`, and check authorization again in each handler that changes data.
- Always `return ctx.next()` (or a response) from middleware.
- In 2.x, register `staticFiles()` before middleware that should not run for assets.
- Use the 2.x built-ins (`csrf()`, `cors()`, `csp()`) instead of hand-rolled headers.

## Islands and client code
- Guard browser-only APIs with `IS_BROWSER` or run them in `useEffect`/`useSignalEffect`.
- Prefer `@preact/signals` for island state shared between islands; create signals inside the component or pass them as props, not as module globals on the server.
- Don't import server-only modules (database clients, `Deno.env`, KV) into islands.

## Styling and assets
- Put static files in `static/` and reference them with `asset()` (1.x) or the Vite import (2.x) so they get cache-busted.
- With Tailwind, keep class names as complete literal strings; never build them by concatenation.
