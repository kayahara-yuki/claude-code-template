---
description: "Clarify ambiguities in plans with structured questions"
allowed-tools:
  - Write
  - Edit
  - Read
  - Grep
  - Glob
  - TodoRead
  - TodoWrite
  - AskUserQuestion
context: fork
---

# Plan Clarification (/dig)

Read the current plan/requirements and identify unclear points through structured questioning.

## Phases

### Phase 1: Gather Context

Read all available context files:
- `CLAUDE.md` (project root and .claude/)
- `README.md`
- `prd.md`, `spec.md`, or similar in docs/
- Any `.md` files in `docs/` or `plans/`
- Current todo list

Also check for:
- Existing code patterns that might constrain decisions
- Test files that indicate expected behavior
- Config files that reveal tech stack choices

### Phase 2: Identify Ambiguities

Analyze the plan across these categories:

| Category | What to Check |
|----------|---------------|
| **Architecture** | System boundaries, component responsibilities, data flow |
| **Data** | Storage mechanism, formats, validation rules, migrations |
| **API** | Endpoints, request/response contracts, authentication |
| **UI/UX** | User flows, interactions, edge case handling, states |
| **Testing** | Coverage requirements, test types needed, test data |
| **DevOps** | Deployment targets, environments, CI/CD requirements |
| **Scope** | MVP definition, feature priorities, out-of-scope items |

For each category, identify:
- Missing information that blocks implementation
- Implicit assumptions that need confirmation
- Trade-offs that require user decision

### Phase 3: Ask Questions

Use the **AskUserQuestion** tool with:
- 2-4 questions per round
- Each question has 2-4 concrete options
- Each option includes brief pros/cons or implications
- Avoid open-ended questions (specific choices only)
- "Other" option is automatically added by the tool

**Question format example:**
```
Header: "Data Storage"
Question: "How should user preferences be stored?"
Options:
  - "LocalStorage": Pros: Simple, no backend. Cons: Browser-only, 5MB limit.
  - "Database": Pros: Persistent, shareable. Cons: Requires backend API.
  - "File-based": Pros: Portable. Cons: Manual import/export needed.
```

### Phase 4: Apply Decisions

After receiving answers, document in a decisions table:

| Item | Choice | Reason | Notes |
|------|--------|--------|-------|
| Data storage | LocalStorage | Fast iteration, MVP scope | Consider migration path |
| Auth method | None for MVP | Reduce complexity | Add in v2 |

Update any plan files or todo items to reflect decisions.

### Phase 5: Iterate

Return to Phase 2 and check for remaining ambiguities.
Continue until the plan is clear enough to start implementation.

Signal completion with:
```
All ambiguities resolved. Ready for implementation.
```

## Important Rules

1. **Must use AskUserQuestion tool** - never ask questions conversationally
2. **Check CLAUDE.md for language preference** - match user's language in questions
3. **Every option needs context** - always include implications or trade-offs
4. **multiSelect: false by default** - only use multiSelect for non-exclusive choices
5. **Continue digging** - don't stop until plan is implementation-ready
6. **Respect existing decisions** - don't re-ask about already-decided items
