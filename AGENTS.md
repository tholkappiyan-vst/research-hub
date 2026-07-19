# AGENTS.md — Research Hub

# Overview

Research Hub is a single-author technology research publication platform.

The complete product specification is located at:

docs/specification.md

This specification is the single source of truth. If any implementation conflicts with the specification, the specification always wins.

---

# Core Product Rules (Non-Negotiable)

## Single Author

- Only one administrator (the author) exists.
- Only the administrator can create, edit, publish, archive, or delete articles.
- Never implement multi-author support.
- Never implement role management.
- Never implement "Become an Author."

---

## Readers

Readers never create accounts.

Readers never authenticate.

Readers are identified only through a guest_id (UUID) stored locally.

Never generate:

- Login page for readers
- Registration
- OAuth
- Google Login
- GitHub Login
- Password reset
- User profile
- User dashboard

These features are permanently out of scope.

---

## Suggestions

Readers may submit suggestions.

Suggestions NEVER modify article content.

Suggestions only change status:

- Pending
- Accepted
- Rejected

Only the administrator edits article content manually.

---

# Technology Stack

## Backend

- Java 21
- Spring Boot 3.5+
- Maven
- Spring Security
- Spring Data JPA
- PostgreSQL
- Flyway
- Bucket4j
- CommonMark
- Docker

## Frontend

- Next.js 15
- App Router
- TypeScript
- Tailwind CSS
- shadcn/ui
- React Query
- React Hook Form
- Zod

---

# Architecture

Backend must follow Layered Architecture.

```
Controller
      ↓
Service
      ↓
Repository
      ↓
Database
```

Rules:

- Controllers contain no business logic.
- Services contain business logic.
- Repositories only access data.
- DTOs must be used.
- Constructor Injection only.
- Validation must use Jakarta Validation.
- Global Exception Handler required.

---

Frontend Architecture

- Reusable Components
- Feature-based folders
- Server Components by default
- Client Components only when necessary
- No duplicated UI components

---

# Database

Use PostgreSQL.

Database schema must be managed using Flyway.

Never modify schema manually.

Every schema change requires a migration.

---

# API Rules

Base URL

```
/api/v1
```

Public APIs

```
/api/public/**
```

must remain open.

Admin APIs

```
/api/admin/**
```

must require authentication.

Follow REST principles.

---

# Security

Only Admin authentication exists.

Readers are never authenticated.

Guest interactions use:

- guest_id
- Cookie
- LocalStorage

Never use JWT for readers.

Rate limiting must use:

- guest_id
- IP Address

---

# Code Quality

Generate production-ready code.

Do not generate placeholder implementations.

Avoid duplicated logic.

Follow SOLID principles.

Keep methods small.

Prefer composition over inheritance.

Write meaningful names.

No dead code.

No commented-out code.

---

# UI Rules

Follow the design tokens defined in docs/specification.md.

Design goals:

- Minimal
- Professional
- Stripe Docs
- Apple Developer
- Linear
- Vercel

Never generate dashboard-style public pages.

Never use excessive gradients.

Never use bright colors.

Prefer whitespace over decoration.

---

# Project Structure

Backend

```
backend/
```

Frontend

```
frontend/
```

Documentation

```
docs/
```

Do not mix frontend and backend code.

---

# Implementation Rules

Before implementing any feature:

1. Read docs/specification.md
2. Verify the requirement exists.
3. If ambiguous, ask instead of inventing.
4. Follow existing architecture.

Never introduce new frameworks without justification.

Never rewrite working architecture.

---

# Development Workflow

Implement features incrementally.

Recommended order:

1. Project Setup
2. Database
3. Security
4. Articles
5. Categories
6. Admin
7. Comments
8. Suggestions
9. Search
10. Newsletter
11. Deployment

---

# Commit Style

Use Conventional Commits.

Examples

```
feat: add article API

fix: correct flyway migration

docs: update specification

refactor: simplify article service

test: add article integration tests
```

---

# Testing

All generated code must compile.

Backend should include:

- Unit Tests
- Integration Tests where appropriate

Frontend should avoid runtime errors.

---

# Final Rule

If the specification and these instructions disagree:

**Always follow docs/specification.md.**

Never invent product behavior that is not described there.
