# ADR 004: No Node BFF in v1

**Date:** 2026-09-09
**Status:** Accepted

## Context

The frontend will be a React SPA with local state and cookie sessions. A Node
backend-for-frontend (BFF) was floated for session/token handling and image
proxying, adding a service to operate for a single-developer hobby project.

## Decision

**No BFF in v1.** The SPA talks directly to the ASP.NET API using cookie
authentication with CSRF protection. Revisit via a new ADR when phase P4
(dashboard) starts if pain emerges.

## Alternatives Considered

### Node BFF from day one

**Pros:** token-in-cookie flows fully controlled; a place for image proxying and
response shaping; backend keeps a pure API surface.
**Cons:** an extra runtime, service, and failure mode to maintain; duplicate
contract handling; YAGNI at current scale.

## Consequences

### Positive
- Fewer moving parts; one less container; matches "simplicity over scale".
- Cookies already carry what the frontend needs (trusted-email claim).

### Negative
- If token-based auth or heavy media proxying is ever needed, the BFF decision
  must be revisited (additive change — acceptable).

### Neutral
- Adding a BFF later sits in front of the API without API changes.

## Affected Areas

- `docs/architecture.md` — system context
- `GOALS.md` — P4 scope

## References

- Constitution principle 8 (simplicity over scale)
