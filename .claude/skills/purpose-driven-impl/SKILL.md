---
name: purpose-driven-impl
description: "Prevent hollow implementations, stubs, and hardcoded values. Auto-triggers when implementing functions, classes, or features. Ensures implementations serve real purposes, not just pass tests."
---

# INSTRUCTIONS

Ensure every implementation serves a real purpose and solves the actual problem.

## Anti-Patterns to Avoid

### 1. Hollow Implementations
```typescript
// BAD: Just returns expected value without logic
function calculate(a: number, b: number): number {
  return 42; // Hardcoded to pass test
}

// GOOD: Actual implementation
function calculate(a: number, b: number): number {
  return a * b + 10;
}
```

### 2. Stub Code
```typescript
// BAD: Empty or minimal implementation
async function fetchUser(id: string): Promise<User> {
  return {} as User; // Stub
}

// GOOD: Real implementation
async function fetchUser(id: string): Promise<User> {
  const response = await api.get(`/users/${id}`);
  return response.data;
}
```

### 3. Test-Driven Hardcoding
```typescript
// BAD: Only handles test cases
function greet(name: string): string {
  if (name === "Alice") return "Hello, Alice!";
  if (name === "Bob") return "Hello, Bob!";
  return "";
}

// GOOD: Generic implementation
function greet(name: string): string {
  return `Hello, ${name}!`;
}
```

## Self-Check Questions

Before marking implementation as complete, ask:

1. **Does this solve the actual problem?**
   - Not just pass the current tests

2. **Would this work with real data?**
   - Not just test fixtures

3. **Is there any hardcoded value that should be dynamic?**
   - Look for magic strings, numbers, conditionals

4. **Is this a stub that needs real logic?**
   - Empty functions, TODO comments, placeholder returns

5. **Does this handle edge cases?**
   - Null, empty, invalid inputs

## When Implementing

1. Understand the PURPOSE of the feature
2. Write tests that capture the PURPOSE (not just examples)
3. Implement to fulfill the PURPOSE (not just pass tests)
4. Verify with varied inputs (not just test data)

## Red Flags

- Implementation complexity doesn't match problem complexity
- Multiple `if` statements matching exact test values
- Comments like `// TODO`, `// FIXME`, `// Temporary`
- Type assertions (`as Type`) without validation
- Empty catch blocks or silent failures
