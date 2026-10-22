---
paths:
  - "**/*.{ts,tsx,js,jsx,mjs,cjs}"
---
<!-- Installed by good-practices-rules. Edit the package, not this copy: the next install replaces it. -->
# TypeScript/JavaScript good practices: modern APIs and style

## Strings
- Use `replaceAll('x', 'y')` instead of `replace(/x/g, 'y')`.
- Use `includes()` to check existence, not `indexOf() !== -1`. Use `startsWith()`/`endsWith()`, not a regex or `slice`.
- Use `RegExp.exec()` instead of `String.match()` without the `g` flag.
- Use `String.raw` instead of escaping many backslashes, and remove unnecessary escapes.
- Don't nest template literals. Use `codePointAt()` instead of `charCodeAt()`.

## Objects
- Use `structuredClone(obj)` instead of `JSON.parse(JSON.stringify(obj))`.
- Use spread `{ ...a, ...b }` instead of `Object.assign`, with no empty fallback: in `...(x ?? {})` the `?? {}` is redundant.
- Use `Object.hasOwn(obj, k)` instead of `hasOwnProperty`.
- Use `??` and `??=` for defaults, not `||` or ternaries.
- Use `Math.min()`/`Math.max()` instead of ternaries for simple comparisons.
- Pass `Boolean`, `String` or `Number` directly instead of wrapping them in an arrow function: `.filter(Boolean)`. Remove redundant `!!`.

## Arrays
- Use a `Set` when an array is only used for membership checks.
- Use `.some()` instead of `.filter().length` or `.find()` for existence checks. Use `.find()` instead of `.filter()[0]`.
- Use `.at(-1)` instead of `[arr.length - 1]`, and negative indices in `slice`.
- Use `indexOf()` instead of `findIndex()` for plain equality.
- Merge consecutive `push()` calls into one, `push(a, b)`. Don't clone with spread when it isn't needed.
- Don't check `length` before `some()`/`every()`. Use `for...of` instead of an indexed `for`.
- Use `flat()`/`flatMap()`, and spread instead of `apply()`.

## Modules
- **Merge imports from the same module into one `import`**. Keep a `type`-only import separate from a value import of the same module.
- Re-export with `export { x } from './x'`. Give default exports a name.

## Classes
- Declare static values as class fields, not in the constructor. Remove empty constructors.
- Use `globalThis` instead of `window`, `self` or `global`.
- Call built-in constructors consistently: `new Error()`, `new Map()`.
- Don't use `instanceof` with built-ins like `Array`; use `Array.isArray`.
- Remove unnecessary `.bind()` calls.

## DOM
- Use `el.remove()` instead of `parent.removeChild(el)`, and `el.dataset.x` instead of `getAttribute('data-x')`.
- Use `Date.now()` instead of `new Date().getTime()`.

## Other
- Write numbers without a zero fraction: `1`, not `1.0`.
- Use a character class `[abc]` instead of `a|b|c`. Group alternatives used with anchors: `^(a|b)$`. Don't put joined grapheme clusters in regex classes.
- In Axios, use native array destructuring instead of `axios.spread`.
- Don't use special identifiers like `arguments` or `eval` as variable names.
