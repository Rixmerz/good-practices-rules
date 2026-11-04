---
paths:
  - "**/*.cs"
  - "**/*.csproj"
  - "**/*.vb"
  - "**/*.vbproj"
---
<!-- Installed by good-practices-rules. Edit the package, not this copy: the next install replaces it. -->
# C# and VB.NET good practices: async, disposal, ASP.NET and security

## Top rules
- **Never use `async void`** except for event handlers; return `Task` so callers can await it and see exceptions.
- **Never block on async code** with `.Result`, `.Wait()` or `GetAwaiter().GetResult()`; it deadlocks under a synchronization context. Go async all the way.
- **Dispose what you create**: `using var stream = ...;` for every `IDisposable` you own. Don't dispose what you were given.
- **Rethrow with `throw;`**, never `throw ex;`, which resets the stack trace.
- **Never build SQL by concatenating strings**; use parameters (`SqlParameter`, Dapper `@param`, EF Core LINQ or `FromSqlInterpolated`).

## Async
- Async methods end in `Async` and accept a `CancellationToken` they pass down.
- In library code, use `ConfigureAwait(false)`.
- Don't wrap synchronous code in `Task.Run` inside ASP.NET request handlers.
- Don't return `null` from a method that returns `Task`; return `Task.CompletedTask` or `Task.FromResult`.

## Exceptions and logging
- Don't catch `Exception` without rethrowing or logging it with the exception object: `logger.LogError(ex, "...")`.
- Throw specific exceptions; use `ArgumentNullException.ThrowIfNull(arg)` for guards.
- Don't throw from `Dispose`, finalizers, `Equals`, `GetHashCode`, `ToString` or static constructors.
- Use structured logging templates: `logger.LogInformation("Order {OrderId}", id)`, not interpolation.

## Types and design
- Enable nullable reference types (`<Nullable>enable</Nullable>`) and don't silence warnings with `!` without a reason.
- Use `record` for immutable data and `readonly` for fields never reassigned.
- Override `Equals` and `GetHashCode` together, and implement `IEquatable<T>` on value types.
- Don't expose public fields; use properties. Don't expose `List<T>` publicly; use `IReadOnlyList<T>` or `IEnumerable<T>`.
- Make a class `sealed` unless it's designed for inheritance.
- Use `string.IsNullOrEmpty`/`IsNullOrWhiteSpace`, `StringComparison.Ordinal` for non-linguistic comparisons, and `StringBuilder` in loops.
- Don't enumerate an `IEnumerable` twice; materialize it with `ToList()` once.
- Use `DateTimeOffset` or UTC `DateTime`; never `DateTime.Now` for stored or compared values.
- Inject dependencies through the constructor; don't call `new` on services or use a service locator.
- Follow naming: `PascalCase` types, methods and properties; `camelCase` locals and parameters; `_camelCase` private fields; interfaces start with `I`.

## ASP.NET Core
- Validate input with model validation and check `ModelState` (or use `[ApiController]`).
- Put `[Authorize]` on controllers by default and `[AllowAnonymous]` only where needed.
- Use `[ValidateAntiForgeryToken]` on state-changing MVC actions with cookie auth.
- Don't return exception details to clients in production; use `UseExceptionHandler` and ProblemDetails.
- Use `IHttpClientFactory`, never `new HttpClient()` per request.
- Keep secrets in user-secrets, environment variables or a vault, never in `appsettings.json`.

## .NET Framework 4.x
- Set `ServicePointManager.SecurityProtocol` to TLS 1.2+ or target 4.7+ and let the OS decide; never force SSL3/TLS 1.0.
- Use `packages.config` or `PackageReference` with pinned versions; don't copy DLLs into the repo.

## Security
- Never use `BinaryFormatter`, `NetDataContractSerializer`, `SoapFormatter` or `LosFormatter`; use `System.Text.Json`.
- Don't set `TypeNameHandling` other than `None` in Json.NET on untrusted input.
- Use `RandomNumberGenerator`, not `Random`, for tokens and keys.
- Hash passwords with `PasswordHasher`/PBKDF2 (`Rfc2898DeriveBytes` with SHA-256 and 100k+ iterations), never MD5/SHA-1.
- Never set `ServerCertificateValidationCallback` to always return `true`.
- Use `XmlReader` with `DtdProcessing.Prohibit`; don't load untrusted XML with `XmlDocument` and a resolver.
- Don't pass user input to `Process.Start` or file paths without validating it.

## More rules to follow
Write code that already complies with each one; the ones marked **critical** break builds or production.

### Bugs
- "IDisposables" should be disposed
- Recursion should not be infinite **(critical)**
- Locks should be released on all paths
- "async void" methods should not be used outside event handlers
- Blocking calls should not be made on async code
- "GetHashCode" should not reference mutable fields
- Values should not be uselessly incremented
- Collection elements should not be replaced unconditionally
- Null pointers should not be dereferenced
- Integral numbers should not be shifted by zero or more than their number of bits-1

### Vulnerabilities
- Database queries should not be vulnerable to injection attacks **(critical)**
- OS commands should not be vulnerable to command injection attacks **(critical)**
- Deserialization should not be vulnerable to injection attacks **(critical)**
- XML parsers should not be vulnerable to XXE attacks **(critical)**
- Endpoints should not be vulnerable to reflected cross-site scripting attacks **(critical)**
- I/O function calls should not be vulnerable to path injection attacks **(critical)**
- LDAP queries should not be vulnerable to injection attacks **(critical)**
- Server-side requests should not be vulnerable to forging attacks **(critical)**
- Cipher algorithms should be robust
- Encryption algorithms should be used with secure mode and padding scheme
- Server certificates should be verified during SSL/TLS connections
- Weak SSL/TLS protocols should not be used
- JWT should be signed and verified with strong cipher algorithms
- Cookies should be "secure" and "HttpOnly"
- ASP.NET request validation should not be disabled

### Code smells
- Methods should not have too many parameters
- Cognitive complexity of methods should not be too high
- Unused private members should be removed
- "ConfigureAwait(false)" should be used in library code
- Exceptions should not be thrown from property getters
- Fields should not have public accessibility
