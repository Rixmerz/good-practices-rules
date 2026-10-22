---
paths:
  - "**/*.{sql,pks,pkb,pls,plb,pck,trg}"
---
<!-- Installed by good-practices-rules. Edit the package, not this copy: the next install replaces it. -->
# Good practices: SQL and PL/SQL

- Never `SELECT *`; list the columns.
- Move repeated literals into a constant.

## More rules to follow
Write code that already complies with each one; the ones marked **critical** break builds or production.

### Bugs
- Anchored types should not be constrained **(critical)**
- "FETCH ... BULK COLLECT INTO" should not be used without a "LIMIT" clause **(critical)**
- "RAISE_APPLICATION_ERROR" should only be used with error codes from -20,000 to -20,999 **(critical)**
- Constant declarations should contain initialization assignments **(critical)**
- "DELETE" and "UPDATE" statements should contain "WHERE" clauses **(critical)**
- Scale should not be specified for float types **(critical)**
- "FORALL" statements should use the "SAVE EXCEPTIONS" clause **(critical)**
- Improper constraint forms should not be used **(critical)**
- Constraints should not be applied to types that cannot be constrained **(critical)**
- "END LOOP" should be followed by a semicolon **(critical)**
- "NCHAR" and "NVARCHAR2" size should not be specified in bytes **(critical)**
- "NOT NULL" variables should be initialized **(critical)**
- Predefined exceptions should not be overridden **(critical)**
- Pipelined functions should have at least one "PIPE ROW" statement and not return an expression **(critical)**
- Inserts should include values for non-null columns **(critical)**
- Size should be specified for string variables **(critical)**
- Variables should be declared only once in a scope **(critical)**
- Collections should not be iterated in "FOR" loops
- Functions should end with "RETURN" statements
- Positional and named arguments should not be mixed in invocations
- Nullable subqueries should not be used in "NOT IN" conditions
- Individual "WHERE" clause conditions should not be unconditionally true or false
- "COMMIT" should not be used inside a loop
- "COMMIT" and "ROLLBACK" should not be called from non-autonomous transaction triggers
- "VARCHAR2" and "NVARCHAR2" should be used
- All code should be reachable
- "MLSLABEL" should not be used
- "NULL" should not be compared directly
- The "result_cache" hint should be avoided
- "PACKAGE BODY" initialization sections should not contain "RETURN" statements
- "IF" statement conditions should not evaluate unconditionally to "TRUE" or to "FALSE"
- Unary prefix operators should not be repeated
- "WHERE" clause conditions should not be contradictory
- Strings should only be moved to variables or columns which are large enough to hold them
- "ROWNUM" should not be used at the same query level as "ORDER BY"
- Output parameters should be assigned
- DML events clauses should not include multiple "OF" clauses

### Vulnerabilities
- Dynamic code execution should not use user-controlled data
- Cipher algorithms should be robust

### Code smells
- GOTO should not be used to jump backwards **(critical)**
- Block start and end labels should match **(critical)**
- Loop start and end labels should match **(critical)**
- Quoted identifiers should not be used **(critical)**
- Block labels should appear on the same lines as "END"
- "CREATE OR REPLACE" should be used instead of "CREATE"
- "EXCEPTION_INIT -20,NNN" calls should be centralized
- Blocks containing "EXECUTE IMMEDIATE" should trap all exceptions
- Whitespace and control characters in string literals should be explicit
- "LOOP ... END LOOP;" constructs should be avoided
- Constant names should comply with a naming convention
- "IF" statements should not be nested too deeply
- "LIKE" clauses should not be used without wildcards
- "SELECT" statements used as argument of "EXISTS" statements should be selective
- SQL "JOIN" conditions should involve all joined tables
- Weak "REF CURSOR" types should not be used
