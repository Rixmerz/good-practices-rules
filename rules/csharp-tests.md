---
paths:
  - "**/*Tests.cs"
  - "**/*Test.cs"
  - "**/*.Tests/**/*.cs"
---
<!-- Installed by good-practices-rules. Edit the package, not this copy: the next install replaces it. -->
# C# good practices: tests (xUnit, NUnit, MSTest)

- **Give test methods a valid signature**: `public`, `async Task` (never `async void`), no generic parameters.
- Every test has an assertion, and every test class has at least one test.
- Use specific, complete assertions (`Assert.Equal(expected, actual)` with expected first, `Assert.Null`), not `Assert.True(a == b)` or a literal `true`/`false`.
- Use `Assert.Throws<T>`/`ThrowsAsync<T>` instead of `[ExpectedException]`.
- Don't leave tests ignored; fix or delete them.
- Don't use `Thread.Sleep` or the real clock; inject `TimeProvider` or a fake clock.

## More rules to follow
Write code that already complies with each one; the ones marked **critical** break builds or production.

### Code smells
- Tests should not be ignored
- Test classes should contain at least one test case **(critical)**
- Tests should include assertions **(critical)**
- Literal boolean values should not be used in assertions **(critical)**
- "Thread.Sleep" should not be used in tests
- Assertions should be complete **(critical)**
- Assertion arguments should be passed in the correct order
- "[ExpectedException]" should not be used
- Test method signatures should be correct **(critical)**
