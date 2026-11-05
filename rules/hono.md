---
paths:
  - "**/*.{ts,tsx,js,mjs}"
---
<!-- Installed by good-practices-rules. Edit the package, not this copy: the next install replaces it. -->
# Good practices: Hono 4.x

Installed only in projects that depend on Hono; these rules apply to its TypeScript files.

## Top rules
- **Validate every request body before reading it**: put `zValidator("json", schema)` (or the project's json/form validator) in the chain of every POST, PUT and PATCH and read with `c.req.valid("json")`, never `c.req.json()`/`parseBody()` raw (V001); only call `c.req.valid(x)` for a target that has a validator (V003).
- **Register middleware before the routes and mounts it covers**: `use()` above the routes on the same instance (M001) and above `route()` mounts (M002), and `cors()` before any auth middleware (M007).
- **Always `await next()` (or `return next()`)** in middleware, on every path (M004, M005).
- **Never hard-code or leak secrets**: read `jwt` secrets, `bearerAuth` tokens and `basicAuth` passwords from the environment (S004), validate secret env keys once at startup (S003), and in `onError` log the error and answer a generic body (S005).
- **List the allowed origins in `cors()` when `credentials: true`**; never `*` or the default (S001).
- Register each method and path once (R001), static routes before params that also match them (R002), and finish a sub-app before `route()` mounts it (R003).
- Build an exported `AppType` from one chained expression (`new Hono().get(...).post(...)`) (P001) and pass that exported type to `hc<T>()` (P002).
- Add `csrf()` to cookie/session-authenticated mutating routes (S006) and `bodyLimit()` where `formData()`/`parseBody()` is read (S007).
- Give the served root app `app.onError` (A002) and `app.notFound` (A003).
- Use the runtime's own env API, or `env(c)` from `hono/adapter` (C001); depend on one Hono copy, the npm one (C002); replace deprecated Hono APIs (C003).

## More rules to follow
Write code that already complies with each one; the ones marked **critical** break builds or production.

Source: the hono-dev plugin validator (`hono_validate`, `bin/hono-mcp rules`), 36 rules; **critical** marks the ones with severity `error`.

- R001: the same method and full path registered twice in one root table; the first answers, the second never runs. **critical**
- R002: a param or wildcard route registered before a static route it also matches (`/users/:id` before `/users/me`).
- R003: a route (or middleware) added to a sub-app after `route()` mounted it: the parent copied the table at that moment. **critical**
- R004: a Hono instance with routes that no `route()` mounts and nothing serves.
- R005: a route path, `route()` prefix or `basePath()` argument that is not a string literal: it is invisible to traces and to the other route rules.
- R006: a path with no leading slash, a `//`, or a route that duplicates another up to the trailing slash.
- R007: one sub-app mounted at two prefixes: every route exists once per mount.
- M001: `use()` registered after routes on the same instance that its path covers: they are matched first and never run through it.
- M002: a parent `use()` registered after the `route()` mount whose routes it would cover.
- M003: a scoped `use('/x/*')` whose path matches no route of the table.
- M004: a middleware that declares `next` and neither calls nor forwards it.
- M005: `next()` called without `await` or `return`: code after it runs early and the response is lost.
- M006: the same middleware (same name, source and argument text) twice in one route's chain: it runs twice.
- M007: `cors()` after an auth middleware on the same route: the preflight (no credentials) is answered 401. **critical**
- V001: POST, PUT or PATCH whose handler reads `c.req.json()`, `parseBody()` or `formData()` with no json or form validator in the chain.
- V002: a body route with no validator whose handler cannot be read: it hands `c`, `c.req` or `c.req.raw` to other code, is built by a factory, or is not resolvable.
- V003: `c.req.valid('x')` with no validator for target `x` in the chain: it returns `undefined`. **critical**
- V004: a json or form validator on a GET or HEAD route: there is no body to validate.
- V005: route params read with `c.req.param()` and no `param` validator.
- S001: `cors()` with `credentials: true` and an origin that is `'*'`, absent (the default is `'*'`), or a function that returns its own argument. **critical**
- S002: a route with no auth where a path-scoped auth middleware covers its path but sits after it in the table, or more than half of the routes of its own sub-app (at least three) are guarded.
- S003: a secret-looking env key read with no validation.
- S004: a literal string at `secret` (`jwt`), `token` (`bearerAuth`) or `password` (`basicAuth`). **critical**
- S005: an `onError` that puts `err.message`, `err.stack` or the raw error in the response.
- S006: a cookie or session auth middleware guarding POST, PUT, PATCH or DELETE routes with no `csrf()` in their chain.
- S007: `formData()` or `parseBody()` read with no `bodyLimit()` in the chain.
- A001: a route handler that calls a DB client, or a module that opens one, outside `config.serviceDirs` (default `src/services`, `services`, `src/lib`).
- A002: a served root app that never calls `onError`.
- A003: a served root app that never calls `notFound`.
- A004: an import cycle through two or more route modules (value imports only): one runs before the other is defined.
- P001: an exported `type AppType = typeof app` where `app` has routes but is not built by one chain: the client type has none.
- P002: `hc<T>()` where `T` is not an exported app type.
- C001: a runtime API that does not match the runtime: `Deno.env` on Node, `c.env` on Node, `process.env` on Workers without Node compatibility. **critical**
- C002: two Hono copies, `npm:hono` and `jsr:@hono/hono`. **critical**
- C003: a deprecated Hono API (`app.mount`, `app.fire`, `c.req.routePath`, `c.req.matchedRoutes`, the `hono/<runtime>` adapters, ...).
- C004: a route module that no test imports and whose static path prefixes no test text contains.
