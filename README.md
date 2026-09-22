# LiveList

Track concerts with friends. See what's coming up, what your friends are
going to, and centralize tickets scattered across multiple platforms —
all in one clean, concert-only agenda.

## Overview

LiveList replaces the usual calendar-with-empty-days view with a
**concert-only timeline**: only days with a show actually show up.
Concerts can be added manually or imported from a ticketing link
(Shotgun, Dice, Ticketmaster), and shared within invite-only spaces so
friends can see what's on each other's radar and join in.

## Status

🚧 In active development. Functional specs are finalized; technical
specs and data model are next. Not deployed yet.

## Features

- **Concert-only agenda** — a vertical timeline showing only days with a
  scheduled concert, grouped by date
- **Spaces** — join a group via an invite link/code; concerts added by
  any member become visible to the whole space
- **Personal + shared view** — see your own schedule (`My Schedule`),
  everything happening in your space (`Group Schedule`), or past shows
- **Ticket import** — paste a link from Shotgun, Dice, or Ticketmaster to
  auto-fill artist, venue, date, and time (always editable before saving)
- **Manual entry** — add a concert by hand when no link is available
- **Duplicate detection** — when a similar event already exists in the
  space, LiveList suggests merging instead of creating a duplicate
- **Profile** — editable, unique username, with an initials-based avatar
  fallback

See [`functional-specs.md`](./functional-specs.md) for the full
specification.

## Tech stack

- **Framework:** Next.js (App Router) + TypeScript
- **Styling:** Tailwind CSS + shadcn/ui
- **Auth:** external provider (Google) via Auth.js
- **Database:** TBD — to be finalized in the technical specs
- **Deployment:** Vercel

> This section will be updated as backend and data-layer decisions are
> finalized.

## Project structure

```
livelist/
├── app/              # Next.js App Router (pages + API routes)
│   └── api/          # Backend route handlers
├── components/        # UI components
├── lib/               # Shared logic, utilities
├── functional-specs.md
├── CLAUDE.md          # Project context and workflow rules for Claude Code
└── README.md
```

## Getting started

```bash
# Install dependencies
npm install

# Set up environment variables
cp .env.example .env.local
# then fill in the required values

# Run the dev server
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) to view the app.

> Environment variables and full setup instructions will be completed
> once authentication and the database are wired in.

## Development workflow

This project follows a fine-grained branching workflow — one branch per
indivisible unit of work, merged frequently. Full rules are documented in
[`CLAUDE.md`](./CLAUDE.md), which also serves as the project context file
for Claude Code.

Quick summary:
- Branch naming: `feat/`, `fix/`, `refactor/`, `test/`, `chore/`
- Conventional commits (`feat:`, `fix:`, `refactor:`, `test:`, `chore:`)
- No direct commits to `main`
- Every commit should leave the build passing

## Documentation

- [`functional-specs.md`](./functional-specs.md) — what the product does
- `technical-specs.md` — how it's built *(coming soon)*
- [`CLAUDE.md`](./CLAUDE.md) — project conventions and AI-assisted
  development workflow

## License

TBD
