---
paths:
  - "**/*.rs"
  - "**/Cargo.toml"
---
<!-- Installed by good-practices-rules. Edit the package, not this copy: the next install replaces it. -->
# Rust good practices: correctness, unsafe and panics

## Top rules
- **Don't `unwrap()` a value that can be `None`/`Err`**; handle it with `?`, `match` or `if let`.
- **Don't access an array element with an index that will panic**; check the bounds or use `.get(i)`.
- **Keep `transmute` out of normal code**: never transmute collections, null pointers or to incompatible types; use safe conversions (`from_raw_parts` with checked sizes, `ptr::null()`).
- **Await every future** in async code; an un-awaited future does nothing.
- Don't acquire a lock and drop the guard immediately (`let _ = m.lock()`); bind it to a named variable for the critical section.

## Correctness
- Don't call `MaybeUninit::uninit().assume_init()`, `mem::uninitialized()` or `mem::zeroed()` to fill values, and don't `set_len` on an uninitialized `Vec`.
- Process I/O buffers entirely: use `read_exact`/`write_all` instead of ignoring the byte count from `read`/`write`.
- Use `checked_add`/`saturating_sub` where arithmetic can overflow or underflow.
- If you derive `Hash` or `Ord`, derive `PartialEq`/`PartialOrd` too; keep `Borrow` and `Hash` consistent.
- Make `OpenOptions` consistent (no `truncate` without `write`), and set Unix permissions with octal literals (`0o644`).
- Mark functions that dereference raw pointer arguments `unsafe`.
- Swap with `std::mem::swap`, not two assignments.
- Don't use wildcard imports or invisible Unicode characters.
- Log through a logging crate, not `println!`/`eprintln!`.


## More rules to follow
Write code that already complies with each one; the ones marked **critical** break builds or production.

### Bugs
- Variables should not be self-assigned
- Loops with at most one iteration should be refactored
- Identical expressions should not be used on both sides of a binary operator
- Related "if/else if" statements should not have the same condition
- "while" loop counters should not have floating type
- Null pointers should not be passed to functions expecting non-null arguments **(critical)**
- Getters should access the expected fields **(critical)**
- Regular expressions should be syntactically valid **(critical)**
- Accessing an array element should not trigger a panic **(critical)**
- Clamping values with `cmp::min` and `cmp::max` should use correct ranges
- Avoid resizing a vector to zero using `vec.resize(0, value)`
- Await should be used for awaitable returns in async blocks and functions **(critical)**
- Avoid transmutes that can never be correct **(critical)**
- Immutable variables should not be used in while loop conditions **(critical)**
- Manual PartialOrd implementation should be avoided when Ord is derived **(critical)**
- Lint attributes should not be used on crate imports
- I/O buffers should be processed entirely **(critical)**
- Collections should not be transmuted to different types **(critical)**
- Closures of `type Fn(...) -> Ord` should not return the unit type **(critical)**
- Unit values should not be hashed **(critical)**
- Unit values should not be compared
- Avoid manual PartialEq implementation with a derived Hash **(critical)**
- `MaybeUninit::uninit().assume_init()` should not be used
- C-like enums should not have unportable variants **(critical)**
- Null pointers should not be transmuted **(critical)**
- Case mismatches in pattern arms of match expressions should be avoided **(critical)**
- Null function pointers should not be created through `transmute`
- `splitn` should not be used with a limit of 0 or 1
- `size_of::<T>` should not be used to count elements of type `T`
- Reversed ranges and slices should not be empty
- Raw pointers should not be casted to slices with differently sized elements **(critical)**
- Redundant comparisons should be removed
- Variables should be swapped using `std::mem::swap` **(critical)**
- Incompatible bit masks should not be used in comparisons **(critical)**
- Comparisons with overlapping ranges that are always false should not be made **(critical)**
- Formatting trait implementations should not be recursive
- Lines read from the standard input should be trimmed
- `unwrap()` should only be used when there is a value to unwrap **(critical)**
- Calls to `std::mem::transmute` should not be evaluated eagerly **(critical)**
- `checked_add` and `overflowing_add` should be used to prevent overflows
- `env!` should be preferred over `option_env!`
- Functions expecting raw pointer arguments should be marked as unsafe
- File open options should be consistent **(critical)**
- Unix file permissions should be set with octal values
- The `#[inline]` attribute should not be used on trait methods without implementation **(critical)**
- Synchronization locks should not be dropped immediately after acquisition **(critical)**
- Remainder operations with `1` or `-1` should be avoided **(critical)**
- Functions should not return mutable references from immutable parameters
- Numeric literal suffixes should not be mistyped
- The return value of `next` should not be looped over
- `skip(0)` should not be used on iterators
- Calls to `step_by` that always panic should not be made **(critical)**
- Inherent shadowing definitions of to_string should be avoided
- `set_len` should not be called on uninitialized vectors **(critical)**
- Serde `visit_str` method should be implemented when `visit_string` is implemented
- Conflicting `Borrow` and `Hash` implementations should be avoided **(critical)**
- `mem::uninitialized` and `mem::zeroed()` should not be used to replace values
- `saturating_sub` should be used to avoid subtraction underflow
- Infinite iterators should be finished off with a terminating operation
- Non-empty statements should change control flow or have at least one side-effect

### Code smells
- Functions should not have too many parameters
- Erasing mathematical operations should not be performed
- Unnecessary mathematical comparisons should not be made **(critical)**
- Wildcard imports should not be used **(critical)**
- Unnecessary bit operations should not be performed **(critical)**
- Invisible Unicode characters should not be used **(critical)**
- Boolean expressions should not be gratuitous
- Array elements should be separated by commas
- Cognitive Complexity of functions should not be too high **(critical)**
- "std::ptr::null" should be used to denote the null pointer **(critical)**
- Mathematical constants should not be hardcoded
- Inline vector literals should be preferred to chains of insertions
- Pointer arithmetic should not be performed on zero-sized types
