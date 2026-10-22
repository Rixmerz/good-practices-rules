---
paths:
  - "**/*.java"
---
<!-- Installed by good-practices-rules. Edit the package, not this copy: the next install replaces it. -->
# Java good practices: design, Spring, errors and security

These are the Java rules that fail most often in real projects; apply them as you write.

## Top five
- **Use constructor injection**, never `@Autowired` on a field. Make the fields `private final` and use `@RequiredArgsConstructor` if the repo already uses Lombok.
- **Never throw generic `Exception`, `RuntimeException` or `Throwable`**. Throw a specific domain or library exception, such as `IllegalArgumentException` or `BookingNotFoundException`.
- **Don't evaluate log arguments eagerly**: write `log.debug("id {}", id)`, not `log.debug("id " + id)` or `String.format`. Wrap an expensive argument in `if (log.isDebugEnabled())`.
- **Use placeholders in format strings**: `{}` for SLF4J and `%s` for `String.format`, with exactly as many arguments as placeholders. Never concatenate.
- **Don't return wildcard types** like `List<? extends T>` or `ResponseEntity<?>`; return the concrete type.

## Spring
- Use the specialized stereotype: `@Service`, `@Repository` or `@Controller` instead of `@Component`. Don't add `@Component` to a `@Configuration` class.
- Never call `@Async`, `@Transactional` or `@Cacheable` methods through `this.`, because the proxy is bypassed. Call them through another injected bean.
- Methods with those annotations must be `public`. Don't call a method with an incompatible `@Transactional` setting from the same class.
- Don't inject prototype- or request-scoped beans into a singleton; use `ObjectProvider`.
- In `@Configuration`, inject dependencies as parameters of the `@Bean` method that uses them, not as fields. Don't put `@Qualifier` on `@Bean` methods.
- Use `@Value` only for `${property}` or SpEL; use a plain initializer for a fixed value.
- Return the `HttpStatus` that matches the operation: 201 on create, 404 when not found, and so on.
- Never bind JPA entities as `@RequestBody`; use a DTO or `record` to prevent mass assignment.

## Exceptions
- Don't declare `throws` for exceptions the method can't throw.
- **Either** log the exception **or** rethrow it with context, never both.
- Don't nest `try/catch`; extract the inner block into a method.
- In `catch (InterruptedException e)`, call `Thread.currentThread().interrupt()` or rethrow.
- Make exception class fields `final`. A class named `*Exception` must extend `Exception`.
- Use try-with-resources for every `Closeable`, with no redundant `close()` inside.

## Logging
- Never use `System.out` or `printStackTrace`; use the logger.
- Name the logger after its enclosing class: `LoggerFactory.getLogger(MyClass.class)`.

## Dates and time
- Use `java.time` (`LocalDate`, `Instant`, `ZonedDateTime`), never `Date`, `Calendar` or `SimpleDateFormat`.
- Pass a `ZoneId` or `Clock` to every `.now()`, e.g. `LocalDateTime.now(ZoneId.of("America/Santiago"))`; otherwise the result depends on the server's time zone.
- Compute durations with zone-aware types (`ZonedDateTime`, `Instant`), not `LocalDateTime`.
- Use `Month.MARCH` and `DayOfWeek.MONDAY`, not numbers.

## Nulls and Optional
- Check for null before dereferencing. Return empty collections, never `null`. A `Boolean` method never returns `null`.
- Don't call `Optional.get()` without `isPresent()`; prefer `orElseThrow()` or `map`. Never assign `null` to an `Optional`.
- Annotate parameters that accept null with `@Nullable`.

## Concurrency and state
- Instance methods don't write `static` fields, and neither do constructors.
- Don't make non-thread-safe fields such as `SimpleDateFormat` `static`. `volatile` doesn't make an object thread-safe.
- Synchronize only on `private final` fields. Clean up `ThreadLocal` with `remove()`.
- Overrides of `synchronized` methods must also be `synchronized`.
- Create one `Random` and reuse it.

