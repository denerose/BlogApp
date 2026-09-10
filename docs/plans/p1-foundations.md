# P1 — Foundations (starter plan)

Status: not started. This is a starter plan handed to the implementing agent at
the end of P0. Update it as decisions land; record significant choices as ADRs.

Exit condition (from [`GOALS.md`](../../GOALS.md)): **an author can log in locally.**

## Scope

- `infra/` compose with four services only: `api`, `content-db`, `identity-db`,
  `mailpit` (`web` joins in P4, `storage` in P5 — see
  [`docs/architecture.md`](../architecture.md))
- Migration scaffolding for both databases — separate migration sets (ADR 001)
- Identity DB: users, credentials (password hashes, passkeys), sessions
- Author registration endpoint + login: password, passkey, optional magic link
- Auth paths added to [`docs/contracts/openapi.yaml`](../contracts/openapi.yaml)
  **before** implementation (constitution principle 5); the Postman collection
  is updated in the same commit

## Settled in the P0 review — do not re-litigate

- P1 compose runs the four services listed above; nothing else.
- An author registration endpoint exists in v1; it creates a user with exactly
  one space.
- Password reset / email verification are **in scope** for v1; the exact shape
  is settled with the human during planning (see open decisions).
- The identity DB sessions table must allow **user-optional sessions** carrying
  only a trusted-email claim — reader sessions from magic links are not bound to
  a user row (ADR 007). P1 writes these migrations; design for it now even
  though reader sessions arrive in P3.

## Open decisions — ask the human, never guess

1. Registration: open to anyone, or gated (invite code / allow list)?
2. Password reset, email verification, or both — and does either reuse the
   magic-link mechanism?
3. API style: minimal APIs vs controllers (needs an ADR — `rules/coding.md`)
4. Data access: EF Core vs Dapper vs raw Npgsql
5. Migration tooling: EF migrations vs DbUp/FluentMigrator vs raw SQL
6. Password hashing: ASP.NET Identity vs custom
7. Session cookie shape + CSRF mechanism (ADR 004 commits to cookies + CSRF)
8. Passkey configuration: RP ID and origins for localhost
9. .NET version and solution layout
10. Seed data: with registration existing, is a seeded author still wanted for
    smoke tests? (The v1 definition promises seed data generally.)

## Guardrails

- Contracts before code — read [`rules/api-contracts.md`](../../rules/api-contracts.md)
  and the spec before writing any endpoint.
- No secrets in the repo; configuration via environment variables
  ([`rules/coding.md`](../../rules/coding.md)).
- Every significant choice gets an ADR in [`docs/decisions/`](../decisions/);
  record settled language-level conventions in `rules/coding.md`.
- When P1 starts, refresh the stale P0 status lines in `AGENTS.md`,
  `README.md`, and `rules/coding.md`.
