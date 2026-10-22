---
paths:
  - "**/src/test/**/*.java"
  - "**/*Test.java"
  - "**/*Tests.java"
  - "**/*IT.java"
---
<!-- Installed by good-practices-rules. Edit the package, not this copy: the next install replaces it. -->
# Java good practices: tests (JUnit 5, Mockito, AssertJ)

## JUnit 5
- **Leave test classes and methods package-private**: `void createsBooking()`, not `public void`.
- A test class has at least one `@Test`. Don't put two `@BeforeEach` methods (or two of any lifecycle type) in one class.
- Inside `assertThrows`, the lambda contains **exactly one call** that can throw; set up data before the lambda.
- Use the dedicated assertion: `assertEquals(a, b)`, not `assertTrue(a.equals(b))`.
- Never use `Thread.sleep` in tests; use Awaitility or a fake `Clock`.
- Don't use the system clock; inject `Clock.fixed(...)`.
- A test has at most 25 assertions; split it if it needs more.

## Mockito
- Statically import `when`, `verify`, `mock` and `any` (`import static org.mockito.Mockito.*`).
- If every argument is `eq(...)`, pass the values directly: `when(repo.find(1L))`, not `when(repo.find(eq(1L)))`.
- Use `@InjectMocks` instead of building the object under test by hand with the mocks.
- Don't create mocks inline inside `thenReturn(mock(...))`; create them first in a variable.

## AssertJ
- Chain assertions on the same subject: `assertThat(x).isNotNull().hasSize(2)`.
- Use the dedicated assertion: `containsEntry(k, v)`, `hasSize(n)`, `isEmpty()`, not `assertThat(map.get(k)).isEqualTo(v)`.
- Before `allMatch` or `doesNotContain`, assert the list isn't empty.
