# Research Hub

A personal technology research publication platform — single-author, no reader login required.

Readers can read, like, bookmark, comment, suggest improvements, follow updates, and share.
Only the site owner can publish or edit articles. Suggestions never directly modify content —
they're reviewed and, if accepted, manually incorporated by the author.

## Structure

```
research-hub/
├── docs/
│   └── specification.md   Full product/UX/architecture spec
├── frontend/               Next.js app (public site)
├── backend/                Spring Boot API
└── AGENTS.md               Shared conventions for AI coding agents
```

## Stack

- **Backend:** Spring Boot 3.x (Java 21), PostgreSQL, Flyway, Spring Security, Bucket4j
- **Frontend:** Next.js (App Router), Tailwind CSS, shadcn/ui
- See `docs/specification.md` for the full design and architecture rationale.

## Getting Started

### Backend
```bash
cd backend
./mvnw spring-boot:run
```

### Frontend
```bash
cd frontend
npm install
npm run dev
```

## License

See `LICENSE`.