## Numbers and comparisons
- Compare `String` and wrappers (`Integer`, `Long`) with `equals()`, never `==`.
- Compare `BigDecimal` with `compareTo()`. Create it with `BigDecimal.valueOf(double)` or from a `String`, not `new BigDecimal(double)`.
- Cast before the operation to avoid overflow: `(long) a * b`. Make narrowing casts explicit.
- Don't use a boxed `Boolean` directly in a condition.

## Serialization and records
- Fields of a `Serializable` class must be serializable or `transient`.
- A `record` with array fields must override `equals`, `hashCode` and `toString`.

## Security
- Never build outgoing URLs, JSON or SQL from user data. For SQL, use a `PreparedStatement` with parameters.
- Never disable TLS certificate or hostname verification. Use TLS 1.2 or later.
- Don't disable CSRF in Spring Security without a comment justifying it.
- Keep debug modes such as `@EnableWebSecurity(debug = true)` off in production.
- Don't use reflection to break encapsulation with `setAccessible(true)`.
- Read URIs from configuration; don't hard-code them.

## SQL and performance
- Never `SELECT *`; list the columns.
- Don't create a `PreparedStatement` or compile a `Pattern` inside a loop. Set parameters that don't change outside the loop.
- Concatenate in loops with `StringBuilder`, not `+`. Don't concatenate inside `append`.
- When you need both key and value, iterate `entrySet()`.
- Don't reuse a stream that has already been consumed.

## Design
- A utility class (only `static` methods) gets a `private` constructor.
- Split "Brain Methods" and classes that depend on too many other classes. Inheritance depth is 5 levels at most.
- Give every `@Deprecated` a reason (`since`, `forRemoval`) plus a `@deprecated` Javadoc tag. Don't use code marked `forRemoval`.
- Every `switch` has a `default`, and every `case` ends with `break`, `return` or `throw`.
- Don't hand-roll Singletons; let Spring manage the lifecycle.

## More rules to follow
Write code that already complies with each one; the ones marked **critical** break builds or production.

