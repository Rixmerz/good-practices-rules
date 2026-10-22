---
paths:
  - "**/*.component.ts"
  - "**/*.directive.ts"
  - "**/*.pipe.ts"
  - "**/*.service.ts"
  - "**/*.guard.ts"
  - "**/*.resolver.ts"
---
<!-- Installed by good-practices-rules. Edit the package, not this copy: the next install replaces it. -->
# Good practices: Angular

These are **critical**: break them and the component misbehaves or the build checks fail.

- **Implement the interface for every lifecycle hook you use**: `class X implements OnInit, OnDestroy` if it has `ngOnInit` and `ngOnDestroy`.
- **Delete empty lifecycle hooks** like `ngOnInit() {}`; the CLI generates them and they add nothing.
- **Don't name an `@Output`/`output()` `on` or prefix it with `on`**: use `selected`, not `onSelected`. Don't name it after a native DOM event like `click`, `change` or `submit` either.
- **Don't alias inputs** with `@Input('otherName')` or `input({ alias })`.
- Mark injected services and never-reassigned fields `readonly`. With `inject()`, write `private readonly http = inject(HttpClient)`.
- `bypassSecurityTrustHtml`/`Url`/`ResourceUrl` disable Angular's sanitization. Use them only on content you control, never on user input, and leave a comment explaining why. The comment is what justifies it in review.
