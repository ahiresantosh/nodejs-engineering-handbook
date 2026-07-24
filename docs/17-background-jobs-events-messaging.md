# Chapter 17 – Background Jobs, Events & Messaging

## Purpose

Not every task should be executed during an API request.

Background jobs and messaging allow applications to:

- Respond faster
- Handle long-running tasks
- Improve scalability
- Increase reliability
- Decouple different parts of the system

Use asynchronous processing whenever immediate user feedback is not required.

---

# Synchronous vs Asynchronous Processing

### Synchronous

The client waits until every operation is completed.

```text
Client
   │
   ▼
API
   │
   ▼
Save Order
   │
   ▼
Send Email
   │
   ▼
Generate Invoice
   │
   ▼
Response
```

Response time becomes slower.

---

### Asynchronous

The API completes quickly and background workers handle remaining tasks.

```text
Client
   │
   ▼
API
   │
   ▼
Save Order
   │
   ▼
Return Response
   │
   ▼
Queue
   │
   ▼
Worker
   │
   ├── Send Email
   ├── Generate Invoice
   └── Notify Customer
```

Users receive faster responses.

---

# When to Use Background Jobs

Move these operations out of the request cycle:

- Sending emails
- SMS notifications
- Push notifications
- Report generation
- File processing
- Image resizing
- Data synchronization
- PDF generation
- Backup tasks

Keep API requests focused on completing essential business operations.

---

# What is a Queue?

A queue temporarily stores tasks until a worker processes them.

```text
Application
      │
      ▼
Job Queue
      │
      ▼
Worker
      │
      ▼
Task Completed
```

Queues help distribute work efficiently.

---

# Worker

A worker continuously listens for new jobs.

Responsibilities:

- Process queued tasks
- Retry failed jobs
- Log failures
- Update job status

Workers should be independent of the API server.

---

# Event-Driven Architecture

An event represents something that has happened.

Examples:

- UserRegistered
- OrderCreated
- DeviceConnected
- PaymentCompleted

Events allow different parts of the system to react independently.

---

# Event Flow

```text
Create Order
      │
      ▼
Publish OrderCreated Event
      │
      ├──────────────┐
      ▼              ▼
Inventory      Notification
      │              │
      ▼              ▼
Update Stock   Send Email
```

Multiple consumers can react to the same event without changing the original service.

---

# Commands vs Events

| Command | Event |
|----------|-------|
| Requests an action | Announces something happened |
| One receiver | Multiple receivers |
| "Create Order" | "Order Created" |
| Expects execution | Used for notifications and reactions |

Use commands to perform actions and events to notify interested components.

---

# Retry Failed Jobs

Some failures are temporary.

Examples:

- Email server unavailable
- Network timeout
- External API unavailable

Instead of failing immediately:

```text
Attempt 1

↓

Retry

↓

Retry

↓

Dead Letter Queue
```

Use controlled retries with limits.

---

# Dead Letter Queue (DLQ)

A Dead Letter Queue stores jobs that cannot be processed successfully.

Benefits:

- Prevents infinite retry loops
- Enables investigation
- Allows manual recovery

Never discard failed jobs silently.

---

# Idempotency

A job may execute more than once.

Ensure repeated execution produces the same result.

Example:

### Good

```text
Update Order Status to "Completed"
```

Running it twice has no additional effect.

### Avoid

Creating duplicate invoices or sending duplicate emails.

Design background jobs to be idempotent.

---

# Scheduling Jobs

Some tasks run on a schedule instead of responding to requests.

Examples:

- Daily reports
- Cleanup jobs
- Database backups
- Token cleanup
- Data synchronization

Schedule these tasks using a job scheduler or cron.

---

# Monitoring Jobs

Monitor:

- Queue length
- Processing time
- Failed jobs
- Retry count
- Worker health

Background processing should be observable.

---

# Common Mistakes

## ❌ Long Tasks Inside API Requests

Move them to background jobs.

---

## ❌ Infinite Retries

Limit retries and use a Dead Letter Queue.

---

## ❌ Ignoring Failed Jobs

Every failed job should be logged and monitored.

---

## ❌ Duplicate Processing

Design jobs to be idempotent.

---

## ❌ Tight Coupling

Use events to reduce dependencies between modules.

---

# Background Job Checklist

Before implementing asynchronous processing:

- [ ] Task identified as long-running
- [ ] Queue implemented
- [ ] Worker created
- [ ] Retries configured
- [ ] Dead Letter Queue enabled
- [ ] Job is idempotent
- [ ] Failures logged
- [ ] Monitoring configured

---

# Best Practices

- Keep API requests fast.
- Process long-running tasks asynchronously.
- Use queues for reliable processing.
- Publish events instead of tightly coupling services.
- Limit retries.
- Monitor background workers.
- Design jobs to be idempotent.

---

# Engineering Rule

> **If a task does not need to finish before responding to the client, move it to a background job or event-driven workflow.**

---

# Summary

Background jobs, events, and messaging improve application responsiveness, scalability, and reliability.

By separating long-running tasks from API requests, using queues for asynchronous processing, and designing idempotent workers, applications can handle increasing workloads while remaining fast and maintainable.
