---
paths:
  - "**/*.java"
---
<!-- Installed by good-practices-rules. Edit the package, not this copy: the next install replaces it. -->
# Java good practices: style, modern syntax and naming

## Modern Java (needs Java 17+; check the repo's version before applying)
- Use `stream.toList()` instead of `collect(Collectors.toList())`. **Careful: it returns an unmodifiable list.** If you add or remove elements afterwards, keep `Collectors.toList()` instead.
- Use pattern matching: `if (o instanceof Customer c)` instead of `instanceof` plus a cast.
- Use `record` for immutable data. Check the framework supports it first; JPA can't use records as entities.
- Use text blocks `"""` instead of concatenating multi-line strings.
- Merge labels into one `case A, B ->`. Use `switch` instead of an `if/else` chain on the same variable.
- Use the diamond operator `new ArrayList<>()`.
- Don't use `var`, `record`, `yield` or `sealed` as identifiers.
- Use a stream when a loop only filters, maps or collects. Use `coll.forEach(...)` instead of `stream().forEach(...)`. Simplify chains like `!stream.anyMatch` to `noneMatch`.

## Lambdas
- Use a method reference when it works: `map(Customer::getId)`.
- For a single-statement lambda, drop the braces and `return`. With one parameter of inferred type, drop the parentheses: `x -> ...`.
- Turn an anonymous class with a single method into a lambda.

## Redundant code
- Return the expression directly instead of storing it in a variable and returning that.
- Write `return cond;` instead of `if (cond) return true; else return false;`. Don't compare against `true`/`false` literals.
- Don't write conditions that always give the same result, or repeat a condition in a later `else if`.
- If a ternary only changes one argument, move the ternary inside the call.
- Remove redundant casts, needless boxing, `toString()` on a `String`, extra parentheses and stray `;`.
- Use `String.replace` when you don't need a regex. Use plain concatenation instead of a `String.format` with no formatting.
- Don't create an explicit array to pass varargs.
- Remove unused `private` fields and methods. A field used only inside one method becomes a local variable.
- A loop has at most one `break` or `continue`.
- Don't reuse a parameter, caught exception or for-each variable as a scratch variable.
- No empty classes and no `FIXME` comments. Delete deprecated code once it's unused.

## Naming and declarations
- Packages are lowercase: `^[a-z_]+(\.[a-z_][a-z0-9_]*)*$`. Never use the default package.
- Constants are `UPPER_SNAKE_CASE`. Non-final `static` fields follow the field convention. Type parameters are one capital letter, like `T` or `K`.
- No field is named like its class. No method is named like a field, even with different casing.
- Locals don't shadow class fields. A child class field doesn't shadow a parent field.
- One variable per declaration line. Order modifiers per the spec: `public static final`.
- Declare with the interface type: `List` and `Map`, not `ArrayList` or `HashMap`.
- Never expose mutable `public` fields. `public static` fields are `final`, and a `final` field initialized at declaration is also `static`.
- Enums don't expose public setters.
- Put `@Override` on every overriding or implementing method.
- Access `static` members through the class, not an instance.

## Annotations and comments
- Repeat the annotation instead of wrapping it: several `@ApiResponse` rather than `@ApiResponses({...})`.
- Single-line comments start with exactly `//`, not `///`.
- Escape tabs and control characters in literals: `\t`.

## Regex
- Add `UNICODE_CASE` when you use `CASE_INSENSITIVE` on text with accents or ñ.
- Remove superfluous quantifiers like `{1}`. Avoid repetitions that overflow the stack on large inputs.
- A `private` method used only by an inner class moves into that class.

## Collections
- Use `isEmpty()` to check whether a collection or `String` is empty, not `size() == 0` or `length() == 0`.
- Iterate a collection with the type it holds, not a supertype followed by a cast.
