---
paths:
  - "**/*Tests.cs"
  - "**/*Test.cs"
  - "**/*.Tests/**/*.cs"
---
<!-- Installed by good-practices-rules. Edit the package, not this copy: the next install replaces it. -->
# C# good practices: tests (xUnit, NUnit, MSTest)

- **Make async tests `async Task`**, never `async void` and never `.Result`.
- Every test has an assertion; use the specific one (`Assert.Equal(expected, actual)` with expected first, `Assert.Empty`, `Assert.Null`), not `Assert.True(a == b)`.
- Use `Assert.Throws<T>`/`ThrowsAsync<T>` with exactly one call inside the lambda.
- Tests that differ only in data become `[Theory]` + `[InlineData]` (xUnit) or `[TestCase]` (NUnit).
- Skip with a reason (`[Fact(Skip = "reason")]`, `[Ignore("reason")]`), never by commenting the test out.
- Don't use `Thread.Sleep` or the real clock; inject `TimeProvider` or a fake clock.
- Name tests `Method_Scenario_ExpectedResult`.
