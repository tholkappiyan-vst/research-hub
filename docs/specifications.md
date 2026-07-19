# AGENTS.md — Research Hub

## What this project is
A single-author technology research publication platform. Full spec: `docs/specification.md`.

**Non-negotiable model — do not deviate from this even if it seems simpler otherwise:**
- Only one person (the author/admin) can create, edit, or publish articles. There is no
  multi-user authoring, no "become an author" flow, no reader-facing publish button.
- Readers never log in. They are identified by an anonymous `guest_id` (UUID, stored client-side
  in a cookie/localStorage), never by an authenticated account. Do not build a reader
  registration/login system — it is explicitly out of scope.
- "Suggest Improvement" submissions from readers NEVER modify article content directly.
  They only ever change a `status` field (pending/accepted/rejected). Only the author,
  manually, in the editor, updates the actual article.

## Repo layout
- `backend/` — Spring Boot 3.x REST API. See `backend/AGENTS.md` for conventions.
- `frontend/` — Next.js (App Router) public site. See `frontend/AGENTS.md` for conventions.
- `docs/specification.md` — source of truth for data model, page specs, design tokens.

## Cross-cutting conventions
- API base path: `/api/v1/...`. Public/reader endpoints under `/api/public/**` (open, rate-limited
  by guest_id + IP). Admin endpoints under `/api/admin/**` (Spring Security authenticated).
- Commit style: Conventional Commits (`feat:`, `fix:`, `chore:`, `docs:`).
- Before implementing a new feature, check `docs/specification.md` first — if something isn't
  covered there, ask rather than inventing a new pattern.
- Never add reader authentication, social login, or "sign up" flows anywhere in the app.
