# Chapter 0 – Project Setup & Development Environment

## Purpose

Before writing code, every developer should have a consistent development environment.

This chapter provides a step-by-step guide to set up a production-ready Node.js backend project using TypeScript.

Following the same setup across the team helps eliminate environment-specific issues and improves onboarding.

---

# Prerequisites

Install the following software before creating the project.

| Software                    | Recommended Version | Download                                       |
| --------------------------- | ------------------- | ---------------------------------------------- |
| Node.js (LTS)               | 22.x or later       | https://nodejs.org                             |
| Git                         | Latest              | https://git-scm.com/downloads                  |
| Visual Studio Code          | Latest              | https://code.visualstudio.com                  |
| Docker Desktop *(Optional)* | Latest              | https://www.docker.com/products/docker-desktop |
| PostgreSQL *(Optional)*     | Latest              | https://www.postgresql.org/download            |
| Redis *(Optional)*          | Latest              | https://redis.io/downloads                     |

Verify installation:

```bash
node -v

npm -v

git --version
```

---

# Recommended VS Code Extensions

Install these extensions.

| Extension                    | Purpose                   |
| ---------------------------- | ------------------------- |
| ESLint                       | Code quality              |
| Prettier                     | Code formatting           |
| Error Lens                   | Inline error highlighting |
| Docker                       | Docker support            |
| GitLens                      | Git history               |
| DotENV                       | Environment variables     |
| Thunder Client / REST Client | API testing               |
| Prisma *(Optional)*          | Prisma schema support     |
| Markdown All in One          | Markdown editing          |

---

# Create the Project

Create a new project directory.

```bash
mkdir employee-management-api

cd employee-management-api
```

Initialize a Node.js project.

```bash
npm init -y
```

---

# Initialize Git

```bash
git init
```

Create the first commit after the initial project setup.

---

# Install Runtime Dependencies

```bash
npm install express dotenv cors helmet compression pino pino-http uuid
```

---

# Install Development Dependencies

```bash
npm install -D typescript ts-node tsx @types/node @types/express nodemon eslint prettier eslint-config-prettier eslint-plugin-import jest ts-jest supertest @types/jest
```

> **Note:** Additional libraries (Prisma, Zod, JWT, BullMQ, Redis, etc.) will be introduced in later chapters as they become relevant.

---

# Initialize TypeScript

```bash
npx tsc --init
```

Update your `tsconfig.json` with project-specific settings as covered in later chapters.

---

# Create Folder Structure

```text
employee-management-api/
│
├── src/
│   ├── app.ts
│   ├── server.ts
│   ├── config/
│   ├── common/
│   ├── features/
│   ├── infrastructure/
│   └── shared/
│
├── tests/
├── docs/
├── .env
├── .env.example
├── package.json
├── tsconfig.json
└── README.md
```

This structure will evolve throughout the handbook.

---

# Configure Environment Variables

Create a `.env` file.

```text
PORT=3000

NODE_ENV=development
```

Create a corresponding `.env.example` file without sensitive values.

Never commit secrets to version control.

---

# Add Useful Scripts

Update `package.json`.

```json
{
  "scripts": {
    "dev": "tsx watch src/server.ts",
    "build": "tsc",
    "start": "node dist/server.js",
    "lint": "eslint .",
    "format": "prettier --write .",
    "test": "jest"
  }
}
```

---

# Create a Basic Server

Create `src/server.ts`.

```ts
import express from "express";

const app = express();

app.get("/health", (_, res) => {
  res.send("Application is running.");
});

const PORT = process.env.PORT || 3000;

app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
```

Run the application.

```bash
npm run dev
```

Open:

```text
http://localhost:3000/health
```

Expected response:

```text
Application is running.
```

---

# Git Ignore

Create `.gitignore`.

```text
node_modules

dist

.env

coverage

.vscode
```

---

# Recommended Branch Strategy

```text
main

develop

feature/<feature-name>

bugfix/<bug-name>

hotfix/<issue-name>
```

Examples:

```text
feature/user-authentication

feature/employee-module

bugfix/login-error
```

---

# Coding Tools

Throughout this handbook, the following tools will be configured:

* TypeScript
* ESLint
* Prettier
* Jest
* Swagger/OpenAPI
* Prisma
* PostgreSQL
* Redis
* BullMQ
* Docker
* GitHub Actions

Each tool is introduced in its respective chapter.

---

# Verify the Setup

Before continuing, confirm:

* [ ] Node.js installed
* [ ] Git installed
* [ ] VS Code installed
* [ ] Project created
* [ ] Dependencies installed
* [ ] TypeScript initialized
* [ ] Folder structure created
* [ ] Git repository initialized
* [ ] Server starts successfully
* [ ] `/health` endpoint responds correctly

---

# Common Commands

| Command          | Description              |
| ---------------- | ------------------------ |
| `npm install`    | Install dependencies     |
| `npm run dev`    | Start development server |
| `npm run build`  | Compile TypeScript       |
| `npm start`      | Run compiled application |
| `npm test`       | Run tests                |
| `npm run lint`   | Check code quality       |
| `npm run format` | Format source code       |

---

# Troubleshooting

### Node.js not found

```bash
node -v
```

Ensure Node.js is installed and added to your system PATH.

---

### Port already in use

Change the port in `.env` or stop the process using the current port.

---

### Dependency installation failed

Delete:

```text
node_modules
package-lock.json
```

Then reinstall:

```bash
npm install
```

---

# Best Practices

* Use the latest LTS version of Node.js.
* Keep dependencies up to date.
* Commit code frequently with meaningful messages.
* Never commit secrets or credentials.
* Follow the project folder structure from the beginning.
* Use environment variables for configuration.
* Run linting and tests before committing changes.

---

# Engineering Rule

> **Every developer should be able to clone the repository, install dependencies, and start the application with minimal effort. A consistent setup is the foundation of a productive engineering team.**

---

# Summary

You now have a clean, standardized development environment and a working Node.js project. The following chapters will build upon this foundation to create a production-ready backend application while following the engineering standards defined in this handbook.