### Bugs
- Double-checked locking should not be used **(critical)**
- Loops should not be infinite **(critical)**
- Recursion should not be infinite **(critical)**
- Do not mix field and getter persistence annotations **(critical)**
- Methods "wait(...)", "notify()" and "notifyAll()" should not be called on Thread instances **(critical)**
- Printf-style format strings should not lead to unexpected behavior at runtime **(critical)**
- "wait(...)" should be used instead of "Thread.sleep(...)" when a lock is held **(critical)**
- Files opened in append mode should not be used with "ObjectOutputStream" **(critical)**
- "PreparedStatement" and "ResultSet" methods should be called with valid indices **(critical)**
- "wait" should not be called when multiple locks are held **(critical)**
- "@Controller" classes that use "@SessionAttributes" must call "setComplete" on their "SessionStatus" objects **(critical)**
- "@SpringBootApplication" and "@ComponentScan" should not be used in the default package **(critical)**
- Incompatible bit masks should not be used in comparisons **(critical)**
- Conversions between local and timezone-aware types should use explicit timezone handling **(critical)**
- Jump statements should not occur in "finally" blocks
- The signature of "finalize()" should match that of "Object.finalize()"
- "ScheduledThreadPoolExecutor" should not have 0 core threads
- "runFinalizersOnExit" should not be called
- Locks should be released on all paths
- Classes should not access their own subclasses during class initialization
- JEE applications should not use threads or synchronization
- Zero should not be a possible denominator
- Getters and setters should access the expected fields
- Assertion methods should not be used within the try block of a try-catch catching an Error
- Only one method invocation is expected when testing checked exceptions
- JUnit5 inner test classes should be annotated with @Nested
- Regular expressions should be syntactically valid
- Regex patterns following a possessive quantifier should not always fail
- Regex boundaries should not be used in a way that can never be matched
- Back references in regular expressions should only refer to capturing groups that are matched before the reference
- Regex lookahead assertions should not be contradictory
- Map "computeIfAbsent()" and "computeIfPresent()" should not be used to add "null" values
- Members ignored during record serialization should not be used
- Unsupported methods should not be called on some collection implementations
- Calls to methods should not trigger an exception
- Nullable injected fields and parameters should provide a default value
- Use of the "@Async" annotation on methods declared within a "@Configuration" class in Spring Boot
- "@Autowired" should only be used on a single constructor
- SpEL expression should have a valid syntax
- Virtual threads should be used for tasks that include heavy blocking operations
- "@Scheduled" annotation should only be applied to no-arg methods
- @EventListener methods should have one parameter at most
- Subclass fields should be initialized before calls to "super()" or "this()"
- Value-based classes should be compared using their value
- "CacheKeyGenerator" implementations should be instantiable by the framework
- Methods annotated with "@Startup" should be non-static, non-producer, and parameter-free
- Custom CredentialsProvider implementations should be annotated with @Unremovable
- JPA entity classes and methods should not be final
- Bean Validation constraints should not be placed on static fields
- The return value of "EntityManager.merge()" should be used
- Stateless session beans should not store mutable state in instance variables
- Mockito stubbing chains should be complete
- "@Configuration" classes should not be final
- Stream read results should be checked for -1 before casting
- Archive entries should not be empty
- Bitwise AND operations with zero should be corrected
- Integer values should not be cast to long for use as timestamps
- TestNG data provider methods should return valid types
- Data provider names should be unique within a class
- The "Object.finalize()" method should not be called
- "equals" method overrides should accept "Object" parameters
- "Thread.run()" should not be called directly
- Methods should not be named "tostring", "hashcode" or "equal"
- "StringBuilder" and "StringBuffer" should not be instantiated with a character
- "Iterator.hasNext()" should not call "Iterator.next()"
- Synchronization should not be done on instances of value-based classes
- Classes should not be compared by name
- "Externalizable" classes should have no-arguments constructors
- Custom serialization methods should have required signatures
- Reflection should not be used to check non-runtime annotations
- Invalid "Date" values should not be used
- Collections should not be passed as arguments to their own methods
- "hashCode" and "toString" should not be called on array instances
- "writeObject" argument must implement "Serializable"
- String operations with predictable outcomes should be avoided
- Values should not be uselessly incremented
- "Double.longBitsToDouble" should take "long" as argument
- Classes extending java.lang.Thread should provide a specific "run" behavior
- Dissimilar primitive wrappers should not be used with the ternary operator without explicit casting
- Unnecessary equality checks should not be made
- Inappropriate "Collection" calls should not be made
- Child class methods named for parent class methods should be overrides
- ".equals()" should not be used to test the values of "Atomic" classes
- "toString()" and "clone()" methods should not return null
- Servlets should not have mutable instance fields
- A "for" loop update clause should move the counter in the right direction
- Loop conditions should be true at least once
- "Object.wait()", "Object.notify()" and "Object.notifyAll()" should only be called from synchronized code
- Non-serializable objects should not be stored in "javax.servlet.http.HttpSession" instances
- "notifyAll()" should be preferred over "notify()"
- Inappropriate regular expressions should not be used
- "read" and "readLine" return values should be used
- Non-existent operators like "=+" should not be used
- Unary prefix operators should not be repeated
- Getters and setters should be synchronized in pairs
- Raw byte values should not be used in bitwise operations in combination with shifts
- Record component accessors should not be called before the fields are assigned
- Indexes to passed to "String" operations should be within the string's bounds
- Assignment of lazy-initialized members should be the last step with double-checked locking
- Min and max used in combination should not always return the same value
- "volatile" variables should not be used with compound operators
- Expressions used in "assert" should not produce side effects
- Value-based classes should not be used for locking
- Intermediate Stream methods should not be left unused
- Collection sizes and array length comparisons should make sense
- Exceptions should not be created without being thrown
- Week Year ("YYYY") should not be used for date formatting
- "iterator" should not return "this"
- "compareTo" should not be overloaded
- InputSteam.read() implementation should not return a signed byte
- JUnit5 test classes and methods should not be silently ignored
- AssertJ configuration should be applied
- AssertJ methods setting the assertion context should come before an assertion
- DateTimeFormatters should not use mismatched year and week numbers
- Assertions should not be used in production code
- Tests method should not be annotated with competing annotations
- The regex escape sequence \cX should only be used with characters in the @-_ range
- AssertJ assertions with "Consumer" arguments should contain assertion inside consumers
- Reflection should not be used to increase accessibility of records' fields
- Collections should not be modified while they are iterated
- Model attributes should follow the Java identifier naming convention
- Async methods should return void or Future
- "@Bean" methods for Singleton should not be invoked in "@Configuration" when proxyBeanMethods is false
- Path template variables should be properly bound to method parameters
- Beans in "@Configuration" class should have different names
- "setDaemon", "setPriority" and "getThreadGroup" should not be invoked on virtual threads
- Virtual threads should not run tasks that include synchronized code
- "Math.clamp" should be used with correct ranges
- "String.indexOf" should be used with correct ranges
- Instant APIs should only use supported temporal units
- ScopedValue instances should be assigned to a stable reference
- equals() implementations should not compare mismatched members
- A bare "." should not be used as a regular expression
- "Comparable.compareTo()" and "Comparator.compare()" should not use subtraction on numerical fields
- Duplicate keys or elements should not be passed to immutable collection factory methods
- hashCode() and equals() should use consistent fields
- Copy constructors should initialize all fields
- Duration conversions should agree with the supplied TimeUnit
- "Duration.get()" should only be called with SECONDS or NANOS
- "contentEquals()" should be used instead of "equals()" for "CharSequence" comparisons
- Method references not fulfilling the "Comparator" contract should not be used as comparators
- IdentityHashMap keys should not be boxed primitive types
- Sorted collections should have an ordering for their element or key types
- "Arrays.fill()" should not be called with an incompatible value type

