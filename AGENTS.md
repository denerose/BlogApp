# AGENTS.md

Hearthside — multi-author, circles-based blog + photo stream. C#/ASP.NET API,
two Postgres databases (identity, content), containers, React SPA later.
**No code yet (P0).**

<!-- adr-path: docs/decisions -->

**Read before working:**

1. [`CONSTITUTION.md`](CONSTITUTION.md) — principles and glossary. Use this vocabulary everywhere.
2. [`GOALS.md`](GOALS.md) — current phase, v1 definition, non-goals.

**Hard rules** ([`rules/`](rules/)):

- [`rules/api-contracts.md`](rules/api-contracts.md) — never guess API contracts; the OpenAPI spec is truth
- [`rules/coding.md`](rules/coding.md) — stack and coding conventions
- [`rules/docs.md`](rules/docs.md) — documentation conventions

**Docs** ([`docs/`](docs/)): [`architecture.md`](docs/architecture.md) ·
[`decisions/`](docs/decisions/) (ADRs) · [`contracts/`](docs/contracts/) · [`plans/`](docs/plans/)

**Skills** ([`skills/`](skills/)): project agent skills; see [`skills/README.md`](skills/README.md).

**Decisions:** significant technical choices get an ADR in `docs/decisions/`
(next number, follow the existing format).
