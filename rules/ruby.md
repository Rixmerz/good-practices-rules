---
paths:
  - "**/*.rb"
  - "**/*.rake"
  - "**/Gemfile"
---
<!-- Installed by good-practices-rules. Edit the package, not this copy: the next install replaces it. -->
# Ruby good practices: Rails, errors and security

## Top rules
- **Never interpolate user input into SQL**: `where("name = ?", name)` or `where(name: name)`, never `where("name = '#{name}'")`.
- **Use strong parameters** (`params.require(:user).permit(:name, :email)`); never pass raw `params` to `create`/`update`.
- **Rescue specific errors**, never bare `rescue` or `rescue Exception`, which also catches `Interrupt` and `SystemExit`.
- Add `# frozen_string_literal: true` at the top of every file.
- Commit `Gemfile.lock`.

## Style
- `snake_case` methods and variables, `CamelCase` classes and modules, `SCREAMING_SNAKE_CASE` constants.
- Predicate methods end in `?`, dangerous (mutating) ones in `!`.
- Use guard clauses instead of nested conditionals; use `unless` only without `else`.
- Use `&.` instead of `x && x.foo`.
- Don't use `and`/`or` for control flow; use `&&`/`||`.
- Don't monkey-patch core classes; use refinements if you must.

## Rails
- Avoid N+1 queries: `includes`/`preload` associations you iterate over.
- Keep controllers thin; put logic in models or service objects.
- Validate in the model and enforce with a database constraint (`null: false`, unique index).
- Use `find_each` for large batches, not `all.each`.
- Don't call `html_safe` or `raw` on user content.
- Keep `protect_from_forgery` enabled and `config.force_ssl = true` in production.
- Scope record lookups to the current user (`current_user.orders.find(id)`) to avoid IDOR.
- Read secrets from `Rails.application.credentials` or `ENV`.

## Security
- Never call `eval`, `send`, `public_send`, `constantize` or `instance_variable_get` with user input.
- Never pass user input to `system`, backticks, `%x`, `exec` or `open`; use the array form `system("cmd", arg)`.
- Use `YAML.safe_load`, never `YAML.load` or `Marshal.load` on untrusted data.
- Use `SecureRandom` for tokens, and `bcrypt`/`has_secure_password` for passwords.
- Don't redirect to a URL taken from params without an allowlist.

## More rules to follow
Write code that already complies with each one; the ones marked **critical** break builds or production.

### Vulnerabilities
- Database queries should not be vulnerable to injection attacks **(critical)**
- OS commands should not be vulnerable to command injection attacks **(critical)**
- Deserialization should not be vulnerable to injection attacks **(critical)**
- HTTP request redirections should not be open to forging attacks **(critical)**
- Dynamic code execution should not be vulnerable to injection attacks **(critical)**
- Mass assignment should be restricted
- Server certificates should be verified during SSL/TLS connections
- Cipher algorithms should be robust

### Code smells
- Methods should not be too complex
- Methods should not have too many parameters
- Unused local variables should be removed
