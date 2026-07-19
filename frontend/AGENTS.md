# AGENTS.md — Frontend (research-hub/frontend)

Applies only when working inside `frontend/`. See root `AGENTS.md` for project-wide rules first.

## Stack
Next.js (App Router, TypeScript), Tailwind CSS, shadcn/ui components, Mermaid.js for diagrams,
Shiki for code highlighting. Calls the Spring Boot API in `../backend`.

## Design tokens (see docs/specification.md §3 for full rationale)
- Palette: grayscale + one blue accent (`#2563EB`). No other hues except status chips
  (green/amber/red) reserved strictly for suggestion status — never decorative.
- Typography: Inter/Geist Sans for headings, same family 400wt for body (17–18px, 1.7 line-height),
  JetBrains Mono/Geist Mono for code.
- Cards: 1px hairline border, shadow only on hover. No glassmorphism on cards — reserve
  `backdrop-filter: blur()` for sticky nav and modals only.
- Radius: 12px cards, 8px inputs/buttons, 999px pills.
- Motion: 150–250ms ease-out for hover/press, 300–400ms for route transitions. Respect
  `prefers-reduced-motion`.

## Guest identity (no login)
- Generate a `guest_id` (UUID) on first interaction, persist in localStorage + cookie.
- Send it as a header (e.g. `X-Guest-Id`) on every write request (like, bookmark, comment,
  suggestion). Never build a login/signup UI for readers.
- Comment/suggestion forms collect Name (required) + Email (optional, for reply notifications)
  as free text — not an account.

## Pages (see docs/specification.md §5–§8 for full detail per page)
- `/` — landing (hero, featured/latest/trending research, categories, about, newsletter)
- `/research` — filterable grid, sticky filter bar, Load More pagination (not infinite scroll)
- `/categories/[slug]` — category hub with its own hero/description
- `/research/[slug]` — article page: sticky TOC, scroll progress bar, Like/Bookmark/Share,
  comments (nested, author replies highlighted), and the "Suggest Improvement" modal
- Admin pages (`/admin/**`) use a visually separate shell — do not reuse the public nav/theme.

## Do not
- Do not add any reader login/signup/profile pages.
- Do not let the "Suggest Improvement" flow write to article content — it only POSTs a
  Suggestion object.
