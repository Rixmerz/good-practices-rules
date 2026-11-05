---
paths:
  - "**/*.kt"
  - "**/*.kts"
---
<!-- Installed by good-practices-rules. Edit the package, not this copy: the next install replaces it. -->
# Kotlin good practices: null safety, coroutines, Android and security

## Top rules
- **Never use `GlobalScope`**. Launch coroutines in a scope tied to a lifecycle (`viewModelScope`, `lifecycleScope`) or a `CoroutineScope` you cancel.
- **Inject dispatchers** instead of hard-coding `Dispatchers.IO`/`Default` inside a class, so tests can swap them.
- **Never hard-code secrets or credentials**; read them from the environment, a vault or the Android Keystore.
- **Verify server certificates and hostnames**; never install a `TrustManager` or `HostnameVerifier` that accepts everything.
- Prefer `val` over `var`, and read-only `List`/`Map` over `MutableList`/`MutableMap` when the contents never change.

## Coroutines and Flow
- A `suspend` function is main-safe: it switches to the right dispatcher itself with `withContext`.
- Don't wrap a call in `withContext` for a dispatcher the callee already switches to.
- Expose `StateFlow`/`SharedFlow` read-only: a private `MutableStateFlow` and a public `asStateFlow()`.
- Don't make `suspend` functions that never suspend, and don't mark functions that return `Flow` or `Channel` as `suspend`.
- In a `ViewModel`, launch coroutines in `viewModelScope` instead of exposing `suspend` functions to the UI.
- Don't drop the result of a `Flow` or `Sequence` intermediate operation; it does nothing unless collected.

## Style
- Follow the naming convention: `PascalCase` classes, `camelCase` functions.
- Use `object` for singletons, not a class with a private constructor and a companion holding the instance.
- Use `when` instead of a chain of `if`/`else if` on the same value.
- Don't leave unused parameters, local variables or imports.
- Don't use `return`, `break` or `continue` inside `finally`.

## Security
- Use `SecureRandom` for anything secret, and AES-GCM (or another secure mode and padding) for encryption; never reuse a GCM/CTR IV.
- Use TLS 1.2+ and `https://`; never weak SSL/TLS versions or clear-text protocols.
- Hash passwords with a slow, salted algorithm (bcrypt, PBKDF2, Argon2); never MD5 or SHA-1.
- Don't let a `WebView` load file URLs with unrestricted file access, and don't call `addJavascriptInterface` for untrusted content.
- Register broadcast receivers with a permission, and send broadcasts with a receiver permission.
- Don't write sensitive data to external storage.
- Enable obfuscation (`isMinifyEnabled = true`) for release builds.

## More rules to follow
Write code that already complies with each one; the ones marked **critical** break builds or production.

### Bugs
- Jump statements should not occur in "finally" blocks **(critical)**
- Useless "if(true) {...}" and "if(false){...}" blocks should be removed
- "equals(Any?)" and "hashCode()" should be overridden in pairs
- Variables should not be self-assigned
- All code should be reachable
- Identical expressions should not be used on both sides of a binary operator
- Related "if/else if" statements should not have the same condition
- "equals(other: Any?)" should test the argument's type
- Collections should not be passed as arguments to their own methods
- "hashCode" and "toString" should not be called on array instances
- "ScheduledThreadPoolExecutor" should not have 0 core threads **(critical)**
- Values should not be uselessly incremented
- "runFinalizersOnExit" should not be called **(critical)**
- Inappropriate collection calls should not be made
- "PreparedStatement" and "ResultSet" methods should be called with valid indices **(critical)**
- Non-existent operators like "=+" should not be used
- All branches in a conditional structure should not have exactly the same implementation
- Intermediate Sequence and Stream functions should not be left unused
- Collection and array sizes comparisons should make sense
- Repeated patterns in regular expressions should not match the empty string
- Alternatives in regular expressions should be grouped when used with anchors
- Regular expressions should be syntactically valid **(critical)**
- Unicode Grapheme Clusters should be avoided inside regex character classes
- Equals method should be overridden in data classes containing array fields
- Flow intermediate operation results should not be left unused
- "Map" values should be accessed safely
- Return values should not be ignored when they contain the operation status code

