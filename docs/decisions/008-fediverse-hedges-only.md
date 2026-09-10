# ADR 008: Fediverse Hedges Only, No Federation Scope

**Date:** 2026-09-09
**Status:** Accepted

## Context

Federation (ActivityPub) compatibility is desirable someday but is explicitly
out of scope for v1. Designing for it fully would distort the schema (actors,
inboxes, delivery queues) for a feature with no committed demand. Ignoring it
entirely risks making future content migration painful.

## Decision

Adopt five **cheap hedges** in the content model and nothing more:

1. Stable **permalinks** for every post
2. **Portable content** — Markdown/HTML source retained, not just rendered output
3. **Author-as-actor identity** — stable IDs, display name, avatar fields
4. **UTC timestamps** with explicit `published_at` semantics
5. Content addressing that **never depends on reader accounts**

No ActivityPub vocabulary, inboxes, outboxes, or federation tables until
federation is explicitly scoped (which would be a new ADR).

## Alternatives Considered

### Full ActivityPub-ready schema now

**Pros:** no migration pain later.
**Cons:** significant schema complexity for an unscoped feature; violates
simplicity-over-scale.

### Ignore Fediverse entirely

**Pros:** simplest today.
**Cons:** risks unportable content and painful migration if federation is ever wanted.

## Consequences

### Positive
- Content stays portable; the door stays ajar at near-zero cost.

### Negative
- If federation is scoped later, a real ADR and schema work are required
  (expected and accepted).

### Neutral
- Hedges are listed in `docs/architecture.md` for implementation reference.

## Affected Areas

- `docs/architecture.md` — Fediverse hedges section
- `GOALS.md` — non-goals

## References

- Constitution principle 7