### Vulnerabilities
- OS commands should not be vulnerable to command injection attacks **(critical)**
- LDAP queries should not be vulnerable to injection attacks **(critical)**
- XPath expressions should not be vulnerable to injection attacks **(critical)**
- A secure password should be used when connecting to a database **(critical)**
- XML parsers should not be vulnerable to XXE attacks **(critical)**
- Deserialization should not be vulnerable to injection attacks **(critical)**
- HTTP request redirections should not be open to forging attacks **(critical)**
- Dynamic code execution should not be vulnerable to injection attacks **(critical)**
- Server-side templates should not be vulnerable to injection attacks **(critical)**
- Extracting archives should not lead to zip slip vulnerabilities **(critical)**
- XML parsers should not allow inclusion of arbitrary files **(critical)**
- Components should not be vulnerable to intent redirection **(critical)**
- WebViews should not be vulnerable to cross-app scripting attacks **(critical)**
- Password hashing functions should use an unpredictable salt
- "HttpServletRequest.getRequestedSessionId()" should not be used
- Custom cryptographic algorithms should not be used
- Cipher Block Chaining IVs should be unpredictable
- Secure random number generators should not output predictable values
- Cryptographic keys should be robust
- LDAP connections should be authenticated
- JavaBean properties should not be populated from untrusted input
- Jackson deserialization should not use unsafe configuration
- "HttpSecurity" URL patterns should be correctly ordered
- Android broadcast receivers should not be registered without a permission
- Sensitive data should not be stored in Android external storage
- Passwords should not be stored in plaintext or with a fast hashing algorithm
- Insecure temporary file creation methods should not be used
- Cipher algorithms should be robust
- JWT should be signed and verified with strong cipher algorithms
- Regular expressions should not cause catastrophic backtracking
- A new session should be created during user authentication
- Thread suspensions should not be vulnerable to Denial of Service attacks
- Counter Mode initialization vectors should not be reused
- Apache XML RPC extensions should not be enabled
- File permissions should not be set to world-accessible values
- Deserialization of Java objects from LDAP should be disabled
- Intents should not be broadcast without receiver permissions
- OpenSAML2 should be configured to prevent authentication bypass
- Authentication mechanisms should not permit user enumeration
- Authorizations should be based on strong decisions
- Reflection should not be vulnerable to injection attacks
- Applications should not create session cookies from untrusted input
- Android KeyStore keys should require user authentication
- Biometric authentication should be cryptographically bound
- Mobile database encryption keys should not be disclosed
- System command arguments constructed from user input should be validated
- WebViews should not allow unrestricted file access
- XML parsers should not be vulnerable to Denial of Service attacks
- XML signatures should be validated securely
- XML operations should not be vulnerable to injection attacks
- Environment variables should not be defined from untrusted input
- Accessing files should not lead to filesystem oracle attacks
- Native methods should not be exposed to untrusted WebView content
- Sensitive information should not be logged in production builds

