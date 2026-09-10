# ADR 001: Split Identity and Content Into Two Databases

**Date:** 2026-09-09
**Status:** Accepted

## Context

Hearthside needs user accounts (authors authenticate with password/passkey) while
gated reading works without accounts at all (magic links). The auth model is the
component most likely to be replaced over the project's life (e.g. moving to an
external IdP). Posts, allow lists, and settings are content that must survive any
such swap untouched.

## Decision

Run two Postgres instances in separate containers:

- **identity-db** — users, credentials (password hashes, passkeys), sessions,
  magic-link tokens. This is the swappable auth boundary.
- **content-db** — posts (yarns, embers, kindling), allow lists (thumbprints +
  `is_inner`), access links, settings, image metadata.

Each database has its own migration set. Allow lists are *content* (audience
curation owned by each author), not identity, so they live in content-db.

## Alternatives Considered

### Single database, two schemas

**Pros:** one container, simpler local setup, cross-schema joins possible.
**Cons:** an auth swap drags a migration through the content store; the "swap the
auth model" boundary becomes conceptual instead of physical.

### Allow lists stored in identity-db as grants

**Pros:** all "who can access what" in one place.
**Cons:** audience curation would not survive an auth swap — contradicting its purpose.

## Consequences

### Positive
- Auth can be replaced by touching exactly one database and one adapter.
- Blast radius of any auth incident is physically bounded.
- Matches the "swappable boundaries" constitution principle.

### Negative
- Two Postgres containers and two migration sets to maintain.
- No cross-database joins (resolution is application-level by design anyway).

### Neutral
- Both DBs run in the local compose stack alongside Mailpit.

## Affected Areas

- `docs/architecture.md` — data ownership table, container diagram
- `rules/coding.md` — separate migrations rule

## References

- ADR 007 (resolution rule), ADR 002 (thumbprints)
