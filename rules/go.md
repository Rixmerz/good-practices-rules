---
paths:
  - "**/*.go"
  - "**/go.mod"
---
<!-- Installed by good-practices-rules. Edit the package, not this copy: the next install replaces it. -->
# Good practices: Go

- **Name functions in MixedCaps with no underscores**: `CreateBooking`/`createBooking`, not `create_booking`. Tests use the `TestXxx` form.
- Don't shadow predeclared identifiers such as `max`, `min`, `len`, `new`, `error` or `string`.
- Single-method interfaces end in `-er`: `Reader`, `Notifier`.
- If the function receives a `ctx`, use it; don't create `context.Background()`/`context.TODO()` inside. Never store a context in a struct field.
- Put `defer cancel()` right after `context.WithTimeout`/`context.WithCancel`.
- If an `if` short-statement variable is used only in the condition, inline the expression.
- Extract nested anonymous structs into named types.
- Comment every blank import: `_ "github.com/lib/pq" // Postgres driver`.
- Group consecutive parameters of the same type: `func f(a, b string)`.
- Commit `go.sum` together with `go.mod`.
- Encrypt with AES-GCM; never use ECB, or CBC without authentication.

## More rules to follow
Write code that already complies with each one; the ones marked **critical** break builds or production.

### Bugs
- Non-existent operators like "=+" should not be used
- Test functions should not call "t.Fatal" from separate goroutines
- HTTP response bodies should be closed to prevent resource leaks
- Busy waiting loops should use proper synchronization

### Vulnerabilities
- OS commands should not be vulnerable to command injection attacks **(critical)**
- XPath expressions should not be vulnerable to injection attacks **(critical)**
- Deserialization should not be vulnerable to injection attacks **(critical)**
- HTTP request redirections should not be open to forging attacks **(critical)**
- Extracting archives should not lead to zip slip vulnerabilities **(critical)**
- Password hashing functions should use an unpredictable salt
- Cipher Block Chaining IVs should be unpredictable
- Cryptographic keys should be robust
- Passwords should not be stored in plaintext or with a fast hashing algorithm
- Insecure temporary file creation methods should not be used
- Cipher algorithms should be robust
- JWT should be signed and verified with strong cipher algorithms
- File permissions should not be set to world-accessible values
- Reflection should not be vulnerable to injection attacks
- System command arguments constructed from user input should be validated
- Environment variables should not be defined from untrusted input
- Accessing files should not lead to filesystem oracle attacks
- File existence checks followed by file creation should use atomic operations
