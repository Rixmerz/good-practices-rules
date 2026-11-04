---
paths:
  - "**/*_test.go"
---
<!-- Installed by good-practices-rules. Edit the package, not this copy: the next install replaces it. -->
# Go good practices: tests

- **Write table-driven tests** with `t.Run(tc.name, ...)` for cases that differ only in data.
- Mark helpers with `t.Helper()` so failures point at the caller.
- Use `t.Cleanup` and `t.TempDir()` instead of manual teardown and fixed paths; use `t.Setenv` for environment variables.
- Call `t.Fatal`/`t.FailNow` only from the test goroutine; from other goroutines report with `t.Error` or a channel.
- Report got and want: `t.Errorf("Parse(%q) = %v, want %v", in, got, want)`.
- Compare structs with `cmp.Diff` (or `reflect.DeepEqual`), errors with `errors.Is`.
- Run tests with `-race` in CI; mark independent tests `t.Parallel()`.
- Don't sleep to wait for goroutines; synchronize with channels or `sync.WaitGroup`.
- Use `httptest.NewServer`/`httptest.NewRecorder` for HTTP code, never a real network.
