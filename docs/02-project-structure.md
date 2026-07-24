---

---
title: Chapter 2 – Project Structure & Feature-Based Architecture
version: 1.0
author: Engineering Team
---

# Chapter 2 – Project Structure & Feature-Based Architecture

## Purpose

A well-organized project structure makes applications easier to understand, maintain, and scale.

This guide follows a **Feature-Based Architecture**, where each business feature contains everything it needs. This approach reduces coupling, improves ownership, and makes onboarding easier for new developers.

---

# Why Feature-Based Architecture?

Instead of grouping files by technical layer (controllers, services, repositories), group them by **business feature**.

### ❌ Layer-Based Structure

```text
src/
├── controllers/
├── services/
├── repositories/
├── models/
├── routes/
└── validators/
```

As the project grows, finding related code becomes difficult.

---

### ✅ Feature-Based Structure

```text
src/
├── modules/
│   ├── users/
│   ├── auth/
│   ├── orders/
│   └── devices/
│
├── shared/
├── config/
├── middleware/
├── database/
├── routes/
└── app.ts
```

Each feature is isolated and easier to maintain.

---

# Recommended Project Structure

```text
src/
│
├── modules/
│   ├── users/
│   │   ├── controller/
│   │   ├── service/
│   │   ├── repository/
│   │   ├── dto/
│   │   ├── mapper/
│   │   ├── validator/
│   │   ├── routes/
│   │   ├── types/
│   │   ├── constants/
│   │   ├── users.module.ts
│   │   └── index.ts
│   │
│   └── orders/
│
├── shared/
│   ├── logger/
│   ├── errors/
│   ├── middleware/
│   ├── utils/
│   ├── constants/
│   └── types/
│
├── config/
├── database/
├── routes/
├── app.ts
└── server.ts
```

---

# Request Flow

Every request should follow the same flow.

```text
Client
   │
   ▼
Route
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
Database
```

Each layer has **one responsibility**.

---

# Layer Responsibilities

| Layer | Responsibility |
|--------|----------------|
| Route | Define API endpoint |
| Controller | Receive request and return response |
| Service | Business logic |
| Repository | Database access |
| DTO | API request & response contract |
| Mapper | Convert Entity ↔ DTO |
| Validator | Validate incoming request |

---

# Controller Responsibilities

Controllers should be very small.

### ✅ Should

- Receive HTTP request
- Validate input
- Call Service
- Return HTTP response

### ❌ Should NOT

- Business logic
- SQL queries
- Complex calculations
- External API calls

### Good Example

```ts
async create(req, res) {
    const user = await userService.create(req.body);
    return res.status(201).json(user);
}
```

---

# Service Responsibilities

Service is the heart of the application.

### ✅ Should

- Business rules
- Transactions
- Call repositories
- Call external services
- Publish events

### ❌ Should NOT

- Access Express Request/Response
- Return HTTP status codes
- Write SQL queries directly

---

# Repository Responsibilities

Repository is responsible only for data access.

### ✅ Should

- CRUD operations
- Database queries
- Transactions
- Parameterized queries

### ❌ Should NOT

- Business rules
- Validation
- HTTP logic

---

# DTO Folder

DTOs define the API contract.

Example:

```text
dto/
├── create-user.dto.ts
├── update-user.dto.ts
└── user-response.dto.ts
```

DTOs should never contain business logic.

---

# Mapper Folder

Mappers convert one object into another.

Example:

```text
Database Entity

↓

Mapper

↓

Response DTO
```

Benefits:

- Hide database fields
- Keep API response consistent
- Prevent accidental data exposure

---

# Validator Folder

Validation should be close to the feature.

Example:

```text
validator/
├── create-user.validator.ts
└── update-user.validator.ts
```

Keep validation separate from business logic.

---

# Shared Folder

Use the `shared` folder only for reusable components.

Examples:

- Logger
- Error classes
- Middleware
- Utilities
- Constants

Do **not** place feature-specific code here.

---

# Module Independence

Each module should be self-contained.

Example:

```text
users/

orders/

products/

payments/
```

Each module should be removable with minimal impact on others.

---

# Dependency Rule

Dependencies should always move inward.

```text
Controller

↓

Service

↓

Repository

↓

Database
```

Never reverse this direction.

For example:

❌ Repository calling Service

❌ Controller calling Database directly

---

# Common Mistakes

### ❌ Fat Controllers

Business logic inside controllers.

---

### ❌ Business Logic Inside Repository

Repositories should only access data.

---

### ❌ Shared Folder Becoming a Dumping Ground

Only place truly reusable code inside `shared`.

---

### ❌ Circular Dependencies

Modules should not depend on each other in a loop.

---

# Engineering Checklist

Before creating a new feature:

- [ ] Feature folder created
- [ ] Controller added
- [ ] Service added
- [ ] Repository added
- [ ] DTO created
- [ ] Validator created
- [ ] Mapper created (if required)
- [ ] Routes added
- [ ] Module exported

---

# Best Practices

- Keep modules independent.
- Controllers should be thin.
- Services contain business logic.
- Repositories access only the database.
- Use DTOs for API contracts.
- Keep reusable code inside `shared`.
- Follow the same folder structure across all modules.

---

# Summary

A Feature-Based Architecture improves scalability, readability, and maintainability by keeping all related code together.

Following a consistent structure allows developers to quickly locate code, understand responsibilities, and develop new features without affecting other parts of the application.
