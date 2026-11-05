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

Each rule names its angular-eslint rule; the first four are in the `recommended` config.

- **Implement the interface for every lifecycle hook you use**: `class X implements OnInit, OnDestroy` if it has `ngOnInit` and `ngOnDestroy` (`use-lifecycle-interface`).
- **Delete empty lifecycle hooks** like `ngOnInit() {}`; the CLI generates them and they add nothing (`no-empty-lifecycle-method`).
- **Don't name an `@Output`/`output()` `on` or prefix it with `on`**: use `selected`, not `onSelected`. Don't name it after a native DOM event like `click`, `change` or `submit` either (`no-output-on-prefix`, `no-output-native`).
- **Don't alias inputs** with `@Input('otherName')` or `input({ alias })` (`no-input-rename`).
- Mark injected services and never-reassigned fields `readonly`. With `inject()`, write `private readonly http = inject(HttpClient)` (`@typescript-eslint/prefer-readonly`).
- Use `ChangeDetectionStrategy.OnPush` for new components, and signals (`signal`, `computed`, `input()`) for component state (`prefer-on-push-component-change-detection`, `prefer-signals`).
- Use standalone components and the built-in control flow (`@if`, `@for` with `track`) in new code (`prefer-standalone`, `template/prefer-control-flow`).
- Give `*ngFor` a `trackBy` function (`template/use-track-by-function`).
- Don't call functions or methods in template bindings; use signals, `computed` or pipes (`template/no-call-expression`).
