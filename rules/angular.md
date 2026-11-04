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
- Use `ChangeDetectionStrategy.OnPush` for new components, and signals (`signal`, `computed`, `input()`) for component state.
- Unsubscribe from every observable you subscribe to manually: `takeUntilDestroyed()`, or use the `async` pipe / `toSignal` instead of `subscribe`.
- Don't nest `subscribe` calls; compose with `switchMap`, `mergeMap` or `concatMap`.
- Use standalone components and the built-in control flow (`@if`, `@for` with `track`) in new code.
- Always `track` by a stable id in `@for` (or `trackBy` in `*ngFor`).
- Don't call functions in templates for values that can be signals, `computed` or pipes.
- Lazy-load feature routes with `loadComponent`/`loadChildren`.
- Use typed reactive forms (`FormControl<string>`), not `UntypedFormControl`.
- Use functional guards and interceptors; attach tokens in an interceptor, not in each service.
- Don't touch the DOM with `document` or `ElementRef.nativeElement`; use bindings or `Renderer2`.
- `bypassSecurityTrustHtml`/`Url`/`ResourceUrl` disable Angular's sanitization. Use them only on content you control, never on user input, and leave a comment explaining why. The comment is what justifies it in review.
