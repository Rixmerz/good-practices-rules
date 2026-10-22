# Changelog

All notable changes to this project are documented here. The format follows
[Keep a Changelog](https://keepachangelog.com/en/1.1.0/) and the project uses
[Semantic Versioning](https://semver.org/).

## [0.1.0] - 2026-10-22

### Added
- 22 Claude Code rules covering TypeScript/JavaScript, Angular, Java, Python, Go, PHP, HTML, CSS,
  Dockerfile, XML, Shell, SQL/PL-SQL and PowerShell, plus secrets and repository hygiene.
- `install.sh`: detects the languages under a folder (several repos at once) and installs only their rules;
  `--user-scope` installs every rule globally; `--dry-run`, `--all`.
- `install.sh --install-cmd`: puts a `good-practices` command on the PATH (macOS/Linux, Windows via Git Bash).
- Safe updates: replaces its own files in place, backs up foreign ones, retires files a newer version drops.

[0.1.0]: https://github.com/Rixmerz/good-practices-rules/releases/tag/v0.1.0
