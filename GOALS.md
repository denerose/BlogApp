# Hearthside — Goals

## Why

Sharing writing and photos with people you actually know, at two levels of closeness,
without forcing anyone to create an account.

## North star

*Grandma reads the photo stream from a link in her email — no account, no password,
and no idea she's in a circle.*

## v1 definition

- Two (or more) authors, one space each
- Yarns, embers, and kindling published to `public` / `outer` / `inner` audiences
- Magic-link reading via each space's access link
- Author registration (an endpoint creates a user with exactly one space)
- Author login with password or passkey (magic link optional)
- Password reset / email verification (in scope; exact shape settled in planning)
- Dashboard: writing, circle management, access link, settings
- Complete local containerized environment (compose up → working app + seed data)
- OpenAPI-conformant API consumed by a React SPA with cookie sessions

## Non-goals (until explicitly re-scoped)

- Fediverse federation (hedges only — see `docs/architecture.md`)
- Comments, likes, notifications
- Analytics or reader tracking of any kind
- Post scheduling
- Multiple spaces per user
- Business planning of any kind

## Work phases

No timeframes. A phase is done when its exit condition is true.

| Phase | Scope | Exit condition |
|---|---|---|
| **P0 — Governance** *(this phase)* | Constitution, architecture + diagrams, contracts scaffold, rules, README, AGENTS.md | This repo exists as described; shared understanding recorded |
| **P1 — Foundations** | Local compose (api, content-db, identity-db, mailpit), migrations for both DBs, identity DB + author registration & auth (password/passkey, optional magic link) | An author can log in locally |
| **P2 — Content domain** | Yarn/ember/kindling schemas, OpenAPI v1 for posts, posts API | Contract-driven CRUD works against seeded data |
| **P3 — Reader access** | Allow lists + thumbprints, access link, magic-link flow, session circle resolution | An allow-listed stranger reads gated content with no account; a non-listed stranger cannot |
| **P4 — Dashboard** | React SPA, editor (yarn/ember/kindling), circle management, access link view/reset, feed | An author manages their whole space in the browser |
| **P5 — Media pipeline** | Storage interface, image upload, kindling assembly, CDN wiring | Photos flow from upload to CDN-backed display |
| **P6 — Deploy variant** | Self-managed server story (details TBC) | A deployed instance runs the same shapes as local |

## How work happens

- The constitution governs (`CONSTITUTION.md`).
- Significant decisions get ADRs (`docs/decisions/`).
- Phase plans and notes live in `docs/plans/`.
- Phase status is tracked in the table above.
