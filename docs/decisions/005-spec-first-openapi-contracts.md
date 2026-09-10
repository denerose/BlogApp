# ADR 005: Spec-First OpenAPI Contracts

**Date:** 2026-09-09
**Status:** Accepted

## Context

The project is developed agent-heavy (AI coding agents). A recurring failure mode
is agents inventing or "remembering" API shapes that don't exist. A contract
source of truth is needed that exists **before** implementations, is consumable
by agents, frontend work, and Postman alike, and is diffable in review.

## Decision

- `docs/contracts/openapi.yaml` is the **single source of truth**, hand-maintained,
  spec-first: spec changes precede implementation commits.
- A Postman collection in `docs/contracts/postman/` is updated in the same commit
  as any spec change.
- `rules/api-contracts.md` makes "never guess at contracts" a hard rule for
  humans and agents.
- Contract vocabulary must match the constitution glossary.

## Alternatives Considered

### Code-first (Swashbuckle/NSwag emits spec from ASP.NET)

**Pros:** spec cannot drift from implementation; compiler-backed.
**Cons:** the contract doesn't exist until code does — frontend, Postman, and
agents have nothing to consume first; drift-checking requires CI plumbing; agents
can still "helpfully" shape controllers wrongly before any spec review.

## Consequences

### Positive
- Agents and the frontend can work against a contract before endpoints exist.
- Review happens at the contract level, where mistakes are cheapest.

### Negative
- Real drift risk: implementation may diverge from the hand-maintained spec.
  Mitigation: contract tests against the spec in P2.

### Neutral
- Postman import replaces hand-built collections.

## Affected Areas

- `docs/contracts/` — all files
- `rules/api-contracts.md` — hard rule

## References

- Constitution principle 5 (contracts before code)
