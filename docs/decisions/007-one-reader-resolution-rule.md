# ADR 007: One Reader-Resolution Rule for All Gated Access

**Date:** 2026-09-09
**Status:** Accepted

## Context

Gated content must be readable by two kinds of identity: anonymous readers
(magic link, no account) and logged-in users (authors reading each other's
spaces). Multiple special-cased paths ("user bypass", "reader tokens",
"author-to-author grants") would multiply auth code and leak paths.

## Decision

A single rule resolves all gated visibility:

> A **trusted email** — a logged-in account's email, or an email claimed via
> magic link — is thumbprinted and checked against each space's allow list.
> The `is_inner` flag determines inner visibility. No special cases.

- Sessions carry a trusted-email claim, **not** cached grants.
- Circle membership is re-derived per request (HMAC + set lookup), so allow-list
  edits apply instantly, both grant and revoke.
- The same rule serves public audience checks trivially (no email required).

## Alternatives Considered

### Cached grants in the session

**Pros:** no per-request DB lookup.
**Cons:** revocation lags until session expiry — contradicts instant-revocation
intent; stale grants after list edits.

### Special author-to-author path

**Pros:** could skip thumbprinting for known accounts.
**Cons:** two code paths, two security surfaces, and a bypass precedent.

## Consequences

### Positive
- One mechanism to test, audit, and explain; constitution principle satisfied.
- Authors reading authors is just the logged-in case — nothing extra to build.

### Negative
- One allow-list lookup per gated request (cheap; cacheable later behind the
  same semantics if ever needed).

### Neutral
- Identity DB says who you are; each space's allow list says what you may see.

## Affected Areas

- `docs/architecture.md` — access & circle resolution sequence
- `docs/contracts/openapi.yaml` — `access` / `feed` tags (future)

## References

- ADR 001 (databases), ADR 002 (thumbprints), ADR 003 (access link)
