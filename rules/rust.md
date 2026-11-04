---
paths:
  - "**/*.rs"
  - "**/Cargo.toml"
---
<!-- Installed by good-practices-rules. Edit the package, not this copy: the next install replaces it. -->
# Rust good practices: errors, ownership, unsafe and security

## Top rules
- **Don't `unwrap()` or `expect()` in library or request-handling code**; return `Result` and use `?`. `expect("why it can't fail")` is fine for true invariants.
- **Every `unsafe` block has a `// SAFETY:` comment** saying which invariant makes it sound. Keep `unsafe` blocks as small as possible.
- **Code must pass `cargo clippy -- -D warnings` and `cargo fmt --check`**; fix the lint rather than adding `#[allow]` without a reason.
- Commit `Cargo.lock` for binaries and applications.

## Errors
- Libraries define error types (`thiserror`); applications can use `anyhow` with `.context("what was being done")`.
- Don't use `panic!` for recoverable errors.
- Don't discard a `Result` with `let _ =` unless you comment why.

## Ownership and types
- Take `&str`, `&[T]` or `impl AsRef<...>` in parameters instead of `&String` or `&Vec<T>`.
- Don't `clone()` to silence the borrow checker; restructure or borrow.
- Use newtypes and enums to make invalid states unrepresentable.
- Use iterators instead of index loops; don't `collect()` only to iterate again.
- Derive `Debug` on public types; implement `Display` for user-facing errors.
- Mark functions whose result must be used with `#[must_use]`.

## Concurrency and async
- Don't hold a `std::sync::Mutex` guard across an `.await`; use `tokio::sync::Mutex` or drop the guard first.
- Don't call blocking code in async tasks; use `spawn_blocking`.
- Prefer channels or `Arc<Mutex<T>>` with short critical sections over global mutable state; never use `static mut`.

## Security
- Never build SQL with `format!`; use `sqlx` bind parameters or a query builder.
- Use `rand::rngs::OsRng`/`getrandom` for secrets, and vetted crates (`ring`, `rustls`, `argon2`) for crypto.
- Don't disable certificate verification (`danger_accept_invalid_certs(true)`).
- Run `cargo audit` and keep dependencies pinned through the lockfile.
- Validate paths from user input before joining them; `Path::join` with an absolute path replaces the base.

## More rules to follow
Write code that already complies with each one; the ones marked **critical** break builds or production.

### Bugs
- Integer arithmetic should not overflow in release builds without explicit handling (`checked_*`, `wrapping_*`, `saturating_*`)
- Infinite recursion should not occur **(critical)**
- Mutex guards should not be held across await points

### Vulnerabilities
- OS commands should not be vulnerable to command injection attacks **(critical)**
- Database queries should not be vulnerable to injection attacks **(critical)**
- Server certificates should be verified during SSL/TLS connections
- Cipher algorithms should be robust
