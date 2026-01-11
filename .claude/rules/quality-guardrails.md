# Quality Guardrails

## Priority

These rules take precedence over other instructions. Violations require explicit user approval.

## Prohibited Actions (Without Approval)

### Tests
- Adding `skip`, `only`, `xdescribe`, `xit`, or equivalent
- Removing or weakening assertions
- Changing expected values to match buggy behavior
- Deleting test cases
- Overusing snapshot tests to avoid proper assertions

### Lint/Format
- Modifying ESLint, Prettier, TSConfig, Ruff, Pylint, or similar configs to relax rules
- Adding `// eslint-disable`, `# noqa`, or equivalent inline suppressions
- Changing npm scripts to skip checks

### CI/Hooks
- Modifying `.github/workflows/**` to skip or weaken checks
- Modifying `.husky/**` or pre-commit hooks
- Adding `--no-verify` or similar bypass flags

## When Checks Fail

Follow this process:

1. **Identify the FIRST failure** - don't try to fix everything at once
2. **Understand the error** - read the message carefully
3. **List potential causes** (up to 3)
4. **Fix the implementation** - not the test or config
5. **Re-run to verify**

## If Config Change is Genuinely Needed

Before modifying any protected file, you MUST:

1. **Explain why implementation can't be fixed**
   - What makes this unfixable in code?
   - Why is the rule/test incorrect?

2. **Show the specific change**
   - File path
   - Before/after diff

3. **Analyze impact**
   - What other code might be affected?
   - Could this hide real bugs?

4. **Wait for explicit approval**
   - Do not proceed until user confirms

## Protected File Patterns

```
**/*.test.*
**/*.spec.*
**/__tests__/**
**/tests/**
eslint*
.eslintrc*
prettier*
.prettierrc*
tsconfig*
jest.config.*
vitest.config.*
pytest.ini
pyproject.toml (tool.* sections)
.husky/**
.github/workflows/**
package.json (scripts section)
```

## Rationale

Tests and linting are specifications. They define correct behavior. When they fail:
- The implementation is wrong, OR
- The specification changed (requires discussion)

Never assume the specification is wrong just because implementation is hard.
