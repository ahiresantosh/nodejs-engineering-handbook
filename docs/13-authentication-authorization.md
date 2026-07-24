# Chapter 13 – Authentication & Authorization

## Purpose

Security is one of the most critical aspects of any backend application.

Before allowing access to protected resources, the application must verify:

- Who is making the request? (Authentication)
- What is the user allowed to do? (Authorization)

These checks protect the application from unauthorized access.

---

# Authentication vs Authorization

| Authentication | Authorization |
|---------------|---------------|
| Verifies identity | Verifies permissions |
| "Who are you?" | "What can you do?" |
| Login process | Access control |
| Uses credentials | Uses roles or permissions |

Authentication always happens before Authorization.

---

# Authentication Flow

```text
User Login
      │
      ▼
Validate Credentials
      │
      ▼
Generate Access Token
      │
      ▼
Client Stores Token
      │
      ▼
Client Sends Token
      │
      ▼
Verify Token
      │
      ▼
Protected API
```

---

# Authentication Methods

Common authentication mechanisms:

| Method | Use Case |
|---------|----------|
| JWT | REST APIs |
| OAuth2 | Third-party login |
| OpenID Connect | Enterprise identity |
| API Key | Service-to-Service communication |
| Session-Based | Traditional web applications |

Choose the method that fits your application.

---

# JWT Authentication

JWT (JSON Web Token) is commonly used for stateless APIs.

Typical flow:

```text
Login

↓

Generate JWT

↓

Client Stores JWT

↓

Authorization Header

↓

Protected API
```

Example Header

```text
Authorization: Bearer <access_token>
```

---

# Password Security

Passwords should **never** be stored in plain text.

Always:

- Hash passwords
- Use a strong hashing algorithm
- Compare hashes during login

### ❌ Bad

```text
Password: myPassword123
```

### ✅ Good

```text
Hashed Password:
$2b$10$...
```

---

# Access Tokens

Access tokens should:

- Have a short expiration time
- Contain only required information
- Be digitally signed
- Never contain sensitive data

Example Claims

```text
User ID

Email

Role
```

Avoid storing passwords or secrets inside tokens.

---

# Refresh Tokens

Access tokens expire.

Instead of forcing users to log in again, use Refresh Tokens.

```text
Login
   │
   ▼
Access Token (15 min)

Refresh Token (7 days)

↓

Refresh Access Token

↓

Continue Using Application
```

Store Refresh Tokens securely.

---

# Authorization

Authentication identifies the user.

Authorization checks permissions.

Example

```text
Admin

Can:

Create User

Delete User

View Reports
```

```text
Operator

Can:

View Devices

Update Device

Cannot Delete User
```

---

# Role-Based Access Control (RBAC)

Assign permissions through roles.

Example

```text
Admin

↓

Manager

↓

Operator

↓

Viewer
```

Each role has different permissions.

---

# Permission Checks

Every protected endpoint should verify permissions.

Example

```text
DELETE /users

↓

Admin Only
```

Avoid relying only on the frontend.

---

# Middleware

Authentication and Authorization should be implemented as middleware.

Flow

```text
Request

↓

Authentication

↓

Authorization

↓

Controller
```

Reject unauthorized requests before reaching business logic.

---

# Token Expiration

Tokens should expire automatically.

Benefits

- Limits impact of stolen tokens
- Improves security
- Reduces long-term risk

Avoid tokens that never expire.

---

# Logout

Logging out should invalidate active sessions or refresh tokens.

Do not rely only on removing the token from the client.

---

# Secure Communication

Always use HTTPS.

Never send:

- Passwords
- Tokens
- API Keys

over unsecured HTTP connections.

---

# Common Mistakes

## ❌ Storing Plain Text Passwords

Always store hashed passwords.

---

## ❌ Long-Lived Tokens

Use short-lived Access Tokens.

---

## ❌ Authorization in Frontend Only

The backend must always verify permissions.

---

## ❌ Trusting Client Roles

Never trust values sent by the client.

Always verify roles from authenticated user information.

---

## ❌ Exposing Sensitive Claims

Keep JWT payloads minimal.

---

# Authentication Checklist

Before releasing a feature:

- [ ] Passwords hashed
- [ ] HTTPS enabled
- [ ] Access Tokens expire
- [ ] Refresh Tokens implemented (if applicable)
- [ ] Protected APIs secured
- [ ] Authorization implemented
- [ ] Roles verified
- [ ] Sensitive information removed from tokens

---

# Best Practices

- Authenticate every protected request.
- Authorize every sensitive operation.
- Use HTTPS.
- Store passwords securely.
- Keep Access Tokens short-lived.
- Use Refresh Tokens where appropriate.
- Validate tokens on every request.
- Never trust client-supplied roles.

---

# Engineering Rule

> **Authentication proves identity. Authorization grants permissions. Never assume that an authenticated user is automatically authorized to perform every action.**

---

# Summary

Authentication and Authorization work together to protect backend applications.

By securely managing credentials, validating tokens, enforcing permissions, and protecting sensitive information, applications become significantly more secure and resilient against unauthorized access.
