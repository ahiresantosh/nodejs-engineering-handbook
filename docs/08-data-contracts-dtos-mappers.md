# Chapter 8 – Data Contracts (DTOs, Mappers & API Standards)

## Purpose

An API is a contract between the backend and its consumers.

A Data Transfer Object (DTO) defines that contract.

Using DTOs ensures:

- Consistent API responses
- Better validation
- Better security
- Easier maintenance
- Separation between database models and API models

---

# What is a DTO?

A DTO (Data Transfer Object) defines the structure of data exchanged between the client and the server.

It is **not** a database model.

Example:

```text
Client

↓

Request DTO

↓

Service

↓

Database Entity

↓

Response DTO

↓

Client
```

---

# Why Use DTOs?

Without DTOs:

- Database structure is exposed.
- Internal fields may leak.
- Different APIs return different formats.
- Breaking database changes affect clients.

With DTOs:

- API contract remains stable.
- Internal implementation stays hidden.
- Easier API versioning.
- Better validation.

---

# DTO Types

A feature typically contains multiple DTOs.

```text
dto/
├── create-user.dto.ts
├── update-user.dto.ts
├── user-response.dto.ts
├── login.dto.ts
└── change-password.dto.ts
```

Each DTO has one responsibility.

---

# Request DTO

Defines what the client is allowed to send.

Example

```ts
export interface CreateUserDto {

    name: string;

    email: string;

    password: string;

}
```

Only include required fields.

---

# Response DTO

Defines what the client receives.

Example

```ts
export interface UserResponseDto {

    id: string;

    name: string;

    email: string;

}
```

Never expose internal fields.

---

# Never Return Database Entities

Database Entity

```ts
User {

    id

    email

    password

    createdAt

    updatedAt

}
```

Returning this directly exposes unnecessary information.

Instead

```ts
UserResponseDto {

    id

    email

}
```

---

# Mapper

A Mapper converts one object into another.

```text
Database Entity

↓

Mapper

↓

Response DTO
```

Example

```ts
export class UserMapper {

    static toResponse(user: User): UserResponseDto {

        return {

            id: user.id,

            name: user.name,

            email: user.email

        };

    }

}
```

---

# Why Use Mappers?

Benefits

- Hide internal fields
- Consistent API responses
- Easier refactoring
- Reusable conversion logic

---

# Mapping Flow

```text
Request DTO

↓

Service

↓

Repository

↓

Entity

↓

Mapper

↓

Response DTO
```

Every response should pass through a mapper.

---

# Sensitive Fields

Never expose:

- Password
- Salt
- Secret Key
- Internal IDs
- Audit Information
- Tokens

Example

### ❌ Bad

```json
{
    "password": "hashed-password"
}
```

### ✅ Good

```json
{
    "id": 1,
    "name": "John"
}
```

---

# Validation vs DTO

DTOs define the data structure.

Validators verify the data.

Example

DTO

```ts
email: string
```

Validator

```ts
Must be valid email

Must not be empty
```

Keep these responsibilities separate.

---

# Naming Convention

Use consistent names.

```text
CreateUserDto

UpdateUserDto

LoginDto

UserResponseDto

DeviceTelemetryDto
```

Avoid generic names like:

```text
Request

Response

Data
```

---

# API Contract Stability

Clients depend on DTOs.

Changing a DTO can break applications.

Instead of changing an existing DTO:

- Add a new version
- Introduce optional fields
- Create a new response contract

---

# Nested DTOs

Complex responses can contain nested DTOs.

Example

```json
{
    "id": 1,
    "name": "John",
    "address": {
        "city": "Ahmedabad",
        "country": "India"
    }
}
```

Create separate DTOs.

```text
AddressDto

↓

UserResponseDto
```

Avoid deeply nested structures unless necessary.

---

# Common Mistakes

## ❌ Returning ORM Models

Always return Response DTOs.

---

## ❌ Business Logic Inside Mapper

Mappers only transform data.

---

## ❌ Validation Inside DTO

Keep validation separate.

---

## ❌ Duplicate DTOs

Reuse DTOs when appropriate.

---

## ❌ Exposing Sensitive Fields

Always review API responses before returning them.

---

# DTO Checklist

Before exposing an API:

- [ ] Request DTO created
- [ ] Response DTO created
- [ ] Validation added
- [ ] Mapper implemented
- [ ] Sensitive fields removed
- [ ] Consistent naming followed
- [ ] API contract reviewed

---

# Best Practices

- Never expose database entities.
- Use Request DTOs for input.
- Use Response DTOs for output.
- Keep DTOs simple.
- Keep Mappers focused on transformation only.
- Keep validation separate.
- Maintain backward compatibility.

---

# Engineering Rule

> **Database models are internal implementation details. APIs should expose only DTOs, never entities.**

---

# Summary

DTOs define a stable contract between the backend and its consumers.

By using Request DTOs, Response DTOs, and Mappers, applications become more secure, maintainable, and resilient to internal changes while keeping API responses clean and consistent.
