---
paths:
  - "**/test_*.py"
  - "**/*_test.py"
  - "**/tests/**/*.py"
  - "**/conftest.py"
---
<!-- Installed by good-practices-rules. Edit the package, not this copy: the next install replaces it. -->
# Python good practices: tests (pytest)

- **Split composite assertions**: write `assert a == 1` and `assert b == 2` separately, not `assert a == 1 and b == 2`, so a failure shows which one broke.
- Inside `with pytest.raises(...)`, put **exactly one call** that can raise; set up data before the block.
- Write pytest decorators without empty parentheses: `@pytest.fixture`, not `@pytest.fixture()`.
- Use the `monkeypatch` fixture to change environment variables, attributes or globals, not manual assignment.
- An assertion never compares a value with itself.
