# Chapter 9 – API Design Best Practices & Governance

## Purpose

An API is a contract between the backend and its consumers.

A well-designed API should be:

- Consistent
- Predictable
- Easy to use
- Backward compatible
- Easy to maintain

Following common standards ensures that every API in the project behaves the same way.

---

# API Design Principles

Every API should follow these principles:

- Keep URLs simple
- Use REST conventions
- Return consistent responses
- Validate all input
- Use correct HTTP status codes
- Never expose internal implementation

---

# Resource Naming

Use plural nouns for resources.

### ✅ Good

```text
/users
/orders
/products
/devices
```

### ❌ Avoid

```text
/getUsers
/createUser
/deleteOrder
```

Resources represent **objects**, not actions.

---

# Endpoint Design

Design URLs around resources.

```text
GET     /users
GET     /users/{id}

POST    /users

PUT     /users/{id}

PATCH   /users/{id}

DELETE  /users/{id}
```

Avoid deeply nested URLs.

### ❌ Bad

```text
/users/1/orders/2/products/10/details
```

Prefer simpler endpoints.

---

# Request Standards

Use JSON request bodies.

Example

```json
{
    "name": "John",
    "email": "john@example.com"
}
```

Validate every request before processing.

---

# Response Standards

Every endpoint should return a consistent response structure.

Success

```json
{
    "success": true,
    "data": {},
    "message": "Success"
}
```

Error

```json
{
    "success": false,
    "message": "User not found"
}
```

Consistency makes APIs easier to consume.

---

# Pagination

Never return thousands of records.

Use pagination.

Example

```text
GET /users?page=1&pageSize=20
```

Example Response

```json
{
    "data": [],
    "pagination": {
        "page": 1,
        "pageSize": 20,
        "totalRecords": 250,
        "totalPages": 13
    }
}
```

---

# Filtering

Allow filtering using query parameters.

```text
GET /users?status=active

GET /orders?customerId=100
```

---

# Searching

Provide a dedicated search parameter.

```text
GET /users?search=john
```

Avoid creating separate search endpoints.

---

# Sorting

Support sorting where appropriate.

Ascending

```text
GET /users?sort=name
```

Descending

```text
GET /users?sort=-createdAt
```

---

# HTTP Status Codes

Return the correct status code.

| Status | Meaning |
|--------|----------|
| 200 | Success |
| 201 | Created |
| 204 | No Content |
| 400 | Bad Request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 409 | Conflict |
| 422 | Validation Error |
| 500 | Internal Server Error |

Never return **200 OK** for every response.

---

# API Versioning

Version APIs to prevent breaking existing clients.

Example

```text
/api/v1/users

/api/v2/users
```

When introducing breaking changes:

- Create a new version
- Keep old version available
- Deprecate gradually

---

# API Documentation

Every endpoint should be documented.

Include:

- URL
- Method
- Authentication
- Request Body
- Response
- Error Codes

Use OpenAPI (Swagger) whenever possible.

---

# API Deprecation

Do not remove APIs immediately.

Recommended lifecycle:

```text
Release v2

↓

Mark v1 as Deprecated

↓

Notify Clients

↓

Remove v1 Later
```

Give consumers enough time to migrate.

---

# Idempotency

Understand which operations are safe to repeat.

| Method | Idempotent |
|---------|------------|
| GET | ✅ |
| PUT | ✅ |
| DELETE | ✅ |
| PATCH | Usually |
| POST | ❌ |

This is important for retries and distributed systems.

---

# Common Mistakes

## ❌ Different Response Formats

Every endpoint should return the same structure.

---

## ❌ Missing Pagination

Large responses impact performance.

---

## ❌ Breaking Existing APIs

Always introduce a new version.

---

## ❌ Returning Internal Errors

Never expose stack traces.

---

## ❌ Poor Documentation

Undocumented APIs increase support effort.

---

# API Governance Checklist

Before publishing an endpoint:

- [ ] URL follows REST conventions
- [ ] Correct HTTP method used
- [ ] Request DTO created
- [ ] Response DTO created
- [ ] Validation implemented
- [ ] Status codes reviewed
- [ ] Pagination supported
- [ ] Filtering supported
- [ ] Swagger updated
- [ ] Versioning considered

---

# Best Practices

- Keep APIs resource-oriented.
- Use consistent naming.
- Return consistent responses.
- Validate all requests.
- Version breaking changes.
- Document every endpoint.
- Design APIs for long-term compatibility.

---

# Engineering Rule

> **An API is a public contract. Design it carefully because changing it later is expensive.**

---

# Summary

Good API design is more than choosing the correct URL or HTTP method. It is about creating consistent, predictable, and maintainable interfaces that clients can rely on for years.

Following common standards for naming, responses, pagination, versioning, and documentation ensures that APIs remain scalable as the application evolves.
