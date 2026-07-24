# Chapter 7 – REST API Standards

## Purpose

A REST API should be:

- Consistent
- Predictable
- Easy to understand
- Easy to consume
- Backward compatible

Following common standards makes APIs easier for developers to use and maintain.

---

# REST Principles

A REST API should:

- Represent resources
- Use standard HTTP methods
- Be stateless
- Return meaningful status codes
- Use JSON for request and response

---

# Resource Naming

Always use **plural nouns** for resources.

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
/orderList
/productData
```

Resources represent **objects**, not actions.

---

# URL Design

Keep URLs simple and meaningful.

### Good

```text
GET    /users
GET    /users/{id}
POST   /users
PUT    /users/{id}
PATCH  /users/{id}
DELETE /users/{id}
```

Avoid verbs in URLs.

### ❌ Bad

```text
/getUser
/createUser
/deleteUser
/updateUser
```

---

# HTTP Methods

| Method | Purpose |
|---------|----------|
| GET | Retrieve data |
| POST | Create resource |
| PUT | Replace entire resource |
| PATCH | Update part of a resource |
| DELETE | Delete resource |

Choose the correct method based on the operation.

---

# HTTP Status Codes

Use standard status codes.

| Code | Meaning |
|------|----------|
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

Avoid always returning **200 OK**.

---

# Request Body

Use JSON.

Example

```json
{
  "name": "John Doe",
  "email": "john@example.com"
}
```

Never send unnecessary fields.

---

# Response Format

Keep responses consistent.

### Success Response

```json
{
  "success": true,
  "data": {
    "id": 1,
    "name": "John Doe"
  }
}
```

### Error Response

```json
{
  "success": false,
  "message": "User not found"
}
```

Clients should receive the same response structure across all APIs.

---

# Query Parameters

Use query parameters for filtering, sorting, and pagination.

Example

```text
GET /users?page=1&pageSize=20

GET /users?status=active

GET /users?sort=name

GET /users?search=john
```

Do not use query parameters to identify resources.

---

# Path Parameters

Use path parameters for unique resources.

Example

```text
GET /users/15

GET /orders/1001
```

---

# API Versioning

Version APIs to avoid breaking existing clients.

Example

```text
/api/v1/users

/api/v2/users
```

Avoid changing existing API behavior without versioning.

---

# Pagination

Never return thousands of records in one request.

Example

```text
GET /users?page=1&pageSize=25
```

Example Response

```json
{
  "success": true,
  "data": [],
  "pagination": {
    "page": 1,
    "pageSize": 25,
    "totalRecords": 240,
    "totalPages": 10
  }
}
```

---

# Sorting

Allow clients to control sorting.

Example

```text
GET /users?sort=name

GET /users?sort=-createdAt
```

Convention:

- Ascending → `sort=name`
- Descending → `sort=-createdAt`

---

# Filtering

Filtering reduces unnecessary data transfer.

Example

```text
GET /users?status=active

GET /orders?customerId=100

GET /devices?online=true
```

---

# Searching

Use a dedicated search parameter.

Example

```text
GET /users?search=john
```

Avoid creating separate search endpoints.

---

# Idempotency

Some HTTP methods should produce the same result even if called multiple times.

| Method | Idempotent |
|---------|------------|
| GET | ✅ |
| PUT | ✅ |
| DELETE | ✅ |
| PATCH | Usually |
| POST | ❌ |

Understanding idempotency helps build reliable APIs.

---

# API Naming

Use consistent naming.

### Resources

```text
/users
/orders
/devices
```

### Fields

```json
firstName

lastName

createdAt

updatedAt
```

Use camelCase for JSON fields.

---

# Error Messages

Provide meaningful error messages.

### Good

```json
{
  "message": "Email already exists"
}
```

### Bad

```json
{
  "message": "Something went wrong"
}
```

Avoid exposing internal implementation details.

---

# Common Mistakes

## ❌ Verbs in URLs

```text
/createUser
```

---

## ❌ Returning Different Response Formats

Every endpoint should follow the same structure.

---

## ❌ Missing Pagination

Large responses slow down applications.

---

## ❌ Ignoring HTTP Status Codes

Return the correct status code.

---

## ❌ Breaking Existing APIs

Always use versioning.

---

# API Design Checklist

Before creating an endpoint:

- [ ] Resource name uses plural noun
- [ ] Correct HTTP method used
- [ ] Correct status code returned
- [ ] Request DTO defined
- [ ] Response DTO defined
- [ ] Validation implemented
- [ ] Pagination supported (if required)
- [ ] Filtering supported (if required)
- [ ] Sorting supported (if required)
- [ ] API documented

---

# Best Practices

- Design APIs around resources.
- Use HTTP methods correctly.
- Keep responses consistent.
- Use meaningful status codes.
- Support pagination.
- Support filtering and sorting.
- Version APIs.
- Never expose internal details.

---

# Engineering Rule

> **An API is a contract. Once published, avoid breaking it. Introduce changes through versioning instead of modifying existing behavior.**

---

# Summary

A well-designed REST API is consistent, predictable, and easy to consume.

By following common standards for URLs, HTTP methods, status codes, request/response formats, and versioning, developers can build APIs that remain maintainable and backward compatible as the application grows.
