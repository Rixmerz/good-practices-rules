---
paths:
  - "**/*.html"
  - "**/*.htm"
---
<!-- Installed by good-practices-rules. Edit the package, not this copy: the next install replaces it. -->
# HTML and templates good practices: accessibility and HTML5

## Top rules
- **Don't use attributes deprecated in HTML5**: `align`, `bgcolor`, `border`, table `width`/`height`, `cellpadding`, `cellspacing`, `valign`. Use CSS or the design system's classes instead. Don't use deprecated elements like `<center>`, `<font>` or `<marquee>` either.
- **Every `(click)`/`onclick` on a non-interactive element needs a keyboard equivalent**: `(keydown.enter)` plus `tabindex="0"`. **Better: use a `<button>`**, which handles the keyboard already.
- **Prefer the native element over an ARIA role**: `<button>`, not `<div role="button">`; `<a href>`, not `role="link"`.
- **Associate every `input`, `select` and `textarea` with a `<label for="id">`** or give it an `aria-label`. Every `<label>` has text and an associated control.
- Never leave commented-out HTML.

## Tables
- Data tables use `<th>` with `scope`. Never use `<table>` for layout; use CSS grid or flex.

## Images and media
- Every `<img>`, `<area>` and `<input type="image">` has an `alt`; decorative ones use `alt=""`. Don't put words like "image" or "photo" in the `alt`.
- `<iframe>` elements have a `title`. Videos have captions via `<track>`.

## ARIA
- Put `tabindex` only on interactive elements.
- Don't give interactive roles to non-interactive elements, or the reverse.
- Don't repeat an implicit role, like `<form role="form">`.
- Use only the ARIA properties a role supports, and include the required ones, e.g. `aria-checked` on `role="checkbox"`.
- The accessible name contains the visible label text. Distinguish look-alike elements with `aria-label`.
- Use valid `autocomplete` values on form fields.

## Structure
- Every `id` is unique on the page. **Watch out in `@for`/`*ngFor`**: build the id from the index.
- Full pages have `<!DOCTYPE html>`, `<html lang="es">` and a `<title>`.
- `<li>` goes inside `<ul>`/`<ol>`, and `<dt>` inside `<dl>`.
- Scripts and styles from a CDN carry `integrity` and `crossorigin`.

## More rules to follow
Write code that already complies with each one; the ones marked **critical** break builds or production.

### Bugs
- Tables used for layout should not include semantic markup
- Table cells should reference their headers
- <script>...</script> elements should not be nested
- The "role" attribute should be used instead of "aria-role"
- "<legend>" tags should be direct children of "<fieldset>" or "<optgroup>" elements
- "<th>" tags should have "id" or "scope" attributes

### Vulnerabilities
- Sandboxing should be enabled
- Content Security Policies should be restrictive
- webSecurity should be enabled

### Code smells
- ARIA properties in DOM elements should have valid values
- "tabIndex" values should be 0 or -1
- Non-interactive elements shouldn't have event handlers
