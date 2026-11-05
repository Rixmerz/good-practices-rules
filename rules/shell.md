---
paths:
  - "**/*.{sh,bash,zsh,ksh}"
---
<!-- Installed by good-practices-rules. Edit the package, not this copy: the next install replaces it. -->
# Good practices: Shell

- **Double-quote every expansion**: `"$var"`, `"$@"`, `"$(cmd)"`. Unquoted variables split on spaces and expand globs (ShellCheck SC2086).
- Don't use bash-only features in a script whose shebang is `#!/bin/sh`; switch the shebang to bash instead (SC3xxx).
- Use `$(...)`, never backticks (SC2006).
- Pipe `find` into `xargs` only with `find -print0 | xargs -0`, and write globs as `./*` (or put `--` before them) so file names starting with `-` are not read as options (SC2038, SC2035).
- Guard destructive paths: `rm -rf "${dir:?}/"` fails if `dir` is empty instead of deleting `/` (SC2115).
- Read lines with `while IFS= read -r line`; don't loop over `$(cat file)` or `ls` output (SC2162, SC2013, SC2045).
- Use `[[ ... ]]` instead of `[ ... ]` for bash conditionals.
- End every function with an explicit `return` or `exit`.
- Assign positional parameters to named locals: `local file="$1"`.
- Move a literal repeated many times into a variable. Merge nested `if`s.

## More rules to follow
Write code that already complies with each one; the ones marked **critical** break builds or production.

### Vulnerabilities
- File permissions should not be set to world-accessible values
- HTTPS should be enforced on HTTP clients following redirects
