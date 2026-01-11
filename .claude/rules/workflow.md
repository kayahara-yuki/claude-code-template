# Development Workflow

## Before Starting Work

1. **Understand the requirement fully**
   - Read all available context (CLAUDE.md, README, specs)
   - Use `/dig` if anything is unclear

2. **Break down into small increments**
   - Each increment should be testable independently
   - Aim for commits that can be reverted cleanly

3. **Check existing patterns**
   - How is similar functionality implemented?
   - What testing patterns are used?

## During Development

### TDD Cycle (Red-Green-Refactor)

1. **Red**: Write a failing test for the next piece of functionality
2. **Green**: Write minimal code to make the test pass
3. **Refactor**: Clean up while keeping tests green

Use `tdd` skill for guidance.

### Tidying (Structural Improvements)

For code cleanup without behavior changes:
- Use `tidying` skill
- Keep tidying commits separate from feature commits
- Follow "Tidy First?" principles

### Before Each Commit

1. Run tests locally
2. Run lint/format checks
3. Use `deslop` skill to clean AI patterns
4. Review diff for unintended changes

## When CI Fails

1. Let `fix-ci` skill diagnose automatically
2. Review suggested fixes before applying
3. If manual intervention needed:
   - Run failed checks locally first
   - Never weaken tests/lint to pass
   - Ask for help if stuck

## Commit Messages

Format:
```
<type>: <subject>

<body explaining WHY, not WHAT>

Co-Authored-By: Claude <noreply@anthropic.com>
```

Types:
- `feat`: New feature
- `fix`: Bug fix
- `refactor`: Code change that neither fixes bug nor adds feature
- `test`: Adding or updating tests
- `docs`: Documentation only
- `chore`: Maintenance tasks
- `tidy`: Structural improvement (Tidy First)

## Documentation Layers

| Layer | Documents |
|-------|-----------|
| Code | HOW it works |
| Tests | WHAT it should do |
| Commits | WHY the change was made |
| Comments | WHY NOT (edge cases, rejected alternatives) |

Keep each layer focused. Don't duplicate information across layers.

## Code Quality

- Avoid over-engineering
- Don't add features beyond what's requested
- Simple is better than clever
- Three similar lines > premature abstraction
- Only validate at system boundaries
