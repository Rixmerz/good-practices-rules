---
paths:
  - "**/*.py"
---
<!-- Installed by good-practices-rules. Edit the package, not this copy: the next install replaces it. -->
# Python good practices: FastAPI, async and security

## Top rules
- **Inside an `except`, use `logger.exception("msg")` instead of `logger.error(...)`**. `exception()` keeps the traceback; `error()` drops it.
- **Document every `HTTPException` an endpoint raises**: `@router.get(..., responses={404: {"description": "Not found"}})`.
- **Declare FastAPI dependencies with `Annotated`**: `db: Annotated[Session, Depends(get_db)]`, not `db: Session = Depends(get_db)`.
- **If an `async` function awaits nothing, drop the `async`**.
- Don't use f-strings without placeholders; use a plain string.

## FastAPI and Pydantic
- Don't repeat `response_model=` when the return annotation already says it.
- Optional Pydantic fields get an explicit default: `x: str | None = None`.
- Don't hard-code a bind to `0.0.0.0`; take the host from configuration.

## Async
- In an `async` function, don't use synchronous `requests` or `open()`; use `httpx.AsyncClient` and `aiofiles`/`anyio`.
- Keep a reference to every `asyncio.create_task(...)` so it isn't garbage-collected mid-flight.
- If you catch `asyncio.CancelledError`, re-raise it after cleanup.

## Types and dates
- Write unions as `int | None`, not `Optional[int]` or `Union[...]`. On Python 3.12+, use the `def f[T](x: T)` generic syntax.
- **Use `datetime.now(timezone.utc)`**, not `datetime.utcnow()`/`utcfromtimestamp()`, which are deprecated and naive.

## Exceptions
- Never raise generic `Exception` or `BaseException`; raise a specific one.
- Don't catch a subclass in the same `except` as its parent.

## Expressions
- No identical operands on both sides: `a or a`. No conditions that always evaluate the same.
- Don't compare floats with `==`; use `math.isclose`.
- Use `s.startswith(("a", "b"))` instead of `startswith("a") or startswith("b")`.
- No duplicate keys in a `dict` or values in a `set`.
- Don't wrap something that's already iterable in `list()` just to loop over it. Use `set.update()` instead of a loop of `add()` calls.
- Use `str.replace` when you don't need a regex. No redundant regex alternatives; group alternatives used with anchors.
- Check the length before indexing. Delete unreachable code.
- pandas `merge`/`join` calls pass `on`, `how` and `validate`.

## Naming
- Functions are `snake_case`: `^[a-z_][a-z0-9_]*$`.

## Security
- Never build file paths from user data; normalize the path and verify it stays inside the base directory.
- Never pass `verify=False` to requests/httpx. Use TLS 1.2+.
- Use `tempfile.mkstemp()`/`tempfile.TemporaryDirectory()`, not fixed paths in `/tmp`.
- Don't disable Jinja2 autoescape.

## More rules to follow
Write code that already complies with each one; the ones marked **critical** break builds or production.

