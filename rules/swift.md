---
paths:
  - "**/*.swift"
---
<!-- Installed by good-practices-rules. Edit the package, not this copy: the next install replaces it. -->
# Swift good practices: optionals, concurrency, iOS and security

## Top rules
- **Never force-unwrap (`!`) or force-try (`try!`)** outside tests; use `guard let`, `if let`, `??` or `do/catch`.
- **Don't use implicitly unwrapped optionals (`String!`)** except for `@IBOutlet`.
- **Capture `self` weakly** (`[weak self]`) in escaping closures stored by `self` or long-lived objects, to avoid retain cycles.
- **Update UI only on the main actor**: mark UI types `@MainActor`.

## Language
- Prefer `let` over `var`, and `struct`/`enum` over `class` unless you need reference semantics.
- Mark classes `final` unless designed for subclassing.
- Use `guard` for early exits.
- Switch exhaustively over enums; avoid `default` so new cases are caught.
- Don't use `as!`; use `as?` and handle the failure.
- `UpperCamelCase` types and protocols, `lowerCamelCase` everything else.

## Concurrency
- Prefer `async/await` and actors over completion handlers and `DispatchQueue` for new code.
- Enable strict concurrency checking and make shared types `Sendable`.
- Cancel tasks you start in a view or object when it goes away; check `Task.isCancelled` in long loops.

## SwiftUI
- Own state with `@State`/`@StateObject` (or `@Observable`); pass it down with bindings, don't recreate view models in `body`.
- Keep `body` free of side effects; use `.task` and `.onChange`.
- Give images and icon buttons an accessibility label.

## Security
- Store tokens and passwords in the Keychain, never in `UserDefaults` or files.
- Don't disable App Transport Security (`NSAllowsArbitraryLoads`) in release.
- Don't accept any certificate in `URLSessionDelegate`.
- Use `SecRandomCopyBytes` or CryptoKit for keys and tokens; use CryptoKit `AES.GCM`, never ECB.
- Don't log personal data with `print`/`NSLog`; use `Logger` with `privacy: .private`.

## More rules to follow
Write code that already complies with each one; the ones marked **critical** break builds or production.

### Vulnerabilities
- Server certificates should be verified during SSL/TLS connections
- Cipher algorithms should be robust
- Encryption algorithms should be used with secure mode and padding scheme
- Cryptographic keys should be robust
- Weak SSL/TLS protocols should not be used

### Code smells
- Functions should not have too many parameters
- Closures should not be too long
- Force casts should not be used
