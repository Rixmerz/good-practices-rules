---
paths:
  - "**/src/test/**/*.kt"
  - "**/src/androidTest/**/*.kt"
  - "**/*Test.kt"
---
<!-- Installed by good-practices-rules. Edit the package, not this copy: the next install replaces it. -->
# Kotlin good practices: tests (JUnit, kotlinx-coroutines-test, MockK)

- **Test coroutines with `runTest`**, never `runBlocking`; it skips delays and fails on leaked coroutines.
- Replace `Dispatchers.Main` with `Dispatchers.setMain(StandardTestDispatcher())` in setup and `resetMain()` in teardown, and pass the same test dispatcher to the code under test.
- Test flows with Turbine (`flow.test { awaitItem() }`) or `toList()` inside `runTest`, not with `delay`.
- Name tests with backticks that read as a sentence: `` fun `returns empty list when no bookings`() ``.
- Every test has at least one assertion; inside `assertFailsWith<T> { }` put exactly one call that can throw.
- Prefer fakes over mocks for your own interfaces; with MockK, use `coEvery`/`coVerify` for suspend functions and don't verify calls the test doesn't care about.
- Never use `Thread.sleep` in tests.
- In Compose UI tests, find nodes by `testTag` or semantics, not by text that will be translated.
