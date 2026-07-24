# Chapter 11 – Error Handling & Exception Management

## Purpose

Errors are inevitable in every application.

A good error handling strategy ensures that applications:

- Fail gracefully
- Return meaningful error messages
- Protect sensitive information
- Generate useful logs
- Continue operating whenever possible

Error handling should be consistent across the entire application.

---

# Why Error Handling Matters

Without proper error handling:

- Applications crash unexpectedly.
- Clients receive confusing responses.
- Debugging becomes difficult.
- Sensitive information may be exposed.

Good error handling improves reliability and user experience.

---

# Error Flow

```text
Client
   │
   ▼
Request
   │
   ▼
Controller
   │
   ▼
Service
   │
   ▼
Repository
   │
   ▼
Exception
   │
   ▼
Global Error Handler
   │
   ▼
Standard Error Response
```

Every unhandled error should reach the Global Error Handler.

---

# Types of Errors

Understanding the type of error helps determine the correct response.

| Error Type | Example |
|------------|---------|
| Validation Error | Invalid email format |
| Authentication Error | Invalid token |
| Authorization Error | Permission denied |
| Business Error | Product out of stock |
| Database Error | Unique constraint violation |
| External Service Error | Payment gateway unavailable |
| Internal Error | Unexpected exception |

---

# Use Custom Exceptions

Create meaningful exception classes instead of throwing generic errors.

### ✅ Good

```ts
throw new ValidationException("Email is required");
```

```ts
throw new NotFoundException("User not found");
```

```ts
throw new ConflictException("Email already exists");
```

### ❌ Avoid

```ts
throw new Error("Something went wrong");
```

Custom exceptions improve readability and consistency.

---

# Throw Errors Where They Occur

Handle errors at the appropriate layer.

Example

```text
Controller
      │
      ▼
Service
      │
      ▼
Repository
```

If a business rule fails, throw the exception in the **Service**.

If a database query fails, let the Repository throw an appropriate error.

---

# Global Error Handler

Use a single global middleware to handle all unhandled exceptions.

Responsibilities:

- Catch errors
- Log errors
- Convert errors into a standard response
- Hide internal implementation details

Every application should have one centralized error handler.

---

# Standard Error Response

Return a consistent error format.

Example

```json
{
    "success": false,
    "message": "Validation failed",
    "errors": [
        {
            "field": "email",
            "message": "Invalid email format"
        }
    ]
}
```

Every endpoint should follow the same structure.

---

# Use Correct HTTP Status Codes

| Error | Status Code |
|--------|-------------|
| Validation Error | 400 |
| Authentication Failed | 401 |
| Access Denied | 403 |
| Resource Not Found | 404 |
| Duplicate Resource | 409 |
| Validation Failure | 422 |
| Internal Server Error | 500 |

Avoid always returning **500 Internal Server Error**.

---

# Don't Leak Sensitive Information

### ❌ Bad

```json
{
    "message": "SQL Error: relation users does not exist"
}
```

### ✅ Good

```json
{
    "message": "An unexpected error occurred."
}
```

Internal details should only appear in server logs.

---

# Use try...catch Wisely

Use `try...catch` only when you can handle or recover from the error.

### Good

```ts
try {
    await paymentService.process();
} catch (error) {
    logger.error(error);
    throw new PaymentException("Payment failed");
}
```

Avoid wrapping every function in unnecessary `try...catch` blocks.

Let unexpected errors propagate to the Global Error Handler.

---

# Business Exceptions

Business rule violations are expected.

Examples

- Email already exists
- Device already registered
- Order already shipped
- Product out of stock

These are **not** internal server errors.

Return appropriate client-friendly messages.

---

# Logging Errors

Every unexpected error should be logged.

Log:

- Error message
- Stack trace
- Request path
- User ID (if available)
- Trace ID
- Timestamp

Never log passwords or sensitive data.

---

# Recoverable vs Non-Recoverable Errors

| Recoverable | Non-Recoverable |
|-------------|-----------------|
| Validation failure | Out of memory |
| Resource not found | Corrupted configuration |
| Duplicate record | Critical application failure |
| Temporary network issue | Application startup failure |

Recover when possible.

Fail safely when necessary.

---

# Common Mistakes

## ❌ Swallowing Exceptions

```ts
catch (error) {
}
```

Never ignore errors.

---

## ❌ Generic Error Messages

```text
Something went wrong
```

Provide meaningful messages.

---

## ❌ Returning Stack Traces

Never expose internal details to clients.

---

## ❌ Catching Every Exception

Only catch exceptions you can handle.

---

## ❌ Different Error Formats

Keep every API response consistent.

---

# Error Handling Checklist

Before releasing a feature:

- [ ] Validation errors handled
- [ ] Business exceptions handled
- [ ] Correct HTTP status codes returned
- [ ] Global error handler configured
- [ ] Sensitive information hidden
- [ ] Errors logged
- [ ] Standard response format followed

---

# Best Practices

- Use custom exception classes.
- Handle errors centrally.
- Return consistent error responses.
- Use meaningful messages.
- Log unexpected errors.
- Protect sensitive information.
- Use correct HTTP status codes.

---

# Engineering Rule

> **Catch an exception only if you can recover from it. Otherwise, let it propagate to the Global Error Handler.**

---

# Summary

Error handling is not about preventing failures—it is about handling failures in a controlled and predictable way.

By using custom exceptions, centralized error handling, consistent responses, and meaningful logging, applications become easier to debug, more secure, and more reliable in production.
