# Chapter 4 – Layered Architecture

## Purpose

A layered architecture separates responsibilities into well-defined layers.

Each layer has a single responsibility and communicates only with the layer below it.

This makes applications easier to understand, test, maintain, and scale.

---

# Why Layered Architecture?

Without clear layers:

- Business logic gets mixed with HTTP code.
- Database queries appear inside controllers.
- Validation is duplicated.
- Testing becomes difficult.
- Code becomes tightly coupled.

A layered architecture solves these problems by clearly defining responsibilities.

---

# Architecture Overview

```text
                Client
                   │
                   ▼
             Route Layer
                   │
                   ▼
          Controller Layer
                   │
                   ▼
          Validation Layer
                   │
                   ▼
            Service Layer
                   │
                   ▼
          Repository Layer
                   │
                   ▼
             Database
```

**Golden Rule**

Each layer communicates only with the layer directly below it.

---

# Layer Responsibilities

| Layer | Responsibility |
|--------|----------------|
| Route | Define endpoints and middleware |
| Controller | Handle HTTP requests and responses |
| Validation | Validate incoming data |
| Service | Business logic |
| Repository | Database operations |
| Database | Data storage |

---

# Route Layer

The Route layer defines the application's API endpoints.

### Responsibilities

- Define URL
- Define HTTP Method
- Apply middleware
- Apply authentication
- Call Controller

### Example

```ts
router.post("/", authMiddleware, userController.create);
router.get("/:id", authMiddleware, userController.findById);
```

### Should NOT

- Business logic
- Validation logic
- Database access

---

# Controller Layer

The Controller is the entry point of the application.

### Responsibilities

- Receive request
- Read parameters
- Validate request
- Call Service
- Return response

### Good Example

```ts
async create(req, res) {

    const dto = req.body;

    const user = await userService.create(dto);

    return res.status(201).json(user);

}
```

Controllers should remain small.

### Should NOT

- SQL queries
- Business rules
- Complex calculations

---

# Validation Layer

Validation protects the application from invalid input.

### Responsibilities

- Required fields
- Data types
- Formats
- Length checks
- Value ranges

Example

```ts
email: valid email

password:
minimum length = 8
```

Validation should happen before business logic.

---

# Service Layer

The Service layer contains the application's business rules.

This is the heart of the application.

### Responsibilities

- Business logic
- Transactions
- External API calls
- Call repositories
- Publish events

### Example

```ts
async create(dto: CreateUserDto) {

    const exists = await repository.exists(dto.email);

    if (exists) {

        throw new ConflictException("Email already exists");

    }

    return repository.create(dto);

}
```

### Should NOT

- Read Express Request
- Return HTTP Responses
- Execute SQL directly

---

# Repository Layer

Repositories interact only with the database.

### Responsibilities

- CRUD
- Queries
- Transactions
- Data persistence

### Example

```ts
async findById(id: string) {

    return prisma.user.findUnique({

        where: { id }

    });

}
```

### Should NOT

- Business logic
- Validation
- HTTP handling

---

# Dependency Direction

Dependencies always move downward.

```text
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

Never reverse this flow.

### Avoid

```text
Repository

↓

Service
```

or

```text
Controller

↓

Database
```

---

# Layer Communication Rules

| Layer | Can Access |
|---------|------------|
| Controller | Service |
| Service | Repository |
| Repository | Database |

Never skip layers.

---

# Why This Matters

Bad

```text
Controller

↓

Database
```

Problems

- Business logic duplicated
- Difficult testing
- Tight coupling

---

Good

```text
Controller

↓

Service

↓

Repository
```

Benefits

- Reusable business logic
- Easy testing
- Clear responsibilities
- Better maintainability

---

# Common Mistakes

## ❌ Fat Controllers

Business logic inside controllers.

---

## ❌ SQL Inside Service

Repositories should own database access.

---

## ❌ Business Logic Inside Repository

Repositories only read and write data.

---

## ❌ Skipping Validation

Always validate requests before reaching the Service layer.

---

## ❌ Returning Database Entities

Return DTOs instead.

---

# Layer Checklist

Before submitting code:

- [ ] Controller is thin
- [ ] Validation added
- [ ] Business logic in Service
- [ ] Repository handles data access
- [ ] No direct database access from Controller
- [ ] DTOs used
- [ ] Error handling implemented

---

# Best Practices

- One responsibility per layer.
- Keep Controllers small.
- Services own business logic.
- Repositories own data access.
- Validate every request.
- Keep dependencies one-way.
- Never bypass architectural layers.

---

# Summary

A layered architecture creates a clean separation of responsibilities.

Following this structure improves readability, maintainability, testability, and scalability while making the codebase easier for every developer to understand.

