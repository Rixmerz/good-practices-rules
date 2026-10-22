# good-practices-rules

Good-practice rules for [Claude Code](https://code.claude.com), installed only for the languages a project actually uses. Claude reads them while it writes code, so common bugs, security holes and maintainability problems never make it into a commit.

- **1,100+ rules** across 13 technologies, written as concrete instructions with the right way to do it.
- **Per language and on demand**: each rule file has a `paths:` frontmatter, so it enters Claude's context only when Claude reads or edits a file of that language.
- **Multi-repo aware**: point it at a folder that holds several repos of one project and it installs the rules for all of them.

## Install

```bash
git clone https://github.com/Rixmerz/good-practices-rules.git
cd good-practices-rules
./install.sh --install-cmd      # once: puts `good-practices` on the PATH
```

Then, from anywhere:

```bash
good-practices                  # the current directory
good-practices /path/to/project # only the languages found there
good-practices --dry-run /path  # show what it would do
good-practices --all /path      # every rule, regardless of extensions
good-practices --user-scope     # every rule in ~/.claude, for every project
```

| System | Requirement | Where the command goes |
| --- | --- | --- |
| macOS / Linux | bash (macOS's 3.2 works) | `~/.local/bin/good-practices` |
| Windows | Git Bash (Git for Windows, which Claude Code also uses) | `~/bin/good-practices` and `~/bin/good-practices.cmd` for cmd/PowerShell; the installer prints how to add that folder to the Path |

`$GOOD_PRACTICES_BIN` changes where the command goes. The command is a wrapper around this folder's `install.sh`, so `git pull` updates it; run `--install-cmd` again if you move the folder.

## What it installs

It walks the folder recursively (skipping `node_modules`, `.git`, `dist`, `build`, `target`, `.venv`, `vendor`…), collects the file extensions and copies the matching rules into `.claude/rules/good-practices/`:

```text
my-project/
├── .claude/
│   └── rules/
│       └── good-practices/
│           ├── 00-how-to.md      always loaded
│           ├── secrets.md        always loaded
│           ├── repo.md           always loaded
│           ├── general.md        any code file
│           ├── java.md           **/*.java
│           ├── python.md         **/*.py
│           └── typescript.md     **/*.{ts,tsx,js,jsx,mjs,cjs}
├── api-java/
├── api-python/
└── web-angular/
```

| Extensions found | Rules |
| --- | --- |
| `ts tsx js jsx mjs cjs` | typescript, typescript-style, typescript-tests, general |
| `angular.json` or `*.component.ts` | the above + angular, html, css |
| `java` | java, java-style, java-tests, general |
| `py` | python, python-tests, general |
| `go` · `php` | go · php, general |
| `html` · `css scss sass less` · `xml` | html · css · xml |
| `sh bash zsh` · `sql pks pkb…` · `ps1` | shell · plsql · powershell |
| `Dockerfile*` | docker |

`secrets`, `repo` and `00-how-to` are always installed and always loaded (about 3 KB). Open Claude Code in the folder you installed into, so the rules cover every repo inside it.

## Global or per project

`--user-scope` puts every rule in `~/.claude/rules/good-practices/`. Each language rule still loads only when Claude touches that language. A per-project install makes sense when you want to commit the rules for the whole team. Pick one: with both, Claude loads each rule twice (the installer warns about it).

## Updating

Run it again. It never deletes anything and never leaves two versions of a rule side by side:

| Case | What it does |
| --- | --- |
| New language in the folder | Adds only its files |
| A file it installed before | Replaces it in place |
| A foreign file with the same name | Backs it up into `.claude/good-practices-backup/<date>/`, then replaces it |
| A file of its own that a newer version renamed or removed | Moves it into the backup folder |
| Any other foreign file in the folder | Leaves it and warns that it may repeat rules |

Don't edit the installed copies: the next run replaces them. Change `rules/` in your fork instead.

## What the rules look like

```markdown
---
paths:
  - "**/*.java"
---
# Java good practices: design, Spring, errors and security

## Top five
- **Use constructor injection**, never `@Autowired` on a field. Make the fields `private final`.
- **Never throw generic `Exception`, `RuntimeException` or `Throwable`**. Throw a specific one.
- **Don't evaluate log arguments eagerly**: write `log.debug("id {}", id)`, not `log.debug("id " + id)`.
...
## More rules to follow
- Resources should be closed
...
```

## License

[MIT](LICENSE)
