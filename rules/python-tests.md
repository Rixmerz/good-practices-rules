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
- Use `@pytest.mark.parametrize` for cases that differ only in data, with `ids=` when the values aren't readable.
- Use `tmp_path` for files, never fixed paths.
- Compare floats with `pytest.approx`.
- Check the message with `pytest.raises(ValueError, match="...")`.
- Mock where the name is looked up (`patch("app.service.requests")`), and prefer `autospec=True`.
- Don't let tests depend on each other's order or shared mutable globals; isolate with fixtures.
- Don't call the network in unit tests; fake it (`respx`, `responses`) or mark the test as integration.
- Mark async tests with `pytest.mark.asyncio` (or `anyio`) and await everything.
- An assertion never compares a value with itself.
