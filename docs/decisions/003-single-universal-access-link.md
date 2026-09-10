# ADR 003: One Universal Access Link Per Space

**Date:** 2026-09-09
**Status:** Accepted

## Context

A constitution principle states readers never learn which circle they're in.
Per-circle or per-post share links would leak circle membership through the link
itself (holding an "inner link" reveals you're inner). Links also leak: forwarding
an ungated link grants access forever.

## Decision

Each space has exactly **one universal access link** (e.g. `/alice/access`).
A reader enters their email; the server thumbprints it, checks the space's allow
list, and — if listed — emails a one-time magic link. Circle resolution happens
silently, server-side, per request. Unlisted submissions get a generic
non-committal response.

## Alternatives Considered

### Per-circle share links

**Pros:** revocable per audience; no email step needed for "outer".
**Cons:** the link identity leaks circle membership — violates the constitution.

### Ungated "secret URL" links (link-knowledge = access)

**Pros:** zero friction for readers.
**Cons:** one forward leaks content forever; no revocation power; wrong threat
model for personal content.

## Consequences

### Positive
- The link cannot leak what it doesn't encode.
- One flow for every reader; one code path to secure and test.
- Revocation is list-based, instant (ADR 002).

### Negative
- Every gated reader must complete an email step once per session.
- Requires transactional SMTP (Mailpit locally) from day one.

### Neutral
- Per-post grants remain a possible future feature without changing this model.

## Affected Areas

- `docs/architecture.md` — access & circle resolution sequence
- `docs/contracts/openapi.yaml` — `access` tag (future)

## References

- ADR 002 (thumbprints), ADR 007 (resolution rule)
