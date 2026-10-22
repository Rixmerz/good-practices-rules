<!-- Installed by good-practices-rules. Edit the package, not this copy: the next install replaces it. -->
# Good practices: how to use these rules

The files in this folder load by file type (their `paths:` frontmatter); `secrets.md` and `repo.md` load always. Apply them to every line you write or touch, not only to new files.

- Fix the cause, never silence a linter or analyzer warning without a one-line reason next to it.
- Rules marked **critical** break builds or production: never leave one for later.
- If you find a secret already committed, it must be revoked and rotated, because deleting it leaves it in git history. Tell the user; don't just replace it.
- When reviewing code against these rules, report findings by file and say that you checked by reading, not with a tool.
