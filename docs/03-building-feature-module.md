# Chapter 3 – Building a Feature Module

## Purpose

A feature module contains everything required for a single business capability.

Instead of spreading files across multiple folders, keep all related files together. This improves maintainability, ownership, and scalability.

---

# What is a Feature Module?

A feature module represents one business domain.

Examples:

- Users
- Authentication
- Orders
- Products
- Devices
- Payments

Each module should be independent and self-contained.

---

# Recommended Module Structure

```text
modules/
└── users/
    ├── controller/
    │   └── user.controller.ts
    │
    ├── service/
    │   └── user.service.ts
    │
    ├── repository/
    │   └── user.repository.ts
    │
    ├── dto/
    │   ├── create-user.dto.ts
    │   ├── update-user.dto.ts
    │   └── user-response.dto.ts
    │
    ├── validator/
    │   └── user.validator.ts
    │
    ├── mapper/
    │   └── user.mapper.ts
    │
    ├── routes/
    │   └── user.routes.ts
    │
    ├── types/
    ├── constants/
    ├── index.ts
    └── user.module.ts
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
Validation
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

Business logic should only exist inside the **Service** layer.

---

# Step 1 – Create Routes

Routes expose API endpoints.

### Responsibilities

- Define endpoint
- Apply middleware
- Apply authentication
- Call controller

### Example

#### File : src/users/routes/user.routers.ts
```ts
import { Router } from "express";
import { userController } from "../controller/user.controller.js";

const router = Router();

// Placeholder for authMiddleware. In a real application, you would import it from your shared middleware folder.
const authMiddleware = (req: any, res: any, next: any) => next();

router.post("/", authMiddleware, userController.create);
router.get("/:id", authMiddleware, userController.findById);

export default router;

```

Routes should never contain business logic.

---

# Step 2 – Create Controller

Controllers receive HTTP requests and return HTTP responses.

### Responsibilities

- Read request
- Validate input
- Call service
- Return response

### Example

#### File : src/users/controller/user.controller.ts
```ts
import type { Request, Response, NextFunction } from "express";

export const userController = {
  create: async (req: Request, res: Response, next: NextFunction): Promise<void> => {
    try {
      res.status(201).json({ message: "User created" });
    } catch (error) {
      next(error);
    }
  },
  findById: async (req: Request, res: Response, next: NextFunction): Promise<void> => {
    try {
      res.status(200).json({ message: "User found" });
    } catch (error) {
      next(error);
    }
  }
};

```

Controllers should remain small and readable.

---

# Step 3 – Validate Request

Validate incoming data before reaching business logic.

### Good

```ts
{
    "name": "John",
    "email": "john@example.com"
}
```

### Invalid

```ts
{
    "name": "",
    "email": "abc"
}
```

Reject invalid requests immediately.

---

# Step 4 – Implement Service

The Service contains all business rules.

### Responsibilities

- Business logic
- Validation against business rules
- Transactions
- Call repositories
- Call external services
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

---

# Step 5 – Repository

Repositories communicate with the database.

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

Repositories should never contain business logic.

---

# Step 6 – Mapper

Never expose database entities directly.

Use mappers to convert them.

```text
Database Entity

↓

Mapper

↓

Response DTO
```

Example

```ts
return UserMapper.toResponse(user);
```

Benefits

- Hide internal fields
- Keep response consistent
- Prevent accidental data leakage

---

# Step 7 – Return Response

Always return DTOs.

Avoid

```ts
return userEntity;
```

Prefer

```ts
return userResponseDto;
```

---

# Module Responsibilities

| Component | Responsibility |
|-----------|----------------|
| Route | API endpoints |
| Controller | HTTP handling |
| Validator | Input validation |
| Service | Business logic |
| Repository | Database access |
| Mapper | Entity ↔ DTO |
| DTO | API contract |

---

# Module Development Checklist

When creating a new feature:

- [ ] Create module folder
- [ ] Add routes
- [ ] Create controller
- [ ] Create DTOs
- [ ] Add validation
- [ ] Implement service
- [ ] Implement repository
- [ ] Create mapper
- [ ] Register routes
- [ ] Add tests

---

# Common Mistakes

## ❌ Fat Controllers

Business logic inside controllers.

---

## ❌ Returning Database Entities

Always return DTOs.

---

## ❌ Skipping Validation

Never trust client input.

---

## ❌ Business Logic in Repository

Repositories should only interact with the database.

---

## ❌ Duplicate Business Logic

Keep business rules inside the Service layer.

---

# Best Practices

- One module = One business capability.
- Keep modules independent.
- Keep controllers thin.
- Services own business logic.
- Repositories only access data.
- Validate every request.
- Return DTOs instead of entities.

---

# Summary

A feature module groups everything required for one business capability into a single location.

By following a consistent structure, developers can easily understand the codebase, add new features quickly, and maintain production-quality applications as the project grows.
