---
paths:
  - "**/*.{ts,tsx,js,mjs}"
---
<!-- Installed by good-practices-rules. Edit the package, not this copy: the next install replaces it. -->
# Good practices: Hono 4.x

Installed only in projects that depend on Hono; these rules apply to its TypeScript files.

## Top rules
- **Validate every input** (`json`, `form`, `query`, `param`, `header`) with a validator middleware (`zValidator`, `sValidator`/Standard Schema, or `validator()`), and read it with `c.req.valid("json")`, never `await c.req.json()` directly.
- **Chain routes on one expression** (`const app = new Hono().get(...).post(...)`) and export `type AppType = typeof app` when you use the RPC client; separate `app.get()` statements lose the types.
- **Register an `app.onError` handler** that returns a generic message and logs the error; throw `HTTPException(status, { message })` for expected failures instead of returning ad-hoc JSON.
- **Type the app**: `new Hono<{ Bindings: Env; Variables: { user: User } }>()`, so `c.env` and `c.get()` are typed.

## Structure
- Split features into sub-apps and mount them with `app.route("/users", users)`; don't build one giant file.
- Write handlers inline or with `factory.createHandlers()`; don't create Rails-style controller functions that lose path-param types.
- Use `basePath()` for a version prefix instead of repeating it in every route.
- Return with `c.json(data, status)`, `c.text`, `c.body`, `c.redirect`; set the status explicitly for anything but 200.
- Add `app.notFound()` with a consistent JSON error shape.

## Middleware
- Order matters: register `logger`/`requestId`, `secureHeaders`, `cors`, `csrf`, then auth, then routes.
- Always `await next()` in middleware, and don't call it twice.
- Use `createMiddleware<{ Variables: ... }>()` so values set with `c.set()` are typed.
- Restrict `cors()` `origin` to known origins; never `*` with `credentials: true`.
- Use `bodyLimit()` on routes that accept uploads or JSON bodies.
- Use the built-in `jwt()`/`bearerAuth()`/`basicAuth()` middleware with secrets from `c.env` or environment variables, and specify the JWT `alg`.

## Security
- Set cookies with `setCookie(c, name, value, { httpOnly: true, secure: true, sameSite: "Lax" })`, and use `setSignedCookie` for values you trust.
- Never interpolate input into SQL; use your driver's parameters or a query builder.
- Scope every query to the authenticated user to avoid IDOR.
- Don't return stack traces or validation internals in production errors.

## Tests
- Test routes with `app.request("/path", { method: "POST", body })` or `testClient(app)`, no real server needed.
- Pass mock bindings as the third argument of `app.request` for Workers/Deno env.
