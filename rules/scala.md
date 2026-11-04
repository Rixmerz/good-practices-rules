---
paths:
  - "**/*.scala"
  - "**/*.sc"
  - "**/build.sbt"
---
<!-- Installed by good-practices-rules. Edit the package, not this copy: the next install replaces it. -->
# Scala good practices

- **Never use `null`**; use `Option`. Never call `.get` on an `Option` or `Try`; use `map`, `getOrElse`, `fold` or pattern matching.
- Prefer `val` and immutable collections; avoid `var` and `mutable` outside tight local scopes.
- Use `case class` for data and `sealed trait`/`enum` for closed hierarchies, and match exhaustively.
- Return errors as `Either`/`Try` or an effect type instead of throwing; don't `catch { case e: Throwable }`, use `NonFatal(e)`.
- Don't block inside a `Future` or effect (`Await.result`, `Thread.sleep`); provide an `ExecutionContext` explicitly.
- Annotate the return type of public methods.
- Don't use `return` inside closures.
- Avoid implicit conversions; keep `implicit`/`given` instances in companions or explicit imports.
- Never build SQL with string interpolation; use your library's bound parameters (`sql"... $x"` in Doobie/Slick binds safely, `s"..."` does not).
- Use `java.security.SecureRandom` for secrets.
- Format with `scalafmt` and keep `-Xfatal-warnings` (or `-Werror`) on.
