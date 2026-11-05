---
paths:
  - "**/*.scala"
  - "**/*.sc"
  - "**/build.sbt"
---
<!-- Installed by good-practices-rules. Edit the package, not this copy: the next install replaces it. -->
# Scala good practices

- **Never hard-code credentials** (passwords, tokens, keys) in source; read them from configuration or a secret store.
- **Don't leave dead or unreachable code**: remove `if (true)`/`if (false)` blocks and code after `return`/`throw`.
- **Keep cognitive complexity and nesting low**; extract functions instead of deeply nested `if`/`match`/`try`, and don't nest `match` expressions.
- Don't repeat a condition in an `if/else if` chain or a `case` in a `match`; the later branch never runs.
- Don't self-assign variables or use identical operands on both sides of an operator.
- Don't give conditional branches identical implementations; merge them.
- End `if ... else if` chains with `else`.
- Don't leave methods or nested blocks empty, and don't leave commented-out code.
- Remove unused private methods, local variables and parameters.
- Don't write redundant boolean literals (`x == true`) or inverted checks (`!(a == b)`).
- Extract string literals repeated several times into constants; don't hard-code IP addresses.


## More rules to follow
Write code that already complies with each one; the ones marked **critical** break builds or production.

### Bugs
- Useless "if(true) {...}" and "if(false){...}" blocks should be removed
- Variables should not be self-assigned
- All code should be reachable
- Identical expressions should not be used on both sides of a binary operator
- Related "if"/"else if" statements and "case" in a "match" should not have the same condition
- All branches in a conditional structure should not have exactly the same implementation

### Vulnerabilities
- Credentials should not be hard-coded

### Code smells
- Mergeable "if" statements should be combined
- Functions should not have too many parameters
- Nested blocks of code should not be left empty
- Track uses of "FIXME" tags
- Unused "private" methods should be removed
- Unused function parameters should be removed
- Methods should not be empty **(critical)**
- String literals should not be duplicated **(critical)**
- Sections of code should not be commented out
- "match" expressions should not have too many "case" clauses
- Two branches in a conditional structure should not have exactly the same implementation
- Cognitive Complexity of functions should not be too high **(critical)**
- Methods should not have identical implementations
