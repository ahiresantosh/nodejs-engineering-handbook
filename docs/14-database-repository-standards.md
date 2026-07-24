# Chapter 14 – Database & Repository Standards

## Purpose

The Repository layer is responsible for all database interactions.

It acts as a bridge between the application's business logic and the database.

A well-designed Repository layer provides:

- Clean separation of concerns
- Easier testing
- Better maintainability
- Database independence
- Reusable data access logic

---

# Why Use a Repository?

Without a Repository layer:

- SQL queries appear everywhere.
- Business logic mixes with database code.
- Testing becomes difficult.
- Changing the database technology is expensive.

A Repository centralizes all data access.

---

# Data Access Flow

```text
Client
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

Business logic should never communicate directly with the database.

---

# Repository Responsibilities

A Repository should:

- Read data
- Insert data
- Update data
- Delete data
- Execute database queries
- Manage transactions (where appropriate)

A Repository should **not** contain business logic.

---

# Service vs Repository

| Service | Repository |
|----------|------------|
| Business rules | Database operations |
| Validation against business rules | CRUD operations |
| Calls multiple repositories | Executes queries |
| Coordinates transactions | Maps data to/from database |

Keep these responsibilities separate.

---

# Example

### Service

```ts
async createUser(dto: CreateUserDto) {

    const exists = await repository.exists(dto.email);

    if (exists) {
        throw new ConflictException("Email already exists");
    }

    return repository.create(dto);

}
```

### Repository

```ts
async create(dto: CreateUserDto) {

    return prisma.user.create({
        data: dto
    });

}
```

The Service decides **what** should happen.

The Repository decides **how** data is stored.

---

# Repository Structure

```text
repository/

user.repository.ts

order.repository.ts

device.repository.ts
```

Each Repository should manage one aggregate or entity.

---

# Naming Convention

Use clear and consistent method names.

### Good

```text
findById()

findByEmail()

create()

update()

delete()

exists()
```

Avoid generic names.

### Avoid

```text
get()

saveData()

process()
```

---

# Transactions

Use transactions when multiple operations must succeed or fail together.

Example

```text
Create Order

↓

Reduce Inventory

↓

Create Invoice

↓

Commit
```

If one operation fails, rollback the entire transaction.

---

# Query Optimization

Retrieve only the required data.

### ❌ Bad

```text
SELECT *
```

### ✅ Good

Select only the required columns.

Reducing unnecessary data improves performance.

---

# Pagination

Never load thousands of records into memory.

Example

```text
GET /users?page=1&pageSize=20
```

Large datasets should always be paginated.

---

# Filtering

Support filtering inside the Repository.

Example

```text
status = ACTIVE

createdAfter = 2026-01-01
```

Avoid filtering large datasets in application memory.

---

# Avoid N+1 Queries

### ❌ Bad

```text
Load Users

↓

For each User

↓

Load Orders
```

This creates many database calls.

### ✅ Good

Use joins or eager loading where appropriate.

Reduce unnecessary round trips.

---

# Database Constraints

Use database constraints to protect data integrity.

Examples

- Primary Key
- Foreign Key
- Unique Constraint
- Not Null
- Check Constraint

Do not rely only on application validation.

---

# Soft Delete vs Hard Delete

### Soft Delete

Mark the record as deleted.

```text
deletedAt = timestamp
```

Benefits:

- Data recovery
- Audit history

---

### Hard Delete

Permanently removes the record.

Use only when required.

---

# Indexing

Indexes improve query performance.

Common candidates:

- Email
- Username
- Device ID
- Foreign Keys
- Frequently searched columns

Avoid unnecessary indexes, as they slow down inserts and updates.

---

# Error Handling

Convert database errors into meaningful business exceptions.

Example

```text
Unique Constraint

↓

ConflictException
```

Avoid exposing raw database errors to clients.

---

# Common Mistakes

## ❌ SQL Inside Controller

Database access belongs only in the Repository.

---

## ❌ Business Logic Inside Repository

Repositories should not make business decisions.

---

## ❌ Returning Entire Tables

Always paginate large datasets.

---

## ❌ Ignoring Transactions

Use transactions for related operations.

---

## ❌ Missing Indexes

Frequently searched columns should be indexed.

---

# Repository Checklist

Before merging code:

- [ ] Repository created
- [ ] No business logic inside Repository
- [ ] Queries optimized
- [ ] Pagination implemented
- [ ] Filtering supported
- [ ] Transactions used where required
- [ ] Database constraints considered
- [ ] Proper indexes created
- [ ] Errors handled correctly

---

# Best Practices

- Keep Repositories focused on data access.
- Keep Services focused on business logic.
- Use transactions when necessary.
- Optimize queries.
- Paginate large datasets.
- Add indexes carefully.
- Protect data integrity with database constraints.
- Never expose raw database errors.

---

# Engineering Rule

> **A Repository answers "How do we store and retrieve data?" A Service answers "What business rule should be applied?" Never mix these responsibilities.**

---

# Summary

The Repository layer provides a clean separation between business logic and data persistence.

By keeping repositories focused on database operations, optimizing queries, using transactions, and enforcing data integrity, applications become easier to maintain, easier to test, and more scalable as data volume grows.
