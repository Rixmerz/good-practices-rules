---
paths:
  - "**/*.{ts,tsx,js,jsx,mjs,cjs}"
---
<!-- Installed by good-practices-rules. Edit the package, not this copy: the next install replaces it. -->
# TypeScript/JavaScript good practices: types, errors, async and security

## Top rules
- **Mark every field assigned only in the constructor `readonly`**. That includes injected services: `constructor(private readonly http: HttpClient)`.
- **Use optional chaining**: `a?.b?.c` instead of `a && a.b && a.b.c`.
- **Use `Number.parseInt`, `Number.isNaN` and `Number.parseFloat`**, not the global versions.
- **Throw only `Error` objects**, never strings or literals: `throw new Error('msg')`. Reject promises with an `Error` as well. Use `TypeError` after a failed type check, and always give errors a message.
- **Import Node built-ins with the `node:` prefix**: `import fs from 'node:fs'`.

## Types
- Don't put redundant members in unions: in `'a' | string`, the `'a'` is redundant. Give a repeated union a `type` alias.
- Remove unnecessary casts and `!` assertions. Use `string` and `number`, never the `String`/`Number` wrappers.
- For an optional property use `?` **or** `| undefined`, not both. Don't pass an explicit `undefined` to an optional parameter.
- Give an object that gets stringified a `toString()`; otherwise it prints `[object Object]`.
- A function always returns the same type. Avoid functions that always return the same value.
- Don't use `===` between different types; it's always `false`.
- Compare with `undefined` directly instead of `typeof x === 'undefined'`.

## Async and promises
- **Never leave a promise floating**: every call that returns a promise is `await`ed, `return`ed, or ends in `.catch(...)` (or `.then(ok, onError)`). Combine several with `Promise.all`/`allSettled` and handle that, never a bare `array.map(asyncFn)`. The trap is calling an async function from a callback-style API that ignores the result, such as Express middleware, event listeners or timers: `route(async () => { … })(req, res, next)` is flagged. Write `route(async () => { … })(req, res, next).catch(next);` so a rejection reaches Express's error handler. Don't silence it with `void`; that only hides the problem.
- Only `await` promises.
- In an `async` function, return the value or throw; don't use `Promise.resolve`/`Promise.reject`.
- A promise inside `try` needs `await` for the `catch` to see it. A `new Promise` executor must not be `async`.
- Keep async work out of constructors; use an `init()` method or `ngOnInit`.
- Prefer top-level `await` over `.then()` chains in ESM modules. Don't wrap a single item in `Promise.all([x])`.
- Don't use the `void` operator; handle an intentionally unawaited promise with `.catch(...)`.

## Errors
- Don't swallow exceptions: an empty `catch` needs handling or a comment explaining why.
- Name the `catch` parameter `error` consistently.

## Control flow
- Don't assign inside an expression, as in `if ((x = f()))`. Don't use the comma operator.
- An `else` whose only statement is an `if` becomes `else if`. When there's an `else`, don't negate the condition, and don't negate operands in equality checks like `!a === b`.
- Declare variables in a `case` only inside `{ }`. Replace a `switch` with too many cases with a lookup map.
- Use braces for multi-line blocks.
- Don't nest functions more than 5 levels deep. Move inner functions to the highest possible scope.
- Don't pass boolean selector parameters that pick behavior; write two methods.
- Don't write constant expressions like `x ?? 'a' === 'b'`, identical operands like `a === a`, statements with no effect, or objects that are created and discarded.
- Don't use optional chaining where a short-circuited `undefined` would throw, e.g. `new (a?.B)()` or `(a?.b).c`.

## Variables and parameters
- Use `const`/`let`, never `var`, and always declare the variable. Don't initialize to `undefined`.
- Default-valued parameters go last. Use a default parameter instead of `x = x || 'y'`. Don't use an object literal as a default.
- Don't reassign a parameter before using its initial value. Don't assign `this` to a variable; use an arrow function.
- No duplicate members in a class or object literal. Don't pass more arguments than the function accepts.
- Don't use the result of a `void` function.

