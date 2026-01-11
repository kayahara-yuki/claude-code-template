---
name: deslop
description: "Remove AI-generated code slop from changes. Auto-triggers when reviewing diffs, preparing commits, or cleaning up AI-generated code."
context: fork
allowed-tools:
  - Read
  - Edit
  - Write
  - Bash
  - Grep
  - Glob
  - Task
---

# Remove AI Code Slop

Check the diff against the main/master branch and remove all AI-generated slop introduced.

## What to Remove

### 1. Unnecessary Comments
- Comments stating the obvious (e.g., `// increment counter` before `counter++`)
- Comments that duplicate code meaning
- Comments inconsistent with file's existing style
- JSDoc/docstrings for internal/private functions that weren't documented before

### 2. Defensive Over-Engineering
- Try/catch blocks in trusted code paths where errors won't occur
- Null/undefined checks where values are guaranteed by types or prior validation
- Type guards that duplicate TypeScript's type system checks
- Fallback values for required parameters
- Excessive input validation in internal functions

### 3. Style Inconsistencies
- Variable naming that doesn't match file conventions (camelCase vs snake_case)
- Formatting that differs from existing code
- Import organization different from file pattern
- Extra blank lines or spacing not matching file style

## Process

1. Detect default branch:
   ```bash
   git symbolic-ref refs/remotes/origin/HEAD 2>/dev/null | sed 's@^refs/remotes/origin/@@' || echo "main"
   ```

2. Get diff against default branch:
   ```bash
   git diff <default-branch>...HEAD
   ```

3. For each changed file:
   - Read the full file to understand existing style
   - Compare added code against existing patterns
   - Identify AI-generated additions that don't match

4. Remove unnecessary additions while preserving functional changes

5. Report 1-3 sentence summary of what was cleaned up

## Important Guidelines

- **Preserve all functional code changes** - only remove stylistic/defensive additions
- **Match the existing code's style** - if the file has comments, keep similar ones
- **When in doubt, keep it** - only remove obvious AI patterns
- **Don't remove error handling that's actually needed** - focus on redundant handling
