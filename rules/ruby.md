---
paths:
  - "**/*.rb"
  - "**/*.rake"
  - "**/Gemfile"
---
<!-- Installed by good-practices-rules. Edit the package, not this copy: the next install replaces it. -->
# Ruby good practices: correctness and security

## Top rules
- **Never hard-code credentials** (passwords, tokens, keys) in source; read them from `ENV` or an encrypted credentials store.
- **Don't write octal literals** (`0755`-style integers are easy to misread); write the value in decimal.
- **Don't leave dead or unreachable code**: remove `if true`/`if false` blocks and statements after `return`/`raise`.
- Don't use the same condition twice in an `if/elsif` chain or `case`; the second branch never runs.
- Don't self-assign variables (`x = x`) or use identical operands on both sides of an operator (`a == a`).
- Write `+=`/`-=`, never `=+`/`=-`.

## Structure
- Keep cognitive complexity and nesting low; extract methods instead of deep `if`/`case` nesting, and don't nest `case` statements.
- End `if ... elsif` chains with `else`, and give `case` statements an `else`.
- Don't give conditional branches identical implementations; merge them.
- Don't leave methods or nested blocks empty; add a comment saying why if it is intentional.
- Remove unused local variables and method parameters.
- Extract string literals repeated several times into constants.
- `snake_case` methods and parameters, `CamelCase` classes and modules.
- Don't hard-code IP addresses; read them from configuration.


## More rules to follow
Write code that already complies with each one; the ones marked **critical** break builds or production.

### Bugs
- Useless "if true ..." and "if false ..." blocks should be removed
- Variables should not be self-assigned
- All code should be reachable
- Identical expressions should not be used on both sides of a binary operator
- Related "if/elsif" statements and "when" in a "case" should not have the same condition
- Non-existent operators like "=+" should not be used
- All branches in a conditional structure should not have exactly the same implementation

### Vulnerabilities
- Credentials should not be hard-coded

### Code smells
- Mergeable "if" statements should be combined
- Functions should not have too many parameters
- Nested blocks of code should not be left empty
- Redundant pairs of parentheses should be removed
- Track uses of "FIXME" tags
- Unused function parameters should be removed
- Methods should not be empty **(critical)**
- String literals should not be duplicated **(critical)**
- "case" statements should have "else" clauses **(critical)**
- "case" statements should not have too many "when" clauses
- Two branches in a conditional structure should not have exactly the same implementation
- Cognitive Complexity of functions should not be too high **(critical)**
- Methods should not have identical implementations
