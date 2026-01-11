---
name: fix-ci
description: "Automatically diagnose and fix CI failures. Auto-triggers when PR has failing checks or user mentions CI problems."
context: fork
allowed-tools:
  - Bash
  - Read
  - Edit
  - Write
  - Grep
  - Glob
  - WebSearch
  - Task
---

# CI Failure Auto-Fix

Diagnose and fix CI failures in the current pull request.

## Prerequisites

- GitHub CLI (`gh`) must be installed and authenticated
- Must be in a git repository with an open PR

## Workflow

### Step 1: Get CI Status

```bash
gh pr view --json statusCheckRollup --jq '.statusCheckRollup[] | select(.conclusion == "FAILURE" or .state == "FAILURE")'
```

If no PR exists or command fails:
```bash
gh run list --limit 5 --json status,conclusion,name,databaseId
```

### Step 2: Investigate Failures

For each failed check:

1. Get run details:
   ```bash
   gh run view <run-id>
   ```

2. Get failed logs:
   ```bash
   gh run view <run-id> --log-failed
   ```

3. Categorize failure type:
   - **Build errors**: Compilation, syntax, missing dependencies
   - **Test failures**: Unit, integration, e2e tests
   - **Lint/format issues**: ESLint, Prettier, Pylint, etc.
   - **Type errors**: TypeScript, mypy, etc.
   - **Security issues**: Dependency vulnerabilities, secrets detection
   - **Configuration problems**: Invalid config files, missing env vars

### Step 3: Research Solutions

Launch sub-agent (Task tool) to:
1. Search codebase for related patterns or similar fixes
2. Review recent commits that might have caused failure
3. Web search for specific error messages if needed
4. Check if the same checks pass locally

### Step 4: Implement Fix

1. Make targeted, minimal fixes addressing the root cause
2. Keep changes focused on the CI issue - no refactoring
3. Preserve existing code style
4. Follow quality-guardrails rules (never weaken tests/lint to pass)

### Step 5: Verify Locally

Run the same checks locally if possible:
- Build: `npm run build` / `cargo build` / etc.
- Tests: `npm test` / `pytest` / etc.
- Lint: `npm run lint` / `ruff check` / etc.
- Types: `tsc --noEmit` / `mypy` / etc.

### Step 6: Report Results

If fix successful after local verification:
```bash
git add -A && git status
```
Report what was fixed and recommend committing.

If still failing after 3 attempts:
Report findings and ask for human intervention.

## Output Format

```
## CI Fix Summary

### Status: [RESOLVED / PARTIALLY_RESOLVED / NEEDS_HUMAN_INTERVENTION]

### Issues Found:
- [List each failure with check name]

### Root Causes:
- [Explain why each check failed]

### Fixes Applied:
- [File: change description]

### Verification:
- [Local check results]

### Notes:
- [Any recommendations or remaining concerns]
```

## Critical Rules

1. **Never skip or weaken tests to make CI pass**
2. **Never modify lint/format configurations to bypass errors**
3. **Never disable or modify CI workflow files without explicit approval**
4. **Always fix the implementation, not the validation**
