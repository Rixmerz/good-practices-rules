---
paths:
  - "**/*.swift"
---
<!-- Installed by good-practices-rules. Edit the package, not this copy: the next install replaces it. -->
# Swift good practices, based on SwiftLint

## Top rules
- **Never force-try (`try!`) or force-cast (`as!`)**; use `do/catch`, `try?` or `as?` and handle the failure (`force_try`, `force_cast`).
- **Never force-unwrap (`!`)**; use `guard let`, `if let` or `??` (`force_unwrapping`).
- **Don't use implicitly unwrapped optionals (`String!`)** where a real optional or a non-optional works (`implicitly_unwrapped_optional`).
- **Capture references weakly** (`[weak self]`), not `unowned`, in closures (`unowned_variable_capture`).
- **Handle errors thrown inside a `Task`** in the task, or `try await` its value (`unhandled_throwing_task`).
- Make delegate protocols class-only and hold delegates `weak` (`class_delegate_protocol`, `weak_delegate`).
- Keep SwiftUI `@State` properties `private` (`private_swiftui_state`).
- Give images that carry meaning an accessibility label, and tappable views the `.isButton` or `.isLink` trait (`accessibility_label_for_image`, `accessibility_trait_for_button`).
- `UpperCamelCase` types, `lowerCamelCase` identifiers (`type_name`, `identifier_name`).

## More rules to follow
Write code that already complies with each one; the ones marked **critical** break builds or production.

### Lint
- force_cast: Force casts should be avoided **(critical)**
- force_try: Force tries should be avoided **(critical)**
- unhandled_throwing_task: Errors thrown inside this task are not handled, which may be unexpected **(critical)**
- duplicate_enum_cases: Enum shouldn't contain multiple cases with the same name **(critical)**
- duplicate_conditions: Duplicate sets of conditions in the same branch instruction should be avoided **(critical)**
- dynamic_inline: Avoid using 'dynamic' and '@inline(__always)' together **(critical)**
- duplicated_key_in_dictionary_literal: Dictionary literals with duplicated keys will crash at runtime
- identical_operands: Comparing two identical operands is likely a mistake
- discarded_notification_center_observer: When registering for a notification using a block, the opaque observer that is returned should be stored so it can be removed later
- notification_center_detachment: An object should only remove itself as an observer in `deinit`
- ns_number_init_as_function_reference: Passing `NSNumber.init` or `NSDecimalNumber.init` as a function reference is dangerous
- self_in_property_initialization: `self` refers to the unapplied `NSObject.self()` method, which is likely not expected
- overridden_super_call: Some overridden methods should always call super
- prohibited_super_call: Some methods should not call super
- private_unit_test: Unit tests marked private are silently skipped
- lower_acl_than_parent: Ensure declarations have a lower access control level than their enclosing parent
- async_without_await: Declaration should not be async if it doesn't use await
- unneeded_escaping: The `@escaping` attribute should only be used when the closure actually escapes
- unneeded_throws_rethrows: Non-throwing functions/properties/closures should not be marked as `throws` or `rethrows`
- unused_closure_parameter: Unused parameter in a closure should be replaced with _
- unused_setter_value: Setter value is not used
- unused_declaration: Declarations should be referenced at least once within all files linted
- unused_import: All imported modules should be required to make the file compile
- variable_shadowing: Do not shadow variables declared in outer scopes
- blanket_disable_command: `swiftlint:disable` commands should use `next`, `this` or `previous` to disable rules for a single line, or `swiftlint:enable` to re-enable them
- invisible_character: Disallows invisible characters like zero-width space (U+200B)
- todo: TODOs and FIXMEs should be resolved

### Idiomatic
- force_unwrapping: Force unwrapping should be avoided
- implicitly_unwrapped_optional: Implicitly unwrapped optionals should be avoided when possible
- fatal_error_message: A fatalError call should have a message
- discouraged_optional_boolean: Prefer non-optional booleans over optional booleans
- discouraged_optional_collection: Prefer empty collection over optional collection
- fallthrough: Fallthrough should be avoided
- legacy_hashing: Prefer using the `hash(into:)` function instead of overriding `hashValue`
- legacy_random: Prefer using `type.random(in:)` over legacy functions
- private_over_fileprivate: Prefer `private` over `fileprivate` declarations
- redundant_nil_coalescing: Coalescing operator with right-hand side nil is redundant
- return_value_from_void_function: Returning values from Void functions should be avoided
- syntactic_sugar: Shorthand syntactic sugar should be used, i.e. [Int] instead of Array<Int>
- unavailable_function: Unimplemented functions should be marked as unavailable
- duplicate_imports: Imports should be unique
- no_magic_numbers: Magic numbers should be replaced by named constants

### Metrics
- cyclomatic_complexity: Complexity of function bodies should be limited
- function_body_length: Function bodies should not span too many lines
- function_parameter_count: Number of function parameters should be low
- closure_body_length: Closure bodies should not span too many lines
- type_body_length: Type bodies should not span too many lines
- file_length: Files should not span too many lines
- large_tuple: Tuples shouldn't have too many members. Create a custom type instead
- nesting: Types should be nested at most 1 level deep, and functions should be nested at most 2 levels deep

### Performance
- empty_count: Prefer checking `isEmpty` over comparing `count` to zero
- first_where: Prefer using `.first(where:)` over `.filter { }.first` in collections
- contains_over_first_not_nil: Prefer `contains` over `first(where:) != nil` and `firstIndex(where:) != nil`
- sorted_first_last: Prefer using `min()` or `max()` over `sorted().first` or `sorted().last`
- reduce_into: Prefer `reduce(into:_:)` over `reduce(_:_:)` for copy-on-write types
