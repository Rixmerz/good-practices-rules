---
paths:
  - "**/*.{css,scss,sass,less}"
---
<!-- Installed by good-practices-rules. Edit the package, not this copy: the next install replaces it. -->
# Good practices: CSS/SCSS

- **Ensure enough text/background contrast**: 4.5:1 for normal text and 3:1 for large text. Use the design system's color tokens, which already meet it, rather than custom hex values.
- Don't repeat a selector in the same file or a property within a block.
- No empty style files. If a component has no styles, delete the file and remove `styleUrl`.
- End every `font-family` list with a generic family, e.g. `font-family: Barlow, sans-serif`, and don't list a font twice.
- Don't put a shorthand after its longhands, because it overrides them (e.g. `padding` after `padding-top`).
- Put `@import` before every other rule. Don't vendor-prefix at-rules.
- No extra `;`, no commented-out CSS, and no deprecated properties.
- Use only valid selectors, media features and pseudo-classes.
- In nested CSS, use `&` to refer to the parent selector: `.button { &.active { ... } }`. The rule is noisy; don't rewrite working SCSS just for it.

## More rules to follow
Write code that already complies with each one; the ones marked **critical** break builds or production.

### Bugs
- Color definitions should be valid **(critical)**
- "calc" operands should be correctly spaced **(critical)**
- Units should be valid **(critical)**
- CSS properties should be valid **(critical)**
- Single line comment syntax should not be used **(critical)**
- At-rule descriptor values should be valid **(critical)**
- "linear-gradient" directions should be valid
- An+B selectors should be matchable
- "!important" should not be used on "keyframes"
- Pseudo-element selectors should be valid
- "at-rules" should be valid
- CSS annotations should be known
- CSS declarations should be placed in valid positions
- Descriptors within "at-rules" should be valid
- Keyframe selectors should not be duplicated
- "at-rule" preludes should be valid

### Code smells
- "!important" should not be used
- Empty blocks should be removed
