# Chapter 12 – Logging & Observability

## Purpose

Logging helps developers understand what the application is doing.

Observability goes one step further by helping teams answer:

- What happened?
- When did it happen?
- Why did it happen?
- Who was affected?
- How can we fix it?

A production application should provide enough information to diagnose problems without reproducing them locally.

---

# What is Logging?

Logging is the process of recording important events during application execution.

Examples:

- User logged in
- Order created
- Device connected
- Payment completed
- Error occurred

Logs help developers troubleshoot issues and monitor system behavior.

---

# What is Observability?

Observability is the ability to understand the internal state of a system using:

- Logs
- Metrics
- Traces

```text
              Observability

      ┌────────┬────────┬────────┐
      │        │        │
      ▼        ▼        ▼
    Logs    Metrics   Traces
```

Together, these provide a complete picture of application health.

---

# Log Levels

Use appropriate log levels.

| Level | Purpose |
|--------|---------|
| DEBUG | Detailed debugging information |
| INFO | Normal application events |
| WARN | Unexpected but recoverable situations |
| ERROR | Failures requiring attention |

Example:

```text
INFO  - User logged in

WARN  - Retry attempt for external API

ERROR - Database connection failed
```

Avoid using the same level for every log.

---

# What Should Be Logged?

Examples:

- Application startup
- User authentication
- API requests
- External API calls
- Database failures
- Business events
- Background jobs
- Unexpected exceptions

Log events that help diagnose problems.

---

# What Should NOT Be Logged?

Never log sensitive information.

Examples:

- Passwords
- JWT Tokens
- API Keys
- Credit Card Numbers
- OTPs
- Personal Identifiable Information (PII)

### ❌ Bad

```text
Password: myPassword123
```

### ✅ Good

```text
User login successful.
```

---

# Structured Logging

Avoid plain text logs.

### ❌ Bad

```text
User logged in.
```

### ✅ Good

```json
{
    "level": "INFO",
    "message": "User logged in",
    "userId": "123",
    "timestamp": "2026-07-23T10:30:00Z"
}
```

Structured logs are easier to search and analyze.

---

# Correlation (Trace ID)

Every request should have a unique identifier.

Example:

```text
Request

↓

Trace ID

↓

All Logs
```

Example

```text
Trace ID: 7fd8a1e5-3d6f-4a0f-b7c8-9f1b2c3d4e5f
```

This helps trace a request across multiple services.

---

# Logging Flow

```text
Client Request
      │
      ▼
Generate Trace ID
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
Response
```

The same Trace ID should appear in every log for that request.

---

# Metrics

Metrics measure application performance over time.

Examples:

- Total requests
- Active users
- CPU usage
- Memory usage
- Error rate
- Response time
- Database latency

Metrics help identify trends before failures occur.

---

# Tracing

Tracing shows the journey of a request through the application.

```text
API Request
      │
      ▼
Authentication
      │
      ▼
Business Logic
      │
      ▼
Database
      │
      ▼
External API
      │
      ▼
Response
```

Tracing is valuable in distributed systems and microservices.

---

# Logging Exceptions

Unexpected exceptions should always be logged.

Include:

- Error message
- Stack trace
- Trace ID
- Request path
- User ID (if available)
- Timestamp

This information helps diagnose production issues quickly.

---

# Logging Best Practices

- Log meaningful events.
- Use structured logs.
- Include Trace ID.
- Use appropriate log levels.
- Keep messages clear and concise.
- Avoid duplicate logs.

---

# Common Mistakes

## ❌ Using console.log()

Use a proper logging library instead.

---

## ❌ Logging Sensitive Information

Protect user privacy and security.

---

## ❌ Logging Too Much

Excessive logging increases storage costs and makes troubleshooting harder.

---

## ❌ Logging Too Little

Missing logs make production issues difficult to diagnose.

---

## ❌ Inconsistent Log Format

Every service should follow the same logging structure.

---

# Logging Checklist

Before deploying:

- [ ] Structured logging implemented
- [ ] Log levels used correctly
- [ ] Sensitive data removed
- [ ] Trace ID included
- [ ] Errors logged
- [ ] Important business events logged
- [ ] Metrics collected
- [ ] Request tracing enabled

---

# Best Practices

- Use structured logging.
- Generate a Trace ID for every request.
- Log important business events.
- Avoid logging secrets.
- Monitor application metrics.
- Enable request tracing.
- Keep logging consistent across all services.

---

# Engineering Rule

> **Logs should help answer what happened, when it happened, and why it happened—without exposing sensitive information.**

---

# Summary

Logging and observability are essential for operating production systems.

By using structured logs, appropriate log levels, metrics, and request tracing, developers can quickly diagnose issues, monitor application health, and improve system reliability.