### Bugs
- Unnecessary equality checks should not be made **(critical)**
- Recursion should not be infinite **(critical)**
- String formatting should not lead to runtime errors **(critical)**
- "yield" and "return" should not be used outside functions **(critical)**
- "__init__" should not return a value **(critical)**
- Only strings should be listed in "__all__" **(critical)**
- "__iter__" should return an iterator **(critical)**
- Variables, classes and functions should be defined before being used **(critical)**
- Iterable unpacking, "for-in" loops and "yield from" should use an Iterable object **(critical)**
- Function arguments should be passed only once **(critical)**
- Operators should be used on compatible types **(critical)**
- Raised Exceptions must derive from BaseException **(critical)**
- Dictionary unpacking should only be done with "mapping" objects **(critical)**
- "in" and "not in" operators should be used on objects supporting them **(critical)**
- Item operations should be done on objects supporting them **(critical)**
- Caught Exceptions must derive from BaseException **(critical)**
- Boolean expressions of exceptions should not be used in "except" statements **(critical)**
- Instance and class methods should have at least one positional parameter **(critical)**
- Special methods should have an expected number of parameters **(critical)**
- Property getter, setter and deleter methods should have the expected number of parameters **(critical)**
- Calls should not be made to non-callable values **(critical)**
- Only defined names should be listed in "__all__" **(critical)**
- The "open" builtin function should be called with a valid mode **(critical)**
- Assert should not be called on a tuple literal **(critical)**
- Equality checks should not be made against "numpy.nan" **(critical)**
- "super()" calls should not be used in TorchScript methods **(critical)**
- Method calls should use parentheses when saving PyTorch model state **(critical)**
- "NotImplemented" should not be used in boolean contexts **(critical)**
- Template strings should be processed before use **(critical)**
- Template and str should not be concatenated directly **(critical)**
- HTTP headers should be accessed safely to avoid KeyError exceptions **(critical)**
- Flask "preprocess_request()" return values should be handled **(critical)**
- "send_file" should specify "mimetype" or "download_name" when used with file-like objects **(critical)**
- Endpoints returning 204 status should have an empty response body **(critical)**
- Child routers should be included before parent router registration **(critical)**
- Django view functions should include all URL parameters in their signature **(critical)**
- Django Command classes should inherit from BaseCommand **(critical)**
- Django middleware should call super().__init__() with appropriate parameters **(critical)**
- Enum classes should not be decorated with "@dataclass" **(critical)**
- "StopIteration" should not be raised inside generators **(critical)**
- Attributes should only be assigned if they are declared in "__slots__" **(critical)**
- "@singledispatch" and "@singledispatchmethod" should not be confused **(critical)**
- Mutable default values should not be used with "dict.fromkeys()" or "ContextVar()" **(critical)**
- Multiple inheritance should not create Method Resolution Order (MRO) conflicts **(critical)**
- TypeVars should not be both covariant and contravariant **(critical)**
- Functions and methods should only return expected values **(critical)**
- Break, continue and return statements should not occur in "finally" blocks
- "break" and "continue" should not be used outside a loop
- Zero should not be a possible denominator
- Exceptions' "__cause__" should be either an Exception or None
- Assertions should not be made within the try block of a try-except catching AssertionError
- Assertions should not be made at the end of blocks expecting an exception
- Regex patterns following a possessive quantifier should not always fail
- Regex boundaries should not be used in a way that can never be matched
- Back references in regular expressions should only refer to capturing groups that are matched before the reference
- Regex lookahead assertions should not be contradictory
- Non-existent dictionary keys should not be accessed
- Unpacking should be done with the same number of elements of the iterable
- The abs_tol parameter should be provided when using math.isclose to compare values to 0
- offset-naive datetime.time and datetime.datetime objects should not be compared with offset-aware ones
- datetime.datetime objects should not be compared with datetime.date objects
- Transformers should not be accessed directly when a Scikit-Learn Pipeline uses caching
- Einops pattern should be valid
- Time-dependent expressions should not be used in class attributes
- Class-Based Views should override "get_context_data" correctly
- BeautifulSoup elements should be checked for None before accessing
- Pydantic models should enable either alias or name validation
- pydantic-core serialization calls should provide fallback handlers
- "json_schema_input_type" should not be used with "mode='after'" field validators
- Pydantic models with dataclass fields should explicitly set "revalidate_instances"
- Pytest fixtures should contain at most one yield statement
- Pydantic field validators should explicitly return the validated value
- Increment and decrement operators should not be used
- All "except" blocks should be able to catch exceptions
- Non-existent operators like "=+" should not be used
- Collection sizes and array length comparisons should make sense
- Exceptions should not be created without being raised
- New objects should not be created only to check their identity
- Alternation in regular expressions should not contain empty alternatives
- Replacement strings should reference existing regular expression groups
- Collections should not be modified while they are iterated
- ExceptionGroup and BaseExceptionGroup should not be caught with except*
- The '@receiver' (Django) and '@route' (Flask) decorators should be the outermost decorators
- The "safe" flag should be set to "False" when serializing non-dictionary objects in Django JSON-encoded responses
- Set members and dictionary keys should be hashable
- Sequence indexes must have an __index__ method
- Subclasses of "torch.nn.Module" should call the initializer
- Async functions should not contain synchronous subprocess calls
- Use non-blocking sleep functions in asynchronous code
- Async functions should not contain synchronous OS calls
- Cancellation scopes should contain checkpoints
- Async functions should not contain input() calls
- Control flow statements should not be used inside TaskGroup or Nursery blocks
- "async with" should be used for asynchronous resource management
- AWS Lambda handlers should return only JSON serializable values
- Network calls in AWS Lambda functions shouldn't be made without explicit timeout parameters
- AWS Lambda handlers should clean up temporary files in /tmp directory
- FastAPI path parameters should be declared in route handlers or dependencies
- Membership tests should not use empty collections
- Property methods should have a return statement
- Group iterators from "itertools.groupby" should not be reused
- Mutable or stateful values should not be used as default values in dataclass attributes
- CSS selectors should be used when selecting elements with multiple required classes
- Pytest fixtures should not combine "autouse" with "params"
- Parametrize decorators should not have empty parameter lists
- "pytest.raises" should be used as a context manager
- Objects passed to "reversed()" should support reverse iteration
- Objects used as context managers should implement the corresponding context manager protocol

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
- S3 buckets should not grant access to all users or authenticated users **(critical)**
- AWS resource-based policies should not grant public access **(critical)**
- Policies should not grant all privileges **(critical)**
- IAM policies should not grant access to all account resources **(critical)**
- Public network access to cloud resources should be disabled **(critical)**
- AWS API Gateway should require authentication **(critical)**
- Flask secret keys should not be disclosed **(critical)**
- JWT secret keys should not be disclosed **(critical)**
- HTTP response headers should not be vulnerable to response splitting attacks **(critical)**
- Long-term AWS access keys should not be used directly in code **(critical)**
- Sensitive data should not be passed in URL query parameters **(critical)**
- Password hashing functions should use an unpredictable salt
- Custom cryptographic algorithms should not be used
- Cipher Block Chaining IVs should be unpredictable
- Cryptographic key generation should be based on strong parameters
- LDAP connections should be authenticated
- Passwords should not be stored in plaintext or with a fast hashing algorithm
- Insecure temporary file creation methods should not be used
- Cipher algorithms should be robust
- JWT should be signed and verified
- Regular expressions should not cause catastrophic backtracking
- Amazon S3 bucket public access should be fully blocked
- AWS IAM policies should limit the scope of permissions given
- Loop boundaries should not be vulnerable to injection attacks
- GraphQL queries should not be vulnerable to Denial of Service attacks
- Models should not be loaded with "torch.load" without restricting deserialization
- File permissions should not be set to world-accessible values
- EBS volumes should be encrypted
- Applications should not create session cookies from untrusted input
- Amazon RDS resources should be encrypted at rest
- OpenSearch domains should have encryption at rest enabled
- SageMaker notebook instances should be encrypted at rest
- SQS queues should be encrypted
- Amazon EFS file systems should be encrypted
- System command arguments constructed from user input should be validated
- XML signatures should be validated securely
- Security groups should not allow unrestricted outbound traffic
- Environment variables should not be defined from untrusted input
- Accessing files should not lead to filesystem oracle attacks
- Memory allocations should not be vulnerable to Denial of Service attacks
- GraphQL introspection should be disabled in production
- S3 operations should verify bucket ownership using ExpectedBucketOwner parameter
- AI agents should be protected with guardrails
- AI agent code execution should be sandboxed
- Agentic workflows should not be vulnerable to command injection attacks
- Agentic workflows should not be vulnerable to SQL injection attacks
- Agentic workflows should not be vulnerable to server-side request forgery
- Agentic workflows should not be vulnerable to code injection attacks
- Agentic workflows should not be vulnerable to connection string injection

