---
paths:
  - "**/*.{sh,bash,zsh,ksh}"
---
<!-- Installed by good-practices-rules. Edit the package, not this copy: the next install replaces it. -->
# Good practices: Shell

- Use `[[ ... ]]` instead of `[ ... ]` for bash conditionals.
- End every function with an explicit `return` or `exit`.
- Assign positional parameters to named locals: `local file="$1"`.
- Move a literal repeated many times into a variable. Merge nested `if`s.

## More rules to follow
Write code that already complies with each one; the ones marked **critical** break builds or production.

### Vulnerabilities
- File permissions should not be set to world-accessible values
- HTTPS should be enforced on HTTP clients following redirects
