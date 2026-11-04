---
paths:
  - "**/*.{sh,bash,zsh,ksh}"
---
<!-- Installed by good-practices-rules. Edit the package, not this copy: the next install replaces it. -->
# Good practices: Shell

- **Start every bash script with `set -euo pipefail`** so errors, unset variables and failed pipes stop it.
- **Double-quote every expansion**: `"$var"`, `"$@"`, `"$(cmd)"`. Unquoted variables split on spaces and expand globs.
- Use `#!/usr/bin/env bash` when the script uses bash features; don't write bashisms under `#!/bin/sh`.
- Declare function variables `local`.
- Use `$(...)`, never backticks.
- Never `eval` input, and never pass it unquoted to `rm`, `find -exec` or `xargs`; use `--` before file arguments and `find -print0 | xargs -0`.
- Guard destructive paths: `rm -rf "${dir:?}/"` fails if `dir` is empty instead of deleting `/`.
- Create temp files with `mktemp` and remove them with `trap 'rm -f "$tmp"' EXIT`.
- Read lines with `while IFS= read -r line`; don't loop over `$(cat file)` or `ls` output.
- Send errors to stderr (`>&2`) and exit non-zero on failure.
- Don't `curl ... | bash` without verifying a checksum.
- Script must pass `shellcheck` with no warnings; disable a check only with `# shellcheck disable=SCxxxx` and a reason.
- Use `[[ ... ]]` instead of `[ ... ]` for bash conditionals.
- End every function with an explicit `return` or `exit`.
- Assign positional parameters to named locals: `local file="$1"`.
- Move a literal repeated many times into a variable. Merge nested `if`s.

## More rules to follow
Write code that already complies with each one; the ones marked **critical** break builds or production.

### Vulnerabilities
- File permissions should not be set to world-accessible values
- HTTPS should be enforced on HTTP clients following redirects