### Code smells
- Backticks should not be used **(critical)**
- The "exec" statement should not be used **(critical)**
- Numerically stable PyTorch functions should be used instead of their unstable equivalents **(critical)**
- Tensor copying should use recommended methods **(critical)**
- Tensor operations should rely on automatic broadcasting instead of manual expansion **(critical)**
- FastAPI applications should be passed as import strings when using reload, debug, or workers **(critical)**
- TestClient requests should use "content" parameter for bytes or text **(critical)**
- Router prefixes should be defined during "APIRouter" initialization **(critical)**
- CORSMiddleware should be added last in the middleware chain **(critical)**
- Wildcard imports should not be used
- Method overrides should not change contracts
- The first argument to class methods should follow the naming convention
- Unread "private" attributes should be removed
- Arguments given to functions should be of an expected type
- Bare "raise" statements should not be used in "finally" blocks
- Custom Exception classes should inherit from "Exception" or one of its subclasses
- Some special methods should return "NotImplemented" instead of raising "NotImplementedError"
- Function parameters' default values should not be modified or assigned
- "self" should be the first argument to instance methods
- Bare "raise" statements should only be used in "except" blocks
- "SystemExit" should be re-raised
- Constants should not be used as conditions
- np.nonzero should be preferred over np.where when only the condition parameter is set
- inplace=True should not be used when modifying a Pandas DataFrame
- Constructor attributes of date and time objects should be in the range of possible values
- The 12-hour format should be used with the AM/PM marker, otherwise the 24-hour format should be used
- "pytz.timezone" should not be passed to the "datetime.datetime" constructor
- "zoneinfo" should be preferred to "pytz" when using Python 3.9 and later
- Dates should be formatted correctly when using "pandas.to_datetime" with "dayfirst" or "yearfirst" arguments
- Numpy weekmask should have a valid value
- Python side effects should not be used inside a "tf.function"
- Subclasses of Scikit-Learn's "BaseEstimator" should not set attributes ending with "_" in the "__init__" method
- PySpark Window functions should always specify a frame
- Dictionary comprehension should not use a static key
- Specify "start_dim" when using "torch.flatten" to preserve batch dimension
- Query parameters should not be used in Flask POST requests
- Django view functions should declare URL parameters explicitly
- Querysets should use "select_related()" or "prefetch_related()" to avoid N+1 queries
- Classes should not inherit from the same base class multiple times
- Loop variables should not be reused in nested loops
- Class fields should not be defined multiple times
- Assertion argument order should be consistent within a file
- Implicit string and byte concatenations should not be confusing
- Tests should check which exception is thrown
- Reluctant quantifiers in regular expressions should be followed by an expression that can't match the empty string
- Asynchronous functions should not accept timeout parameters
- Function return types should be consistent with their type hint
