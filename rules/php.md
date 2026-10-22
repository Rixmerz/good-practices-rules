---
paths:
  - "**/*.php"
---
<!-- Installed by good-practices-rules. Edit the package, not this copy: the next install replaces it. -->
# Good practices: PHP

- **Always use braces for `if`/`else`/`for`/`foreach`/`while`**, even for single-line bodies.
- Always declare method visibility (`public`/`protected`/`private`). Use those modifiers on properties, never `var`.
- Indent with spaces, never tabs. No trailing whitespace. End every file with a newline. Omit the closing `?>` in PHP-only files.
- Don't use parentheses with `echo`, `print`, `require` or `include`.
- Prefer `namespace` + `use` with Composer autoload over `require`/`include`. If you must include a file, use `require_once`.
- Use `elseif`, not `else if`.
- Return the expression directly instead of storing it in a temporary variable.
- A function has at most 3 `return` statements. Split functions and classes that grow too long.
- Every `switch` has a `default`. Initialize variables before use.
- **Security**: escape user data with `htmlspecialchars()` before echoing it. Query with PDO prepared statements, never by concatenation.

## More rules to follow
Write code that already complies with each one; the ones marked **critical** break builds or production.

### Bugs
- "$this" should not be used in a static context **(critical)**
- Raised Exceptions must derive from Throwable **(critical)**
- Caught Exceptions must derive from Throwable **(critical)**
- Class of caught exception should be defined **(critical)**
- References used in "foreach" loops should be "unset"
- Assertion failure exceptions should not be ignored
- Regex patterns following a possessive quantifier should not always fail
- Regex boundaries should not be used in a way that can never be matched
- Back references in regular expressions should only refer to capturing groups that are matched before the reference
- Regex lookahead assertions should not be contradictory
- Regular expressions should have valid delimiters
- All "catch" blocks should be able to catch exceptions
- Useless "if(true) {...}" and "if(false){...}" blocks should be removed
- Short-circuit logic should be used to prevent null pointer dereferences in conditionals
- Values should not be uselessly incremented
- A "for" loop update clause should move the counter in the right direction
- Non-existent operators like "=+" should not be used
- Unary prefix operators should not be repeated
- Array or Countable object count comparisons should make sense
- Exceptions should not be created without being thrown
- Alternation in regular expressions should not contain empty alternatives
- Replacement strings should reference existing regular expression groups

### Vulnerabilities
- OS commands should not be vulnerable to command injection attacks **(critical)**
- LDAP queries should not be vulnerable to injection attacks **(critical)**
- XPath expressions should not be vulnerable to injection attacks **(critical)**
- A secure password should be used when connecting to a database **(critical)**
- XML parsers should not be vulnerable to XXE attacks **(critical)**
- Deserialization should not be vulnerable to injection attacks **(critical)**
- HTTP request redirections should not be open to forging attacks **(critical)**
- Dynamic code execution should not be vulnerable to injection attacks **(critical)**
- Include expressions should not be vulnerable to injection attacks **(critical)**
- Extracting archives should not lead to zip slip vulnerabilities **(critical)**
- Dynamic code execution should not use user-controlled data
- Password hashing functions should use an unpredictable salt
- Cryptographic keys should be robust
- LDAP connections should be authenticated
- Cipher algorithms should be robust
- A new session should be created during user authentication
- File permissions should not be set to world-accessible values
- Session IDs must be securely generated and not user-specified
- Authorizations should be based on strong decisions
- Reflection should not be vulnerable to injection attacks
- Applications should not create session cookies from untrusted input
- Disable direct file editing in WordPress admin interface
- Automatic updates should not be disabled
- External requests should be restricted in WordPress to prevent data leaks and SSRF
- Prevent unauthenticated WordPress database repair for enhanced security
- Prevent unfiltered HTML exposure in WordPress for enhanced security
- Untrusted user input should not be used in file operations

### Code smells
- Variable variables should not be used **(critical)**
- Test class names should end with "Test" **(critical)**
- References should not be passed to function calls
- Conditionals should start on new lines
- A conditionally executed single line should be denoted by indentation
- "default" clauses should be first or last
- Constants should not be used as conditions
