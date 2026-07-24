# Chapter 10 – Validation Standards

## Purpose

Validation ensures that only correct and expected data enters the application.

Every request should be validated before it reaches the business logic.

Good validation improves:

- Security
- Data quality
- Application stability
- User experience

---

# Why Validation Matters

Clients should never be trusted.

A request may come from:

- Web Application
- Mobile App
- Third-party API
- Script
- Hacker

Always validate incoming data.

---

# Validation Flow

```text
Client
   │
   ▼
Request
   │
   ▼
Validation
   │
   ▼
Controller
   │
   ▼
Service
```

Invalid requests should stop immediately.

---

# What Should Be Validated?

Every input should be validated.

Examples

- Required fields
- Data types
- Email format
- Phone number
- String length
- Number range
- Date format
- Enum values

---

# Required Fields

Ensure mandatory fields are provided.

### Example

```json
{
    "name": "John",
    "email": "john@example.com"
}
```

If `email` is missing, return a validation error.

---

# Data Types

Validate the expected type.

Examples

```text
name      → String

age       → Number

isActive  → Boolean

createdAt → Date
```

Never assume the client sends the correct type.

---

# Format Validation

Validate field formats.

Examples

- Email
- UUID
- URL
- Phone Number
- Date

Example

```text
john@example.com
```

Invalid

```text
john@
```

---

# Length Validation

Limit the length of input.

Example

```text
Username

Minimum = 3

Maximum = 30
```

This prevents invalid or malicious input.

---

# Numeric Validation

Example

```text
Age

Minimum = 18

Maximum = 100
```

Always validate ranges where applicable.

---

# Enum Validation

Allow only predefined values.

Example

```text
Status

ACTIVE

INACTIVE

BLOCKED
```

Reject unknown values.

---

# Business Validation

Some validation depends on business rules.

Examples

- Email must be unique
- Order cannot be cancelled after shipping
- Device ID must exist
- User cannot register twice

Business validation belongs in the **Service** layer.

---

# Validation vs Business Rules

| Validation | Business Rule |
|------------|---------------|
| Required field | Email already exists |
| Email format | User already registered |
| Number range | Product out of stock |
| UUID format | Device belongs to another customer |

Validation checks data format.

Business rules check application logic.

---

# Validation Error Response

Return clear error messages.

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

Avoid generic error messages.

---

# Validate Early

Validate requests before reaching business logic.

Good

```text
Request

↓

Validation

↓

Service
```

Avoid

```text
Request

↓

Service

↓

Validation
```

---

# Sanitize Input

Validation verifies data.

Sanitization cleans data.

Examples

Input

```text
"  John  "
```

Sanitized

```text
"John"
```

Other examples

- Trim whitespace
- Normalize email
- Remove unsupported characters

---

# File Validation

When uploading files, validate:

- File type
- File size
- Extension
- Maximum upload limit

Never trust the file extension alone.

---

# Common Mistakes

## ❌ No Validation

Accepting raw client input.

---

## ❌ Validation Inside Controller

Move reusable validation into validators or middleware.

---

## ❌ Business Logic Inside Validators

Validators should not access the database.

---

## ❌ Generic Error Messages

Tell the client what is wrong.

---

## ❌ Returning Multiple Different Error Formats

Keep validation responses consistent.

---

# Validation Checklist

Before processing a request:

- [ ] Required fields checked
- [ ] Data types validated
- [ ] Formats validated
- [ ] Length validated
- [ ] Numeric ranges validated
- [ ] Enum values validated
- [ ] Input sanitized
- [ ] Business validation implemented
- [ ] Consistent error response returned

---

# Best Practices

- Validate every request.
- Fail fast.
- Keep validation reusable.
- Separate validation from business rules.
- Return meaningful error messages.
- Sanitize user input.
- Validate uploaded files.

---

# Engineering Rule

> **Never trust client input. Validate everything before executing business logic.**

---

# Summary

Validation is the first line of defense for every application.

By validating requests early, separating business rules from technical validation, and returning meaningful error messages, applications become more secure, reliable, and easier to maintain.
