---
paths:
  - "**/*.{spec,test}.{ts,tsx,js,jsx,mjs,cjs}"
  - "**/__tests__/**"
---
<!-- Installed by good-practices-rules. Edit the package, not this copy: the next install replaces it. -->
# TypeScript/JavaScript good practices: tests (Jest, Jasmine, Vitest)

- **Use the most specific assertion**: `toHaveLength(2)`, not `expect(a.length).toBe(2)`; `toBeNull()`, not `toBe(null)`; `toHaveBeenCalledWith(x)`, not inspecting `mock.calls`; `toContain(x)`, not `includes(x)` plus `toBe(true)`.
- Don't compare incompatible types in an assertion, like `expect(aNumber).toBe('1')`.
- Test titles are unique within a `describe`.
- Every spec file contains at least one test.
- No duplicate cases in `it.each`/`test.each`.
- Every async test awaits its promises or returns them; never leave a floating `expect(...).resolves`.
- Don't leave `.only` or `.skip` (`fit`, `fdescribe`, `xit`) in committed tests.
- In Testing Library, use `userEvent` over `fireEvent`, and `await findBy*` instead of `waitFor` + `getBy*` for async content.
- Remove unnecessary `as` casts and `!` assertions in tests too.