## Collections
- Pass a compare function to `sort()`/`toSorted()`, e.g. `(a, b) => a.localeCompare(b)`. Don't call `sort()` inside an expression; use `toSorted()`.
- Use `forEach` when you ignore `map`'s result. Every `map`/`filter`/`reduce` callback returns a value, and `reduce` gets an initial value.
- Don't read or iterate collections that are always empty, and don't overwrite an element unconditionally.

## Security
- Never feed user data into NoSQL queries, mass assignment to the database, HTML responses, regexes, CORS headers or client-side request URLs.
- Open external links with `rel="noopener noreferrer"`.
- Hide the framework version, e.g. `app.disable('x-powered-by')` in Express.
- Limit request body size on endpoints. Don't disable CSP fetch directives.
- Don't create temp files in shared `/tmp` paths; use `fs.mkdtemp`.
- In code run by an agent or LLM, validate file paths that come from arguments.
- Never hard-code secrets or passwords.
- When disabling an ESLint rule, name it: `// eslint-disable-next-line no-console`.

## More rules to follow
Write code that already complies with each one; the ones marked **critical** break builds or production.

### Bugs
- Loops should not be infinite **(critical)**
- Jump statements should not occur in "finally" blocks
- "in" should not be used with primitive types
- Parentheses should be used when negating "in" and "instanceof" operations
- "new" operator should not be used with Symbol and BigInt
- "super()" should be invoked appropriately
- "typeof" expressions should only be compared to valid values
- Getters and setters should access the expected fields
- Type intersections should use meaningful types
- Table cells should reference their headers
- Regular expressions should be syntactically valid
- Literals should not be used as functions
- Getters and setters should not recursively access the same property
- Async test assertions should be awaited or returned
- Function references should not be passed directly to iterator methods
- Bitwise operators should not be used in boolean contexts
- Function argument names should be unique
- Values should not be uselessly incremented
- A "for" loop update clause should move the counter in the right direction
- Setters should not return values
- Empty character classes should not be used
- "NaN" should not be used in comparisons
- Non-existent operators '=+', '=-' and '=!' should not be used
- "new" should only be used with functions and classes
- "const" variables should not be reassigned
- Generators should explicitly "yield" a value
- Comma and logical OR operators should not be used in switch cases
- Destructuring patterns should not be empty
- Collection size and array length comparisons should make sense
- Errors should not be created without being thrown
- Constructors should not be declared inside interfaces
- Disabling Mocha timeouts should be explicit
- Alternation in regular expressions should not contain empty alternatives
- Regular expressions should not contain control characters
- Replacement strings should reference existing regular expression groups
- Regular expressions with the global flag should be used with caution
- Exclusive tests should not be committed to version control
- React "render" functions should return a value
- Comments inside JSX expressions should be enclosed in curly braces
- React components should not render non-boolean condition values
- React's useState hook should not be used directly in the render function or body of a component
- React state setter function should not be called with its matching state variable
- Numbers should not lose precision
- "setState" should use a callback when referencing the previous state
- "this" should not be used in functional components
- "children" and "dangerouslySetInnerHTML" should not be used together
- Objects should not have a "then" property
- Tests should not be registered asynchronously in suite callbacks
- Vue component props should not be mutated directly
- UI test debug commands should not be committed to version control
- HTML elements with v-for should have a :key attribute
- Vue v-for directives should be valid
- Side effects should not be performed inside "waitFor" callbacks
- Side effects should not be performed inside Vue computed properties
- Promises should not be passed to "fireEvent"
- Parameterized tests should not have empty datasets
- "<button>" elements should have a valid "type" attribute, explicitly set when associated with a "<form>"
- Vue refs should not be used as operands
- Asynchronous actions should not be used in Vue computed properties
- Promises returned by Testing Library async queries should be handled
- Synchronous exception assertions should receive synchronous callbacks
- Vue component prop types should be constructors
- Lodash and Underscore.js debounced or throttled functions should not be recreated on every React render
- Promises returned by Testing Library async events should be handled
- Vue component field names should not be duplicated
- Testing Library disappearance waits should use non-throwing queries
- Reactive state should not be unconditionally mutated inside Vue's "updated" lifecycle hook

