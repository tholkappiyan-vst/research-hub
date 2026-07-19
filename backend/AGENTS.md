# AGENTS.md — Backend (research-hub/backend)

Applies only when working inside `backend/`. See root `AGENTS.md` for project-wide rules first.

## Stack
Spring Boot 3.x, Java 21, Spring Data JPA + PostgreSQL, Flyway migrations, Spring Security
(admin only), Bucket4j (guest rate limiting), CommonMark-java (markdown rendering).

## Module layout
```
com.<you>.research
 ├── article/       ArticleController, ArticleService, ArticleRepository, Article entity
 ├── category/
 ├── tag/
 ├── comment/        moderation + is_author_reply flag logic
 ├── suggestion/     status transitions only (pending→accepted/rejected) — NEVER mutates Article
 ├── engagement/     Like, Bookmark (guest_id-scoped, unique constraints prevent dupes)
 ├── newsletter/
 ├── admin/          auth, 2FA, editor endpoints — all under /api/admin/**
 ├── security/       SecurityConfig — public read endpoints permitAll, admin authenticated
 └── common/         Bucket4j rate limiting, guest identity resolver (reads X-Guest-Id header)
```

## Data model
Full entity definitions in `docs/specification.md` §9: Article, Category, Tag, Comment,
Like, Bookmark, Suggestion, NewsletterSubscriber, Admin. Use Flyway migrations for every
schema change — never hand-edit the DB.

## Security config shape
- `/api/public/**` → `permitAll()`. Write endpoints on this path (POST comment/like/suggestion)
  are NOT behind Spring Security auth — they go through the Bucket4j + guest-id filter instead.
- `/api/admin/**` → `authenticated()`. This is the only auth-gated surface in the whole app.
- CORS restricted to the frontend origin only.

## Guest identity
- Readers are never Spring Security principals. There is no reader User entity/login.
- Every public write endpoint expects an `X-Guest-Id` header (UUID, validated for shape only)
  plus IP-based Bucket4j rate limiting (e.g. 5 comments/min, 1 like per article per guest_id).

## Suggestions — critical rule
`SuggestionService` may only ever update `Suggestion.status` and `admin_note`. It must never
have write access to `Article.content`. If a task seems to require the suggestion flow to
touch article content, stop and flag it — that would violate the core product model.

## Do not
- Do not add a reader-facing registration/login endpoint or User entity for readers.
- Do not let comment/suggestion writes require authentication — only rate limiting.
