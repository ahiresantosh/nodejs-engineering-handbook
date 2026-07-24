# Chapter 15 – Testing Standards

## Purpose

Testing verifies that the application behaves as expected.

It helps developers:

- Find bugs early
- Prevent regressions
- Refactor with confidence
- Improve code quality
- Deliver reliable software

Testing is part of development, not an optional activity.

---

# Testing Pyramid

Not every test has the same purpose.

Follow the Testing Pyramid.

```text
             E2E Tests
          (Few & Critical)
                ▲
                │
      Integration Tests
        (Important Flows)
                ▲
                │
          Unit Tests
          (Most Tests)
```

Write many Unit Tests, fewer Integration Tests, and only essential End-to-End Tests.

---

# Types of Tests

| Test Type | Purpose |
|------------|----------|
| Unit Test | Test a single function or class |
| Integration Test | Verify multiple components work together |
| End-to-End (E2E) Test | Test complete user journeys |

---

# Unit Testing

A Unit Test verifies one unit of code in isolation.

Example:

```text
UserService

↓

Mock Repository

↓

Test Business Logic
```

Unit Tests should not use:

- Database
- Network
- External APIs
- File System

---

# Example Unit Test

```ts
it("should create a new user", async () => {

    const dto = {
        name: "John",
        email: "john@example.com"
    };

    const result = await userService.create(dto);

    expect(result.email).toBe(dto.email);

});
```

Unit Tests should be fast and independent.

---

# Mock Dependencies

Replace external dependencies with mocks.

Example:

```text
Service

↓

Mock Repository
```

This allows testing business logic without connecting to a real database.

---

# Integration Testing

Integration Tests verify that multiple components work together.

Example:

```text
Controller

↓

Service

↓

Repository

↓

Database
```

Typical scenarios:

- Create User
- Login
- Create Order
- Save Device Data

---

# End-to-End (E2E) Testing

E2E Tests simulate real user behavior.

Example:

```text
Login

↓

Create Order

↓

Make Payment

↓

View Order
```

These tests verify the complete application flow.

---

# What Should Be Tested?

Every feature should include tests for:

- Success scenarios
- Failure scenarios
- Validation errors
- Edge cases

Example:

```text
Create User

✔ Valid request

✔ Duplicate email

✔ Missing email

✔ Invalid email format
```

---

# Test Naming

Use descriptive test names.

### ✅ Good

```text
should create a new user

should reject duplicate email

should return 404 when user is not found
```

### ❌ Avoid

```text
test1

createUserTest

sampleTest
```

A test name should clearly describe the expected behavior.

---

# Arrange – Act – Assert (AAA)

Structure every test using the AAA pattern.

```text
Arrange

↓

Act

↓

Assert
```

### Example

```ts
// Arrange
const dto = {
    email: "john@example.com"
};

// Act
const result = await userService.create(dto);

// Assert
expect(result.email).toBe(dto.email);
```

This keeps tests easy to read.

---

# Test Isolation

Every test should run independently.

Avoid:

- Shared state
- Dependency on test execution order
- Reusing mutable data

A test should pass whether it runs first or last.

---

# Test Data

Use small and meaningful test data.

### Good

```text
john@example.com
```

### Avoid

```text
asdf123@testexample123.com
```

Readable data makes tests easier to understand.

---

# Code Coverage

Code coverage measures how much code is executed by tests.

Focus on testing:

- Business logic
- Error handling
- Edge cases

Do not chase 100% coverage at the cost of meaningful tests.

Quality is more important than percentage.

---

# Common Mistakes

## ❌ Testing Private Methods

Test behavior through public methods.

---

## ❌ Testing Framework Code

Do not test Express, Prisma, or other libraries.

Test your own business logic.

---

## ❌ Large E2E Test Suites

Keep E2E Tests focused on critical user journeys.

---

## ❌ Shared Test Data

Each test should prepare its own data.

---

## ❌ Missing Negative Tests

Always test failure scenarios.

---

# Testing Checklist

Before merging code:

- [ ] Unit Tests added
- [ ] Success scenarios tested
- [ ] Failure scenarios tested
- [ ] Validation scenarios tested
- [ ] Edge cases covered
- [ ] Dependencies mocked
- [ ] Test names meaningful
- [ ] Tests run successfully

---

# Best Practices

- Write Unit Tests for business logic.
- Mock external dependencies.
- Keep tests independent.
- Test both success and failure cases.
- Follow the AAA pattern.
- Keep tests fast and readable.
- Focus on behavior, not implementation.

---

# Engineering Rule

> **Test the behavior of your code, not its implementation. A good test verifies what the system should do, not how it does it.**

---

# Summary

Testing is an essential part of software development.

By writing clear, independent, and meaningful tests, developers can confidently build new features, fix bugs, and refactor code without introducing regressions.

A balanced testing strategy using Unit, Integration, and End-to-End tests leads to reliable and maintainable applications.
