---
name: tidying
description: "Kent Beck's 'Tidy First?' methodology. Small, safe, optional structural improvements that don't change behavior. Auto-triggers for code cleanup and refactoring tasks."
---

# INSTRUCTIONS

Apply Kent Beck's "Tidy First?" principles for structural improvements that don't change behavior.

## Core Principle

Tidying is about making the code easier to work with BEFORE making behavioral changes. Keep tidying commits separate from feature commits.

## Types of Tidyings

| Type | Description | Example |
|------|-------------|---------|
| **Guard Clauses** | Replace nested conditionals with early returns | `if (!valid) return;` |
| **Extract Helper** | Pull out reusable code into named functions | `const formatted = formatDate(date)` |
| **Remove Dead Code** | Delete unused variables, functions, imports | Delete commented-out code |
| **Normalize Symmetry** | Make similar things look similar | Consistent naming patterns |
| **Reorder** | Move related code closer together | Group related functions |
| **Rename** | Improve clarity of names | `data` → `userPreferences` |
| **Inline** | Remove unnecessary indirection | Inline single-use variables |

## When to Tidy

| Timing | Description |
|--------|-------------|
| **Before** | Tidy to make the next change easier |
| **After** | Tidy to clean up after a change |
| **Later** | Add to backlog for future tidying |
| **Never** | Leave it if not worth the effort |

## Workflow

1. Identify ONE structural improvement
2. Verify tests pass before starting
3. Make the structural change (NO behavior change)
4. Run tests to verify nothing broke
5. Commit with `tidy:` prefix
6. Repeat or move to feature work

## Commit Message Format

```
tidy: <what was improved>

<why this makes the code easier to work with>
```

## Critical Rules

- **ONE tidying per commit**: Small, focused changes
- **Tests must pass**: Before AND after each tidying
- **No behavior changes**: If it changes what the code does, it's not tidying
- **Separate from features**: Never mix tidying with feat/fix commits