### Vulnerabilities
- OS commands should not be vulnerable to command injection attacks **(critical)**
- LDAP queries should not be vulnerable to injection attacks **(critical)**
- XPath expressions should not be vulnerable to injection attacks **(critical)**
- XML parsers should not be vulnerable to XXE attacks **(critical)**
- HTTP request redirections should not be open to forging attacks **(critical)**
- Dynamic code execution should not be vulnerable to injection attacks **(critical)**
- Imports should not be vulnerable to injection attacks **(critical)**
- Server-side templates should not be vulnerable to injection attacks **(critical)**
- DOM updates should not lead to cross-site scripting (XSS) attacks **(critical)**
- Extracting archives should not lead to zip slip vulnerabilities **(critical)**
- DOM updates should not lead to open redirect vulnerabilities **(critical)**
- Objects should not be vulnerable to Prototype Pollution attacks **(critical)**
- S3 buckets should not grant access to all users or authenticated users **(critical)**
- AWS resource-based policies should not grant public access **(critical)**
- Policies should not grant all privileges **(critical)**
- Public network access to cloud resources should be disabled **(critical)**
- AWS API Gateway should require authentication **(critical)**
- Wallet phrases should not be hard-coded **(critical)**
- Dynamic code execution should not use user-controlled data
- File uploads should be restricted
- Origins should be verified during cross-origin communications
- Cryptographic keys should be robust
- Cipher algorithms should be robust
- JWT should be signed and verified with strong cipher algorithms
- A new session should be created during user authentication
- Amazon S3 bucket public access should be fully blocked
- AWS IAM policies should limit the scope of permissions given
- Confidential data should not be sent to a remote LLM
- Confidential data should not be logged
- Confidential data should not be sent in HTTP GET parameters
- Confidential data should not be indexed into AI pipelines
- File permissions should not be set to world-accessible values
- EBS volumes should be encrypted
- Applications should not create session cookies from untrusted input
- Amazon RDS resources should be encrypted at rest
- OpenSearch domains should have encryption at rest enabled
- SageMaker notebook instances should be encrypted at rest
- SQS queues should be encrypted
- Amazon EFS file systems should be encrypted
- System command arguments constructed from user input should be validated
- Environment variables should not be defined from untrusted input
- Accessing files should not lead to filesystem oracle attacks
- Connection strings should not be vulnerable to injection attacks
- Templates should not be constructed dynamically
- Agentic workflows should not be vulnerable to command injection attacks
- Agentic workflows should not be vulnerable to SQL injection attacks
- Agentic workflows should not be vulnerable to server-side request forgery
- Agentic workflows should not be vulnerable to code injection attacks
- Agentic workflows should not be vulnerable to argument injection attacks
- Agentic workflows should not be vulnerable to connection string injection

### Security hotspots (review each one)
- Regular expressions should not cause catastrophic backtracking

### Code smells
- "switch" statements should not contain non-case labels **(critical)**
- Octal values should not be used **(critical)**
- Future reserved words should not be used as identifiers **(critical)**
- Assertions should be complete **(critical)**
- Angular lifecycle methods should be used in the correct context **(critical)**
- Components and directives should not use the "inputs" metadata property **(critical)**
- The "outputs" metadata property should not be used in Angular components and directives **(critical)**
- "for" loop increment clauses should modify the loops' counters
- Constructor names should start with an upper case letter
- Conditionals should start on new lines
- "default" clauses should be last
- Union and intersection types should not include duplicated constituents
- Tests should not execute any code after "done()" is called
- Imports should not use absolute paths
- Mutable variables should not be exported
- "trimLeft()" and "trimRight()" should be replaced with "trimStart()" and "trimEnd()"
- Equality operators should not be used in "for" loop termination conditions
- Shorthand promises should be used
- Tests should be stable
- Import variables should not be reassigned
- Redundant type aliases should not be used
- Bitwise operators should not be used to truncate numbers
