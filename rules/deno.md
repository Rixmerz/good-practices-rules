---
paths:
  - "**/deno.json"
  - "**/deno.jsonc"
  - "**/*.{ts,tsx}"
---
<!-- Installed by good-practices-rules. Edit the package, not this copy: the next install replaces it. -->
# Good practices: Deno

Installed only in projects that have a `deno.json`; these rules apply to its TypeScript files.

## Top rules
- **Pin every dependency to a version** in the `imports` map of `deno.json` (`"@std/assert": "jsr:@std/assert@^1"`) and import by the bare name; never write `jsr:`/`npm:` specifiers without a version (no-unversioned-import) or full URLs scattered through the code (no-external-import).
- **Write explicit, extension-full relative imports** (`./math/add.ts`), never `./math/add` (no-sloppy-imports).
- **Never use `eval`** or `new Function` on strings (no-eval).
- **Replace deprecated `Deno.*` APIs** with their current equivalents (no-deprecated-deno-api); use `import ... with { type: "json" }`, not `assert` (no-import-assertions).
- **Every `// deno-lint-ignore` names the rule it silences**, the rule must exist and the directive must still be needed (ban-untagged-ignore, ban-unknown-rule-code, ban-unused-ignore); every `@ts-ignore`/`@ts-expect-error` carries a reason (ban-ts-comment).
- Type values precisely instead of `any` (no-explicit-any) and prefer narrowing over the `!` non-null assertion (no-non-null-assertion).
- Use async file and process APIs inside async functions, not `Deno.readTextFileSync` and friends (no-sync-fn-in-async-fn); only use `await` inside `async` functions (no-await-in-sync-fn), and drop `async` from functions that never await (require-await).
- Use `globalThis` or the bare Web API instead of `window` (no-window, no-window-prefix); import Node globals from `node:` modules (`import process from "node:process"`, `import { Buffer } from "node:buffer"`) (no-process-global, no-node-globals).
- Throw `Error` objects, never strings or literals (no-throw-literal); compare with `===`/`!==` (eqeqeq).
- Use `const` by default, `let` when reassigned, never `var` (prefer-const, no-var); remove unused variables and imports (no-unused-vars).

## More rules to follow
Write code that already complies with each one; the ones marked **critical** break builds or production.

Source: `deno lint` (https://docs.deno.com/lint/). The recommended set, plus opt-in rules that matter for security and correctness.

- no-eval: Disallows the use of eval. **critical**
- no-deprecated-deno-api: Warns the usage of the deprecated - Deno APIs. **critical**
- no-sloppy-imports: Enforces specifying explicit references to paths in module specifiers. **critical**
- no-unversioned-import: Ensure that inline dependency imports have a version specifier.
- no-external-import: Disallows the use of external imports.
- no-import-assertions: Disallows the assert keyword for import attributes.
- ban-ts-comment: Disallows the use of Typescript directives without a comment.
- ban-untagged-ignore: Requires deno-lint-ignore to be annotated with one or more rule names.
- ban-unknown-rule-code: Warns the usage of unknown rule codes in ignore directives.
- ban-unused-ignore: Warns unused ignore directives.
- no-explicit-any: Disallows use of the any type.
- no-non-null-assertion: Disallow non-null assertions using the ! postfix operator.
- no-sync-fn-in-async-fn: Disallow sync function inside async function.
- no-await-in-sync-fn: Disallow await keyword inside a non-async function. **critical**
- require-await: Disallows async functions that have no await expression or await using declaration.
- no-async-promise-executor: Requires that async promise executor functions are not used.
- no-unsafe-finally: Disallows the use of control flow statements within finally blocks.
- no-window: Disallows the use of the window object.
- no-window-prefix: Disallows the use of Web APIs via the window object.
- no-node-globals: Disallows the use of NodeJS global objects.
- no-process-global: Disallows the use of NodeJS process global.
- no-throw-literal: Disallow throwing literals as exceptions.
- eqeqeq: Enforces the use of type-safe equality operators === and !== instead of the more error prone == and != operators.
- no-global-assign: Disallows assignment to native Javascript objects.
- no-prototype-builtins: Disallows the use of Object.prototype builtins directly.
- no-fallthrough: Disallows the implicit fallthrough of case statements.
- no-cond-assign: Disallows the use of the assignment operator, =, in conditional statements.
- no-dupe-keys: Disallows duplicate keys in object literals.
- no-unreachable: Disallows the unreachable code after the control flow statements.
- no-control-regex: Disallows the use ASCII control characters in regular expressions.
- no-invalid-regexp: Disallows specifying invalid regular expressions in RegExp constructors.
- no-inner-declarations: Disallows variable or function definitions in nested blocks.
- no-invalid-triple-slash-reference: Warns the wrong usage of triple-slash reference directives.
- no-unused-vars: Enforces all variables are used at least once.
- prefer-const: Recommends declaring variables with const over let.
- no-var: Enforces the use of block scoped variables over more error prone function scoped variables.
