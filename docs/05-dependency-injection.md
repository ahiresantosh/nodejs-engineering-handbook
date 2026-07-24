# Chapter 5 – Dependency Injection (DI)

## Purpose

Dependency Injection (DI) is a design pattern used to reduce coupling between components.

Instead of creating dependencies inside a class, they are provided from outside.

This makes the code easier to:

- Test
- Maintain
- Extend
- Replace

---

# Why Dependency Injection?

Without Dependency Injection:

- Classes become tightly coupled.
- Unit testing becomes difficult.
- Replacing implementations requires changing multiple files.
- Code becomes harder to maintain.

With Dependency Injection:

- Components are loosely coupled.
- Dependencies can be replaced easily.
- Mock implementations can be injected during testing.
- Code becomes more modular.

---

# Dependency Without Injection

The service creates its own dependency.

```ts
class UserService {

    private repository = new UserRepository();

    async getUser(id: string) {
        return this.repository.findById(id);
    }

}
```

### Problems

- Tightly coupled
- Difficult to test
- Repository cannot be replaced easily

---

# Dependency With Injection

Dependencies are provided from outside.

```ts
class UserService {

    constructor(private repository: UserRepository) {}

    async getUser(id: string) {
        return this.repository.findById(id);
    }

}
```

Now the repository can be replaced without changing the service.

---

# Benefits

| Without DI | With DI |
|------------|---------|
| Tight coupling | Loose coupling |
| Hard to test | Easy to test |
| Difficult to replace | Easy to replace |
| Less reusable | Highly reusable |

---

# Dependency Flow

```text
Application

↓

Create Repository

↓

Inject into Service

↓

Inject Service into Controller
```

Dependencies should be created only once and reused.

---

# Typical Dependency Flow

```text
Controller

↓

Service

↓

Repository

↓

Database
```

The Controller should never create the Service directly.

The Service should never create the Repository directly.

---

# Constructor Injection

Constructor Injection is the preferred approach.

```ts
class UserController {

    constructor(
        private readonly userService: UserService
    ) {}

}
```

Avoid creating dependencies inside methods.

---

# Avoid Manual Object Creation

### ❌ Bad

```ts
const service = new UserService();
```

inside Controller.

---

### ✅ Good

Service is injected through the constructor.

```ts
constructor(
    private readonly userService: UserService
) {}
```

---

# Interface-Based Design

Depend on abstractions instead of concrete implementations.

```text
UserService

↓

UserRepository Interface

↓

PrismaRepository

OR

MongoRepository

OR

MockRepository
```

This allows changing implementations without changing business logic.

---

# Example

```ts
interface UserRepository {

    findById(id: string): Promise<User>;

}
```

Implementation

```ts
class PrismaUserRepository
    implements UserRepository {

}
```

Service

```ts
constructor(
    private repository: UserRepository
) {}
```

---

# Testing Benefits

During testing, inject a mock repository.

```ts
const mockRepository = {

    findById: jest.fn()

};
```

The Service can now be tested without a real database.

---

# Lifetime Guidelines

| Component | Lifetime |
|-----------|----------|
| Controller | Per Request |
| Service | Singleton |
| Repository | Singleton |
| Logger | Singleton |
| Database Client | Singleton |
| Configuration | Singleton |

Avoid creating expensive objects repeatedly.

---

# Common Mistakes

## ❌ Creating Repository Inside Service

```ts
const repository = new UserRepository();
```

---

## ❌ Creating Service Inside Controller

```ts
const service = new UserService();
```

---

## ❌ Multiple Database Connections

Database clients should be shared.

---

## ❌ Static Utility Classes for Everything

Not every class should be static.

Prefer injected services.

---

# Best Practices

- Use constructor injection.
- Depend on interfaces, not implementations.
- Create dependencies once.
- Share expensive resources.
- Inject mock implementations during testing.
- Keep components loosely coupled.

---

# Dependency Checklist

Before submitting code:

- [ ] No `new` keyword inside Controllers
- [ ] No `new` keyword inside Services
- [ ] Dependencies injected
- [ ] Interfaces used where appropriate
- [ ] Easy to replace implementation
- [ ] Easy to unit test

---

# Summary

Dependency Injection improves modularity by separating object creation from business logic.

By injecting dependencies instead of creating them directly, applications become easier to test, maintain, and extend while reducing coupling between components.
