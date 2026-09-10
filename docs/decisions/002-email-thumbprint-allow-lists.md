# ADR 002: Allow Lists as HMAC-SHA256 Email Thumbprints

**Date:** 2026-09-09
**Status:** Accepted

## Context

Circle membership is defined by the author curating a list of authorized reader
emails. Plaintext email storage would make the database a privacy liability for a
family-scale app; a bare hash (e.g. SHA-256) is vulnerable to rainbow-table
enumeration since email space is small and guessable.

## Decision

- Each allow-list entry stores `HMAC-SHA256(normalized_email, server_secret)`.
- Normalization is **trim + lowercase only** — no provider-specific munging
  (no Gmail dot-stripping).
- Thumbprints live in the content DB per space, with an `is_inner` flag.
- The access-link token carries **no** audience information; the server-side set
  is the only authority.
- Removal of a thumbprint revokes access immediately.

## Alternatives Considered

### Plaintext emails

**Pros:** simplest matching, author-friendly list management.
**Cons:** violates the privacy-by-thumbprint constitution principle; database leak
exposes every reader's address.

### Bare SHA-256 hashes

**Pros:** simple, deterministic.
**Cons:** enumerable via precomputed email rainbow tables; the secret key in HMAC
removes that attack cheaply.

## Consequences

### Positive
- A content-DB leak reveals no reader addresses.
- Instant revocation; server-side authority; short tokens.
- Author dashboard can still manage entries (it can re-thumbprint addresses it
  holds transiently at entry time).

### Negative
- Authors cannot recover a stored address from a thumbprint (management UX shows
  entries by label/last-4 or re-entry).
- A rotated HMAC secret invalidates all lists (rotation is a deliberate,
  list-rebuilding operation).

### Neutral
- Magic-link sending requires the plaintext address at send time — handled at
  entry submission, never persisted.

## Affected Areas

- `docs/architecture.md` — access & circle resolution sequence
- `docs/contracts/openapi.yaml` — `circles` tag (future)

## References

- ADR 001 (databases), ADR 003 (access link)
