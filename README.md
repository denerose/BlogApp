# Hearthside

A small multi-author platform for sharing with the people you actually know.
Long-form **yarns**, quick **embers** (caption, photo, or both), and **kindling**
(bounded photo groups) — shared with your **outer circle**, your **inner circle**,
or the public.

Reading requires no account: each space has one access link. Readers enter their
email; if they're on that space's allow list, a magic link lands in their inbox
and they're in. Readers never learn which circle they're in.

- **Authors** log in with a password or passkey and get a dashboard for writing,
  circles, and their access link.
- **Stack (planned):** C#/ASP.NET API · two Postgres databases (identity + content) ·
  React SPA · containers everywhere · external CDN for media.
- **Status:** P0 — governance and architecture. No code yet. See `GOALS.md`.

## Start here

| Doc | What |
|---|---|
| [`CONSTITUTION.md`](CONSTITUTION.md) | Principles and glossary — read first |
| [`GOALS.md`](GOALS.md) | Why, v1 definition, work phases |
| [`docs/architecture.md`](docs/architecture.md) | Diagrams: context, containers, access flow, domain |
| [`docs/decisions/`](docs/decisions/) | ADRs — every settled decision |
| [`docs/contracts/`](docs/contracts/) | OpenAPI spec + Postman collection (source of truth) |
| [`rules/`](rules/) | Hard rules for humans and agents |
| [`AGENTS.md`](AGENTS.md) | Entry point for AI agents |

## Quick vocabulary

**yarn** — long-form post · **ember** — caption-only (≤250 chars), image-only, or
image+caption post · **kindling** — photo group with one caption · **circles** —
outer/inner allow-list levels · **access link** — where readers start.
