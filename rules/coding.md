# Coding Conventions

Status: **P0 — no code exists yet.** Language-level conventions (naming, tests,
project layout) are decided in P1 and recorded here. The following are fixed now.

## Fixed

- **Backend:** C# / ASP.NET. Minimal APIs vs controllers — decided in P1 (ADR).
- **Databases:** two Postgres instances (identity, content) with **separate migration
  sets**. See `docs/architecture.md` → Data ownership for what goes where.
- **Containers:** everything runs containerized, dev and deploy. Local environment
  lives in `infra/`.
- **Swappable boundaries:** auth and media storage sit behind interfaces. No direct
  CDN or IdP SDK calls from domain code.
- **Configuration:** via environment variables. **No secrets in the repo.**
- **Reader emails:** never stored in plaintext — thumbprints only
  (HMAC-SHA256, trim + lowercase normalization).

## Commits

Short, imperative, ≤10 words (e.g. "add migrations for allow lists").
