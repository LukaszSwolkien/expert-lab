# AI Agent Instructions

---

# Code Review

When reviewing code, check for these issues:

| Issue | Look For | Fix |
|-------|----------|-----|
| Magic numbers | Hardcoded values in logic | Extract to named constants |
| Duplicated logic | Same code in multiple places | Extract to shared function/module |
| Dead code | Unused parameters, variables, imports | Remove |
| Missing validation | User inputs not checked | Add boundary checks |
| Resource leaks | Objects not cleaned up | Add proper cleanup/disposal |
| Repeated initialization | Same setup on every call | Cache or initialize once |
| Poor error handling | Silent failures, missing logs | Add logging and user feedback |

**Prioritize issues by impact**: security > correctness > performance > maintainability.

---

# Debugging

1. **Reproduce first** - Confirm the issue exists and understand exact conditions
2. **Add logging** - Trace data flow at key points (inputs, transformations, outputs)
3. **Verify assumptions** - Check paths, values, conditions that "should" work
4. **Check platform differences** - File paths, bundling, environment variables
5. **Fix root cause** - Don't mask symptoms with defensive code

---

# Refactoring

| Goal | Action |
|------|--------|
| Remove duplication | Extract shared logic to dedicated module |
| Eliminate magic numbers | Move to named constants in central location |
| Reduce coupling | Make dependencies explicit, optional sections truly optional |
| Simplify | Remove unused code, merge redundant abstractions |

**Keep changes minimal** - refactor only what's needed, don't over-engineer.

---

# Unit Tests

## Before Writing Tests

1. **Understand domain constraints** - Ask about valid value ranges, physical limits, business rules
2. **Review existing tests** - Check for duplicates before creating new tests
3. **Identify testable logic** - Focus on code with behavior, not trivial getters/constants

---

## Test Requirements

Every test you create MUST:

- [ ] Use **realistic inputs** that can occur in production
- [ ] Test **actual logic**, not trivial code (getters, constants)
- [ ] Be **unique** - no overlap with existing test coverage
- [ ] Have **meaningful assertions** that verify behavior
- [ ] Follow naming: `test_<unit>_<scenario>_<expected_outcome>`
- [ ] Use **Arrange-Act-Assert** structure

---

## Self-Review Before Submitting

After creating tests, verify each one:

1. Can these input values actually occur in the real system?
2. Does this test duplicate existing coverage?
3. Does this test verify meaningful behavior?
4. Are assertions testing the right thing?

If you find issues, fix them before presenting the tests.