### Code smells
- Future keywords should not be used as names **(critical)**
- "switch" statements should not contain non-case labels **(critical)**
- Short-circuit logic should be used in boolean contexts **(critical)**
- JUnit test cases should call super methods **(critical)**
- Unnecessary bit operations should not be performed **(critical)**
- Assertions should be complete **(critical)**
- "clone" should not be overridden **(critical)**
- "ThreadGroup" should not be used **(critical)**
- The "Object.finalize()" method should not be overridden
- Exceptions should not be thrown in finally blocks
- "Object.finalize()" should remain protected (versus public) when overriding
- Interfaces should not solely consist of constants
- Execution of the Garbage Collector should be triggered only by the JVM
- Package declaration should match source file directory
- "for" loop increment clauses should modify the loops' counters
- "readResolve" methods should be inheritable
- "Cloneables" should implement "clone"
- Class names should not shadow interfaces or superclasses
- JUnit assertions should not be used in "run" methods
- Unnecessary mathematical comparisons should not be made
- "IllegalMonitorStateException" should not be caught
- "Object.wait(...)" and "Condition.await(...)" should be called inside a "while" loop
- Method overrides should not change contracts
- "indexOf" checks should not be for positive numbers
- "static" base class members should not be accessed via derived types
- Conditionals should start on new lines
- A conditionally executed single line should be denoted by indentation
- "equals" method parameters should not be marked "@Nonnull"
- "default" clauses should be last
- String offset-based methods should be preferred for finding substrings from offsets
- Derived exceptions should not hide their parents' catch blocks
- Class members annotated with "@VisibleForTesting" should not be accessed from production code
- Methods setUp() and tearDown() should be correctly annotated starting with JUnit4
- Empty lines should not be tested with regex MULTILINE flag
- Mocking all non-private methods of a class should be avoided
- Optional REST parameters should have an object type
- Reverse iteration should utilize reversed view
- Reverse view should be used instead of reverse copy in read-only cases
- Injecting data into static fields is not supported by Spring
- Methods returning "Page" or "Slice" must take "Pageable" as an input parameter
- Methods annotated with "@BeforeTransaction" or "@AfterTransaction" must respect the contract
- Methods with "@Transactional" should specify rollback behavior for checked exceptions
- Throwable and Error should not be caught
- "for" loop stop conditions should be invariant
- Constructors should not be used to instantiate "String", "BigInteger", "BigDecimal" and primitive-wrapper classes
- "StringBuilder" data should be used
- Regexes containing characters subject to normalization should use the CANON_EQ flag
- Use record pattern instead of explicit field access
- Use built-in "Math.clamp" methods
- Anonymous classes on functional interfaces should be lambdas
- Use batch Processing in JDBC
- Dangling Javadoc comments should be removed
