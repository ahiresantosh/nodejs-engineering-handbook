# Chapter 6 – Configuration & Environment Management

## Purpose

Configuration allows an application to behave differently across environments without changing the source code.

A well-designed configuration system improves:

- Security
- Maintainability
- Deployment
- Scalability

The application should be configured using **environment variables**, not hardcoded values.

---

# Why Configuration Matters

Imagine deploying the same application to:

- Development
- Testing
- Staging
- Production

Each environment has different values such as:

- Database URL
- API Keys
- JWT Secret
- Redis Host
- Logging Level

The application code should remain the same. Only the configuration should change.

---

# Recommended Folder Structure

```text
src/
├── config/
│   ├── app.config.ts
│   ├── database.config.ts
│   ├── auth.config.ts
│   ├── logger.config.ts
│   └── index.ts
```

Keep all configuration in one place.

---

# Environment Variables

Store environment-specific values outside the code.

Example:

```env
NODE_ENV=development

PORT=3000

DATABASE_URL=postgres://...

JWT_SECRET=my-secret-key

REDIS_HOST=localhost

LOG_LEVEL=info
```

Never commit secrets to Git.

---

# Access Configuration

Instead of reading `process.env` everywhere, create a configuration module.

### ✅ Good

```ts
export const config = {
    port: process.env.PORT,
    jwtSecret: process.env.JWT_SECRET,
};
```

Usage

```ts
config.jwtSecret
```

---

### ❌ Avoid

```ts
process.env.JWT_SECRET
```

throughout the project.

A centralized configuration is easier to maintain.

---

# Configuration Categories

Organize configuration by purpose.

Example

```text
config/

app.config.ts

database.config.ts

auth.config.ts

cache.config.ts

mail.config.ts

storage.config.ts

logger.config.ts
```

Avoid placing everything in one large file.

---

# Validate Configuration

The application should fail immediately if required configuration is missing.

### Example

```ts
if (!process.env.JWT_SECRET) {
    throw new Error("JWT_SECRET is required");
}
```

Failing during startup is much safer than failing during runtime.

---

# Environment Types

Typical environments:

| Environment | Purpose |
|------------|----------|
| Development | Local development |
| Testing | Automated tests |
| Staging | Pre-production |
| Production | Live application |

Use `NODE_ENV` to distinguish environments.

---

# Secrets Management

Sensitive information should never be hardcoded.

Examples:

- JWT Secret
- Database Password
- API Keys
- OAuth Secrets
- Encryption Keys

### ❌ Bad

```ts
const secret = "my-secret";
```

### ✅ Good

```ts
const secret = config.jwtSecret;
```

---

# Default Values

Provide sensible defaults where appropriate.

Example

```ts
const port = process.env.PORT || 3000;
```

Avoid defaults for sensitive values such as passwords or secrets.

---

# Configuration Flow

```text
.env

↓

Configuration Module

↓

Application

↓

Services
```

Services should depend on the configuration module, not directly on environment variables.

---

# Logging Configuration

Configuration can also control application behavior.

Example

```env
LOG_LEVEL=debug
```

Production

```env
LOG_LEVEL=error
```

This avoids changing code for different environments.

---

# Feature Flags

Use configuration to enable or disable features.

Example

```env
ENABLE_EMAIL=true

ENABLE_CACHE=false
```

This allows safe feature rollout without code changes.

---

# Configuration Checklist

Before deploying:

- [ ] All required environment variables exist
- [ ] No secrets are hardcoded
- [ ] Configuration validated during startup
- [ ] Environment-specific values configured
- [ ] Sensitive values stored securely

---

# Common Mistakes

## ❌ Hardcoded Secrets

Never commit passwords or API keys.

---

## ❌ Reading `process.env` Everywhere

Use a centralized configuration module.

---

## ❌ Missing Configuration Validation

Fail fast during application startup.

---

## ❌ Different Configuration Styles

Follow one consistent configuration approach across the project.

---

## ❌ Committing `.env` Files

`.env` files containing secrets should never be committed to version control.

---

# Best Practices

- Keep configuration centralized.
- Use environment variables.
- Validate configuration at startup.
- Organize configuration by domain.
- Never hardcode secrets.
- Keep `.env.example` updated.
- Separate configuration from business logic.

---

# Engineering Rule

> **Application behavior should change through configuration, not by modifying the source code.**

---

# Summary

A centralized configuration system makes applications easier to deploy, more secure, and easier to maintain.

By validating configuration during startup and keeping secrets outside the source code, the application becomes more reliable across all environments.
