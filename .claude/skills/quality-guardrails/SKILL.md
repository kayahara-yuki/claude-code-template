---
name: quality-guardrails
description: "Fix implementation when tests fail, lint errors occur, or CI breaks. Never weaken tests or config to pass. Auto-triggers on check failures."
---

# INSTRUCTIONS

When tests, lint, or CI fail, fix the implementation—not the validation.

## Core Principle

Tests and linting are specifications. They define correct behavior. When they fail:
- The implementation is wrong, OR
- The specification changed (requires explicit approval)

## Prohibited Actions (Without User Approval)

### Tests
- Adding `skip`, `only`, `xdescribe`, `xit`
- Removing or weakening assertions
- Changing expected values to match buggy behavior
- Deleting test cases

### Lint/Format
- Modifying ESLint, Prettier, TSConfig to relax rules
- Adding `// eslint-disable`, `# noqa` suppressions
- Changing scripts to skip checks

### CI/Hooks
- Modifying `.github/workflows/**` to skip checks
- Modifying `.husky/**` or pre-commit hooks
- Adding `--no-verify` flags

## When Checks Fail

Follow this process:

1. **Identify the FIRST failure**
   - Don't try to fix everything at once

2. **Understand the error**
   - Read the message carefully
   - Understand what the check expects

3. **List potential causes** (up to 3)
   - What could be causing this?

4. **Fix the implementation**
   - NOT the test or config

5. **Re-run to verify**
   - Ensure the fix works

## If Config Change is Genuinely Needed

Before modifying any protected file, you MUST:

1. **Explain why implementation can't be fixed**
   - What makes this unfixable in code?
   - Why is the rule/test incorrect?

2. **Show the specific change**
   - File path and diff

3. **Analyze impact**
   - Could this hide real bugs?

4. **Wait for explicit user approval**
   - Do not proceed without confirmation

## Protected File Patterns

```
**/*.test.*
**/*.spec.*
**/__tests__/**
eslint*, .eslintrc*
prettier*, .prettierrc*
tsconfig*
jest.config.*, vitest.config.*
.husky/**
.github/workflows/**
```

## Escalation

If stuck after 3 attempts:
- Summarize what was tried
- Explain why each approach failed
- Ask for human guidance
