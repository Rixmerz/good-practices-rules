# Changelog

All notable changes to this project are documented here. The format follows
[Keep a Changelog](https://keepachangelog.com/en/1.1.0/) and the project uses
[Semantic Versioning](https://semver.org/).

## Unreleased

### Changed
- Swift rules mark which SwiftLint rules are opt-in or analyzer-only, checked against the SwiftLint source.

## [0.2.2] - 2026-11-04

### Changed
- Shell, Go and Angular rules now name the linter rule that checks each one (ShellCheck, go vet, staticcheck, revive, angular-eslint).

### Removed
- Go and Angular bullets that no linter checks: `-er` interface names, inlining `if` short statements, nested anonymous structs, committing `go.sum`, AES-GCM advice, `bypassSecurityTrust*` advice.

## [0.2.1] - 2026-11-04

### Fixed
- Rules added in 0.2.0 were partly written from opinion, and some "More rules to follow" titles imitated
  SonarSource without existing. Every rule now maps to a named analyzer or linter:
  - Kotlin, C#/VB.NET, Ruby, Scala, Rust, React, Terraform and Kubernetes: rule lists generated from the
    SonarSource analyzers' rule metadata (exact titles, "Sonar way" profile, severity), and top rules
    trimmed to the ones that match.
  - Swift: SwiftLint. GitHub Actions: zizmor and actionlint. Deno and Fresh: `deno lint`. Hono: the
    hono-dev plugin's 36 rules.
  - Bullets added in 0.2.0 to shell, Go, Angular and the TypeScript/Python test rules: kept only where
    ShellCheck, `go vet`/staticcheck, angular-eslint, eslint-plugin-jest/vitest/testing-library or ruff `PT`
    has the rule.

### Removed
- `compose.md`, `kotlin-tests.md` and `go-tests.md`: no public analyzer rules to base them on.

### Added
- README section on where the rules come from; `00-how-to.md` says not to add rules no tool checks.

## [0.2.0] - 2026-11-04

### Added
- New rule files: Kotlin (with Android/Compose and coroutines) and Kotlin tests, C#/VB.NET (async,
  ASP.NET Core, .NET Framework) and C# tests, Ruby/Rails, Rust, Swift/SwiftUI, Scala, React/Next.js,
  Go tests, Terraform, Kubernetes/Helm, Docker Compose, GitHub Actions, Deno, Fresh and Hono.
- Installer reads `deno.json`, `package.json` and `import_map.json` to detect Deno, Fresh and Hono.
- Installer detects `.kt`, `.cs`, `.vb`, `.rb`, `.rs`, `.swift`, `.scala`, `.tf`, `.tsx`/`.jsx` (React),
  Compose files, GitHub workflows and Kubernetes manifests.
- Shell: `set -euo pipefail`, quoting, `mktemp`/`trap`, safe `rm`, ShellCheck.
- Go: error wrapping with `%w` and `errors.Is/As`, goroutine and channel ownership, HTTP timeouts.
- Angular: OnPush, signals, unsubscribing, control flow with `track`, typed forms.
- TypeScript and Python tests: async assertions, fake timers, focused tests, Testing Library, parametrize,
  `tmp_path`, `pytest.approx`, mocking where the name is looked up.

### Changed
- Kotlin files used to get only `general.md`; they now get `kotlin.md` and `kotlin-tests.md`.

## [0.1.1] - 2026-10-27

### Added
- TypeScript: intersect only meaningful types (`T & null`, `T & any`, `& {}`); promoted from the
  short list to a written rule with the fix.

## [0.1.0] - 2026-10-22

### Added
- 22 Claude Code rules covering TypeScript/JavaScript, Angular, Java, Python, Go, PHP, HTML, CSS,
  Dockerfile, XML, Shell, SQL/PL-SQL and PowerShell, plus secrets and repository hygiene.
- `install.sh`: detects the languages under a folder (several repos at once) and installs only their rules;
  `--user-scope` installs every rule globally; `--dry-run`, `--all`.
- `install.sh --install-cmd`: puts a `good-practices` command on the PATH (macOS/Linux, Windows via Git Bash).
- Safe updates: replaces its own files in place, backs up foreign ones, retires files a newer version drops.

[0.2.1]: https://github.com/Rixmerz/good-practices-rules/releases/tag/v0.2.1
[0.2.0]: https://github.com/Rixmerz/good-practices-rules/releases/tag/v0.2.0
[0.1.1]: https://github.com/Rixmerz/good-practices-rules/releases/tag/v0.1.1
[0.1.0]: https://github.com/Rixmerz/good-practices-rules/releases/tag/v0.1.0
