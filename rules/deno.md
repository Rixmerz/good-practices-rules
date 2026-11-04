---
paths:
  - "**/deno.json"
  - "**/deno.jsonc"
  - "**/*.{ts,tsx}"
---
<!-- Installed by good-practices-rules. Edit the package, not this copy: the next install replaces it. -->
# Good practices: Deno

Installed only in projects that have a `deno.json`; these rules apply to its TypeScript files.

## Top rules
- **Run with the narrowest permissions**: `--allow-net=api.example.com`, `--allow-read=./data`, `--allow-env=DATABASE_URL`. Never `-A`/`--allow-all` in tasks, Dockerfiles or deploy scripts.
- **Declare dependencies in the `imports` map of `deno.json`** with a version (`"@std/assert": "jsr:@std/assert@^1"`), and import by the bare name. Don't scatter `jsr:`/`npm:`/URL specifiers with versions through the code.
- **Commit `deno.lock`** and use `deno install --frozen` (or `--frozen` on run/cache) in CI.
- Prefer JSR packages and `@std/*` over `https://deno.land/x` and `https://deno.land/std` URL imports.

## Code
- Code must pass `deno fmt --check`, `deno lint` and `deno check`; don't add `// deno-lint-ignore` without a reason on the same line.
- Use web-standard APIs (`fetch`, `Request`, `Response`, `URL`, `crypto.subtle`, `crypto.randomUUID()`) before reaching for npm packages.
- Read configuration with `Deno.env.get("NAME")` once at startup and fail fast if a required value is missing.
- Use `Deno.serve`, not the removed `std/http/server` `serve`.
- Close resources: `using file = await Deno.open(...)`, or `try/finally` with `close()`; consume or cancel every response body you `fetch`.
- Use `Deno.Command` with an argument array for subprocesses, never a shell string built from input.
- Use `@std/path` `join`/`resolve` and reject `..` in user-supplied paths.
- Prefer `Temporal`-safe date handling or ISO strings in UTC; never rely on the server's local time zone.
- Define scripts as `tasks` in `deno.json`, not shell scripts with ad-hoc flags.

## Deno KV and Deno Deploy
- Use `kv.atomic().check(...)` for read-modify-write; never read then `set` assuming nothing changed.
- Keep KV keys as arrays with a stable prefix (`["users", id]`) and add secondary-index entries in the same atomic operation.
- Deno Deploy has no writable file system and no long-lived process: don't write files or keep state in module globals between requests.
- Keep secrets in Deploy environment variables, never in `deno.json` or the repo.

## Tests
- Use `Deno.test` with `t.step` for sub-cases and `@std/assert` (`assertEquals`, `assertRejects`).
- Tests run with the same narrow permissions as the app (`deno test --allow-read=./fixtures`).
- Leave the resource and op sanitizers on; fix the leak instead of setting `sanitizeOps: false`.
- Use `FakeTime` from `@std/testing/time` instead of real waits.
