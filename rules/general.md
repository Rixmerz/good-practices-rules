---
paths:
  - "**/*.{java,kt,ts,tsx,js,jsx,mjs,cjs,py,go,php}"
---
<!-- Installed by good-practices-rules. Edit the package, not this copy: the next install replaces it. -->
# Good practices: rules shared across languages

These rules apply in every language; write compliant code up front so the PR doesn't bounce.

## Complexity and size
- Keep each function's cognitive complexity at **15 or less**. Every `if`/`for`/`catch`/`&&` adds to it, and nesting adds more: extract functions and return early.
- No more than **7 parameters** per function or constructor; group them into an object/DTO.
- Don't nest ternaries; use `if` or a function.
- Merge nested `if`s that have no `else` into one condition with `&&`.
- Use `if` instead of `switch` when there are fewer than 3 cases.

## Duplication
- A string literal repeated **3 or more times** in a file becomes a constant.
- Don't leave two branches of an `if`/`switch` with the same body, or all branches identical.
- Don't write two functions with the same implementation; have one call the other.

## Dead code and leftovers
- Never leave commented-out code; delete it, git keeps it.
- Don't ship `TODO` comments; resolve it or track it in the issue tracker.
- Remove unused imports, locals, assignments and parameters.
- Don't self-assign a variable.
- No empty functions or blocks; if intentional, add a comment saying why.
- Remove redundant `return`/`continue` at the end of a block.
- Don't write loops that run at most once.
- Don't write `!(a == b)`; use `a != b`.
- A `catch` that only rethrows the same exception should be removed or add context.
- Don't use APIs marked deprecated.

## Naming
- Follow the language's naming convention for classes, functions, fields and locals: `PascalCase` classes; `camelCase` in Java/TS/JS/PHP; `snake_case` in Python.

## Regular expressions
- Avoid patterns with exponential backtracking, such as nested quantifiers `(a+)+` or `(.*)*`, on external input; they allow ReDoS.
- Keep regexes simple; split or comment one that grows.
- Use the concise form: `\d` not `[0-9]`, `x+` not `xx*`. Don't repeat a character inside a class, and don't write single-character classes like `[a]`.

## Cross-language security
- Don't log user-controlled data without stripping `\r` and `\n` first; otherwise attackers can forge log lines.
- Use `https://`, `wss://` or `sftp://` for external services; never `http://`, `ftp://` or `telnet`.
- Never hard-code credentials; see `secrets.md`.
- Don't hard-code IP addresses; they belong in configuration.
- Don't build outgoing request URLs from unvalidated user-controlled path segments.
- Restrict CORS to known origins; never use `*` together with credentials.
- Never use MD5 or SHA-1 for anything security-related; use SHA-256+, and bcrypt/argon2 for passwords.
- Never use `Math.random`, `random` or `java.util.Random` for tokens, IDs or passwords; use the language's cryptographic RNG.

## Tests
- Every test has at least one assertion, and not a trivially true one like `assert True`.
- Tests that differ only in data become one parameterized test.
- Disable a test with `@Disabled("reason")` / `@Ignore("reason")`, never by commenting it out.
