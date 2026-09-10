# API Contracts

The OpenAPI specification is the **single source of truth** for the Hearthside API.
See [`rules/api-contracts.md`](../rules/api-contracts.md) for the hard rules.

## Contents

- `openapi.yaml` — the contract (spec-first: it exists before endpoints do)
- `postman/` — Postman collection, kept in sync with the spec

## Workflow

1. Change the spec first (`docs/contracts/openapi.yaml`).
2. Significant changes get an ADR in `docs/decisions/`.
3. Implementation follows the spec — never the reverse.
4. The Postman collection is updated in the same commit as the spec.

**No endpoints exist yet.** The first paths land in P2 (posts API).
The vocabulary in tags/paths/schema names must match the
[`CONSTITUTION.md`](../CONSTITUTION.md) glossary.