### Vulnerabilities
- Password hashing functions should use an unpredictable salt **(critical)**
- Credentials should not be hard-coded
- Pseudorandom number generators (PRNGs) should not be used in security contexts
- Cipher Block Chaining IVs should be unpredictable **(critical)**
- Secure random number generators should not output predictable values **(critical)**
- Weak SSL/TLS protocols should not be used **(critical)**
- Cryptographic keys should be robust **(critical)**
- Debugging features should not be enabled in production
- Weak hashing algorithms should not be used **(critical)**
- Server certificates should be verified during SSL/TLS connections **(critical)**
- Intents should not be broadcast without receiver permissions
- Android broadcast receivers should not be registered without a permission **(critical)**
- Sensitive data should not be stored in Android external storage **(critical)**
- Clear-text protocols should not be used
- Passwords should not be stored in plaintext or with a fast hashing algorithm **(critical)**
- Server hostnames should be verified during SSL/TLS connections **(critical)**
- Encryption algorithms should be used with secure mode and padding scheme **(critical)**
- Cipher algorithms should be robust **(critical)**
- Android KeyStore keys should require user authentication
- Biometric authentication should be cryptographically bound
- Mobile database encryption keys should not be disclosed
- WebViews should not allow unrestricted file access
- Secrets should not be hard-coded **(critical)**
- Counter Mode initialization vectors should not be reused **(critical)**
- Remote artifacts should be verified for authenticity and integrity
- Obfuscation should be enabled for release builds **(critical)**
- Native methods should not be exposed to untrusted WebView content
- Keyboard cache should be disabled for password inputs
- Android production release targets should not be debuggable
- Avoid using persistent unique identifiers

### Code smells
- Mergeable "if" statements should be combined
- Functions should not have too many parameters
- Nested blocks of code should not be left empty
- Redundant pairs of parentheses should be removed
- Track uses of "FIXME" tags
- Unused "private" methods should be removed
- Unused function parameters should be removed
- Functions should not be empty **(critical)**
- String literals should not be duplicated **(critical)**
- Sections of code should not be commented out
- "when" statements should not have too many clauses
- Two branches in a conditional structure should not have exactly the same implementation
- Cognitive Complexity of functions should not be too high **(critical)**
- Functions should not have identical implementations
- Native features should be preferred to Guava
- Regular expressions should not be too complicated
- Empty lines should not be tested with regex MULTILINE flag **(critical)**
- Character classes in regular expressions should not contain the same character twice
- Operator "is" should be used instead of "isInstance()"
- Redundant methods should be avoided in data classes
- "MutableStateFlow" and "MutableSharedFlow" should not be exposed
- Coroutine usage should adhere to structured concurrency principles
- Suspending functions should be main-safe
- Functions returning Flow/Channel should not be suspending
- Dispatchers should be injectable
- Suspending functions should not be called on a different dispatcher
- Extension functions on CoroutineScopes should not be declared as "suspend"
- ViewModel classes should create coroutines
- The return value of functions returning "Deferred" should be used
- Kotlin coroutines API for timeouts should be used
- "Unit" should be used instead of "Void"
- "return" statements should be lifted before "if" or "when" statement
- "when" statements should be used instead of chained "if" statements
- Delegator pattern should use "by" clause
- Singleton pattern should use object declarations or expressions
- Functional interface implementations should use lambda expressions
- Single function interfaces should be functional interfaces
- Element access should use indexed access operators
- Structural equality tests should use "==" or "!="
- Collection should be immutable if contents is not changed
- Abstract class should be interface
- Expression should be simplified with "isEmpty", "isNotEmpty" or "isNullOrEmpty"
- Type casts and type checks that can never succeed should be removed
- Redundant type casts should be removed
- Check for preconditions should be simplified
- "It" shouldn't be used as a lambda parameter name
- Variables assigned values should be read
- Null checks should be useful
- "tasks.register()" should be preferred over "tasks.create()"
- Dependency versions shouldn't be hard-coded
- "rootProject.name" should always be present in Gradle settings
- Tasks should define "description" and "group"
- Dependencies should be grouped by destination
- Gradle settings file should always be present
