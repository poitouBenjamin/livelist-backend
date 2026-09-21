# LiveList — Project context for Claude Code

## Description

LiveList is a web app for tracking concerts with friends. Dark theme, neon
accents (violet/cyan), mobile-first design. Custom "concerts-only" agenda view
(no empty days), ticket import from URL (Shotgun, Dice, Ticketmaster), shared
schedule between friends.

## Stack (adjust to your actual setup)

- Frontend: Next.js (App Router) + TypeScript + Tailwind CSS + shadcn/ui
- Backend: Next.js API routes or a separate service (TBD)
- Database: TBD (Postgres/Supabase recommended for simple relational data)
- Deployment target: Vercel

> Update/complete this section once your backend/DB stack is finalized.

## Code conventions

- Strict TypeScript, no unjustified `any`
- ESLint + Prettier (or Biome): code must pass lint before any commit
- UI components in `/components`, business logic kept out of React components
- No hardcoded secrets or API keys — always via environment variables

## Git workflow — fine granularity

### Principle

A branch = a single indivisible unit of work, not a whole feature.
A complete feature (e.g. "import ticket from URL") must be broken down
into several successive branches, each merged before starting the next.

### Expected breakdown

For any new feature, break it down BEFORE coding into sub-steps such as:

- 1 branch = 1 data model/schema (e.g. `feat/ticket-model`)
- 1 branch = 1 isolated DB migration (e.g. `feat/ticket-migration`)
- 1 branch = 1 single endpoint/route, without advanced business logic
  (e.g. `feat/ticket-endpoint-skeleton`)
- 1 branch = 1 block of business logic added to the existing endpoint
  (e.g. `feat/ticket-url-parsing`)
- 1 branch = 1 validation/error handling pass (e.g. `feat/ticket-validation`)
- 1 branch = 1 dedicated test or test suite (e.g. `test/ticket-endpoint`)

If a planned step exceeds ~150-200 lines of diff, break it down further
before starting.

### Commit rules

- 1 commit = 1 single, coherent logical change
- The build/existing tests must pass after EVERY commit, not just at the
  end of the branch
- Format: conventional commits (feat:, fix:, refactor:, test:, chore:)
- Never stack multiple responsibilities in one commit
  ("add the model AND the endpoint" = 2 commits, possibly 2 branches)

### Merge rules

- Never commit directly to main/develop
- Merge as soon as a unit of work is functional and tested, even if the
  overall feature isn't finished yet
- An incomplete branch can be merged if the code it adds is inert/not yet
  exposed (e.g. an endpoint not yet wired to the frontend), rather than
  keeping a branch open too long
- Always propose the branch breakdown BEFORE starting to code, not after —
  ask for confirmation if the breakdown isn't obvious

### What NOT to do

- Don't group "model + endpoint + validation + tests" into a single branch
- Don't wait for the whole feature to be done before merging the first part
- Don't create commits like "wip" or "fix" without describing the actual change

## What Claude should always do before coding

1. Propose the branch/commit breakdown for the requested feature
2. Wait for confirmation if the breakdown isn't obvious
3. Explicitly flag when a technical decision was made on the user's behalf
   (library choice, architecture pattern) so it's tracked and reusable in
   the project's documentation (AI_PROCESS.md)
