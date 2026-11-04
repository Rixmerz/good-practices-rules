# Changelog

All notable changes to this project are documented here. The format follows
[Keep a Changelog](https://keepachangelog.com/en/1.1.0/) and the project uses
[Semantic Versioning](https://semver.org/).

## [0.2.0] - 2026-11-04

### Added
- New rule files: Kotlin (with Android/Compose and coroutines) and Kotlin tests, C#/VB.NET (async,
  ASP.NET Core, .NET Framework) and C# tests, Ruby/Rails, Rust, Swift/SwiftUI, Scala, React/Next.js,
  Go tests, Terraform, Kubernetes/Helm, Docker Compose and GitHub Actions.
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

[0.2.0]: https://github.com/Rixmerz/good-practices-rules/releases/tag/v0.2.0
[0.1.1]: https://github.com/Rixmerz/good-practices-rules/releases/tag/v0.1.1
[0.1.0]: https://github.com/Rixmerz/good-practices-rules/releases/tag/v0.1.0
