# Chapter 20 – Engineering Checklist & Definition of Done

## Purpose

A feature is **not complete** simply because it works.

A feature is complete only when it:

- Meets business requirements
- Follows engineering standards
- Is tested
- Is secure
- Is reviewed
- Can be deployed safely

This chapter provides a final checklist to verify that every feature is production-ready.

---

# What is Definition of Done (DoD)?

The **Definition of Done** is a common agreement within the engineering team.

A feature is considered complete only when all required quality checks have been completed.

Following a consistent DoD helps maintain quality across every release.

---

# Development Lifecycle

```text
Requirement

↓

Design

↓

Development

↓

Testing

↓

Code Review

↓

CI/CD

↓

Deployment

↓

Monitoring
```

Every feature should complete this lifecycle.

---

# Engineering Checklist

## 1. Architecture

- [ ] Feature follows project structure
- [ ] Responsibilities are properly separated
- [ ] No business logic inside Controllers
- [ ] Repository pattern followed
- [ ] DTOs and Mappers implemented

---

## 2. Code Quality

- [ ] Code is simple and readable
- [ ] Meaningful naming used
- [ ] No duplicated logic
- [ ] Functions are small and focused
- [ ] Dead code removed

---

## 3. Validation

- [ ] Request validation implemented
- [ ] Business rules validated
- [ ] Input sanitized
- [ ] Validation errors handled properly

---

## 4. Error Handling

- [ ] Custom exceptions used
- [ ] Global exception handler implemented
- [ ] Correct HTTP status codes returned
- [ ] Sensitive information not exposed

---

## 5. Security

- [ ] Authentication implemented (if required)
- [ ] Authorization verified
- [ ] Sensitive data protected
- [ ] Secrets not hardcoded
- [ ] Input validated

---

## 6. Database

- [ ] Queries optimized
- [ ] Pagination implemented (where applicable)
- [ ] Transactions used correctly
- [ ] Database indexes reviewed
- [ ] No unnecessary database calls

---

## 7. Performance

- [ ] No blocking operations
- [ ] Heavy tasks moved to background jobs
- [ ] Caching considered
- [ ] External API calls optimized

---

## 8. Logging & Monitoring

- [ ] Errors logged
- [ ] Important business events logged
- [ ] No sensitive information in logs
- [ ] Trace ID included (where applicable)

---

## 9. Testing

- [ ] Unit Tests added
- [ ] Integration Tests updated
- [ ] Edge cases tested
- [ ] Existing tests passing

---

## 10. API Standards

- [ ] REST conventions followed
- [ ] Request DTO created
- [ ] Response DTO created
- [ ] API documentation updated

---

## 11. DevOps

- [ ] Build successful
- [ ] CI pipeline passed
- [ ] Environment variables configured
- [ ] Deployment ready

---

## 12. Documentation

- [ ] Code comments added only where necessary
- [ ] README updated (if required)
- [ ] API documentation updated
- [ ] Breaking changes documented

---

# Pull Request Checklist

Before creating a Pull Request:

- [ ] Feature completed
- [ ] Self-review completed
- [ ] Tests executed
- [ ] Lint passed
- [ ] Build successful
- [ ] Documentation updated
- [ ] No debug code left
- [ ] No commented-out code
- [ ] Pull Request description completed

---

# Reviewer Checklist

Before approving:

- [ ] Business requirements implemented
- [ ] Architecture followed
- [ ] Code readable
- [ ] Security reviewed
- [ ] Performance considered
- [ ] Tests verified
- [ ] Error handling reviewed
- [ ] CI passed

---

# Release Checklist

Before deploying to Production:

- [ ] CI/CD pipeline successful
- [ ] No critical bugs
- [ ] Database migration verified
- [ ] Rollback plan available
- [ ] Monitoring configured
- [ ] Health checks passing
- [ ] Stakeholders informed (if applicable)

---

# Common Reasons to Reject a Pull Request

A Pull Request should not be approved if:

- Business logic is incorrect
- Coding standards are not followed
- Tests are missing
- Validation is incomplete
- Security concerns exist
- Performance issues are identified
- Documentation is missing
- CI pipeline fails

Rejecting a Pull Request is about protecting the codebase—not criticizing the developer.

---

# Engineering Habits

Every developer should strive to:

- Write readable code.
- Keep solutions simple.
- Reuse existing components.
- Think about maintainability.
- Consider performance.
- Prioritize security.
- Test thoroughly.
- Learn from code reviews.
- Continuously improve.

---

# Definition of Done (DoD)

A feature is considered **Done** only when:

- Business requirements are completed.
- Coding standards are followed.
- Validation is implemented.
- Error handling is complete.
- Security is verified.
- Performance is acceptable.
- Tests pass.
- Documentation is updated.
- Code review is approved.
- CI/CD pipeline passes.
- The feature is ready for production.

---

# Final Engineering Principles

Always remember:

- Build for maintainability.
- Prefer simplicity over complexity.
- Write code that others can understand.
- Protect user data.
- Test everything that matters.
- Automate repetitive tasks.
- Review code respectfully.
- Never stop improving.

---

# Engineering Rule

> **Every commit should improve the codebase. Every release should improve the product.**

---

# Summary

Software quality is achieved through consistent engineering practices—not individual effort.

By following this Definition of Done and Engineering Checklist, every team member contributes to building secure, maintainable, scalable, and production-ready applications.

The checklist should be used before every Pull Request, every code review, and every production release.
