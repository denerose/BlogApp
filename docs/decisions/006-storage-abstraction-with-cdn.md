# ADR 006: Storage Interface With Swappable CDN

**Date:** 2026-09-09
**Status:** Accepted

## Context

Embers and kindling need image storage. The deployment target is a self-managed
server with an external CDN in front (Cloudflare preferred, AWS/Azure possible —
provider TBC). Local development must not depend on any cloud account. The
constitution requires media storage to sit behind a swappable boundary.

## Decision

Define a **storage interface** in the API with two implementations selected by
configuration:

- **Local disk volume** — local development (and the default).
- **CDN-backed object storage** — deployed environments; provider swappable at
  the infra layer (Cloudflare R2 preferred default; AWS S3 / Azure Blob viable).

No CDN or cloud SDK calls from domain code — only via the interface.

## Alternatives Considered

### Postgres blobs

**Pros:** transactional with post rows, zero extra services.
**Cons:** bloats backups and replication for large binaries; poor fit for CDN delivery.

### MinIO in local compose (S3-compatible from day one)

**Pros:** dev/prod parity on the S3 API.
**Cons:** another stateful container to babysit for hobby scale; the interface
already provides parity at the seam that matters.

## Consequences

### Positive
- Local dev needs no cloud account; deployed env swaps via config only.
- Provider choice stays open (Cloudflare/AWS/Azure) with zero domain-code change.

### Negative
- Two implementations to maintain and test.
- CDN-specific features (e.g. R2 custom domains) must stay behind the interface.

### Neutral
- Metadata (paths, captions, ordering) lives in the content DB regardless.

## Affected Areas

- `docs/architecture.md` — swappable boundaries table, container diagram
- `GOALS.md` — P5 scope

## References

- Constitution principle 6 (swappable boundaries)
