---
paths:
  - "**/*.kt"
  - "**/*.kts"
---
<!-- Installed by good-practices-rules. Edit the package, not this copy: the next install replaces it. -->
# Kotlin good practices: null safety, coroutines, Android and security

## Top rules
- **Never use `!!`**. Handle the null with `?.`, `?:`, `requireNotNull(x) { "why" }` or a smart cast after a check.
- **Never use `GlobalScope`**. Launch coroutines in a scope tied to a lifecycle: `viewModelScope`, `lifecycleScope`, or a `CoroutineScope` you cancel.
- **Don't catch `CancellationException` and swallow it**. If you catch `Exception` in a coroutine, rethrow `CancellationException` (or use `ensureActive()`), or the coroutine can't be cancelled.
- **Inject dispatchers** instead of hard-coding `Dispatchers.IO`/`Default` inside a class, so tests can swap them.
- Prefer `val` over `var`, and read-only `List`/`Map` over `MutableList`/`MutableMap` in public APIs.

## Null safety and types
- Don't declare a type nullable if it's never null.
- Don't write `if (x != null) x.foo() else null`; write `x?.foo()`.
- Use `lateinit` only for properties set by a framework or in setup; never for values that might stay unset.
- Use `data class` for value holders, `sealed interface`/`sealed class` for closed hierarchies, and an exhaustive `when` without `else` over them so new cases fail to compile.
- Don't use `Any` where a generic or a sealed type says what's really there.
- Use `object` for singletons, not a class with a `companion object` holding all the state.

## Coroutines and Flow
- A `suspend` function is main-safe: it switches to the right dispatcher itself with `withContext`.
- Don't call `runBlocking` in production code outside `main` and tests.
- Don't create a new `CoroutineScope(...)` inside a function and forget it; it leaks.
- Don't use `async { }` only to `await()` it right away; call the suspend function directly.
- Expose `StateFlow`/`Flow` read-only: a private `MutableStateFlow` and a public `asStateFlow()`.
- Collect flows in the UI with `repeatOnLifecycle`/`collectAsStateWithLifecycle`, not in `onCreate` without lifecycle awareness.
- Don't make `suspend` functions that never suspend.

## Style
- Follow the official style: `PascalCase` classes, `camelCase` functions and properties, `SCREAMING_SNAKE_CASE` for `const val`.
- Use string templates, not `+` concatenation.
- Use expression bodies for one-line functions: `fun area() = w * h`.
- Use `when` instead of a chain of `if`/`else if` on the same value.
- Use named arguments when passing several booleans or literals of the same type.
- Don't write `return` type `Unit` explicitly; don't leave unused parameters or imports.
- Use scope functions (`let`, `apply`, `also`, `run`) only when they make the code clearer; never nest them.

## Android and Compose
- Never keep a `Context`, `Activity`, `View` or `Fragment` in a `ViewModel`, a singleton or a companion object; it leaks the screen.
- Composables are side-effect free: put effects in `LaunchedEffect`/`DisposableEffect`, and state in `remember`/`rememberSaveable` or the `ViewModel`.
- Hoist state: a composable receives `value` and `onValueChange` instead of owning state it doesn't need.
- Pass stable, immutable types to composables; lambdas that capture changing state go through `rememberUpdatedState`.
- Give every interactive composable a `contentDescription` or a semantic label.
- Don't do disk or network work on the main thread.

## Security
- Never build SQL with string templates; use Room `@Query` parameters or `?` placeholders.
- Don't set `android:exported="true"` on a component that doesn't need it, and protect exported ones with permissions.
- Don't enable `WebView.settings.javaScriptEnabled` with `addJavascriptInterface` for untrusted content, and don't allow file access from URLs.
- Don't set `android:usesCleartextTraffic="true"` or a network security config that trusts user CAs in release.
- Don't store tokens or passwords in `SharedPreferences` in plain text; use `EncryptedSharedPreferences` or the Android Keystore.
- Don't log personal data or tokens with `Log.d`; strip debug logs from release.
- Use `SecureRandom` for anything secret, and AES-GCM for encryption.
- Don't accept every certificate with a custom `TrustManager` or `HostnameVerifier`.

## More rules to follow
Write code that already complies with each one; the ones marked **critical** break builds or production.

### Bugs
- "equals" should be overridden together with "hashCode"
- Coroutines should not be launched with "async" when the result is never awaited
- "runBlocking" should not be used inside a coroutine **(critical)**
- Jump statements should not be used inside "finally" blocks
- Identical expressions should not be used on both sides of a binary operator
- Collection sizes and array lengths should not be compared with negative numbers
- Infinite loops should have an exit condition

### Vulnerabilities
- OS commands should not be vulnerable to command injection attacks **(critical)**
- Database queries should not be vulnerable to injection attacks **(critical)**
- Server-side requests should not be vulnerable to forging attacks **(critical)**
- XML parsers should not be vulnerable to XXE attacks **(critical)**
- Cipher algorithms should be robust
- Encryption algorithms should be used with secure mode and padding scheme
- Server certificates should be verified during SSL/TLS connections
- Server hostnames should be verified during SSL/TLS connections
- Weak SSL/TLS protocols should not be used
- Cryptographic keys should be robust
- Passwords should not be stored in plaintext or with a fast hashing algorithm
- Mobile database encryption keys should not be disclosed
- Biometric authentication should be bound to a cryptographic operation
- Exported components should be protected by permissions
- WebViews should not allow access to the file system

### Code smells
- Dispatchers should be injectable
- "suspend" functions should not be called from a "GlobalScope"
- Flows should be exposed as read-only
- Classes should not have too many functions
- Functions should not have too many parameters
- Empty functions and blocks should be avoided
- Unused private declarations should be removed
