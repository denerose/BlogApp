# Hearthside Constitution

This is the highest law of the project. When documents or code disagree, precedence resolves
upward: **code < `rules/` < ADRs (`docs/decisions/`) < this constitution.**

## Principles (non-negotiables)

1. **Readers never need an account.** Magic-link reading is a first-class path, never a
   second-class citizen. No feature may require a reader account to exist.
2. **Readers never learn which circle they're in.** Inner-circle content is *invisible* to
   outer readers — hidden entirely, not blurred, locked, or teased. Responses never leak
   circle membership.
3. **One resolution rule.** A trusted email (a logged-in account's email, or an email claimed
   via magic link) checked against each space's allow list decides all gated visibility.
   No special cases, no backdoors, no author-to-author exceptions.
4. **Inner ⊆ outer.** Inner-circle members see everything their author shares with the outer
   circle, plus inner-only posts.
5. **Contracts before code.** The OpenAPI spec (`docs/contracts/openapi.yaml`) is the single
   source of truth for the API. Agents — human or AI — never guess at contracts.
   See `rules/api-contracts.md`.
6. **Swappable boundaries.** Authentication (the identity database) and media storage/CDN sit
   behind interfaces. Either can be replaced without rewriting the other.
7. **Fediverse hedges, not Fediverse.** Keep content portable (see hedges in
   `docs/architecture.md`), but no ActivityPub vocabulary enters the schema until
   federation is explicitly scoped.
8. **Simplicity over scale.** Single-developer hobby project. Boring technology, fewest
   moving parts, one host. Every added service must justify itself.
9. **No business planning.** No timeframes, estimates, resource plans, or roadmap dates.
   Ever. Anywhere.
10. **Containers everywhere.** Development and deployment run the same shapes.
11. **Privacy by thumbprint.** Reader emails are stored only as HMAC-SHA256 thumbprints.
    No reader analytics, no tracking, no telemetry on reading.

## Glossary (ubiquitous language)

Use these words — and only these words — for these concepts, in code, docs, conversation,
and API contracts.

| Term | Meaning |
|---|---|
| **Hearthside** | This application. |
| **Space** | One user's home: their posts, circles, and access link. Exactly one space per user. |
| **User** | An account holder (an author in v1). Users can read others' gated content when their email is on the relevant allow list. |
| **Author** | A user who posts. |
| **Reader** | Anyone consuming content: a user, a magic-link reader, or a stranger (public content only). |
| **Post** | Generic term for any published item. |
| **Yarn** | Long-form written post (a blog article). |
| **Ember** | Micro-format post: caption only (≤250 chars), image only, or image + caption (≤250 chars). |
| **Kindling** | Image-group post: 1..N images with a single optional caption (≤250 chars). N is bounded by the group max. |
| **Group max** | Upper bound on images per kindling. Global default, adjustable feature-flag-style. Never user-editable. |
| **Circle** | An allow-list level for a space: **outer** or **inner**. |
| **Allow list** | A space's curated set of email thumbprints, each flagged inner or outer. Allow lists are content, not identity. |
| **Thumbprint** | `HMAC-SHA256(normalized email, server secret)`; normalization = trim + lowercase. Nothing more. |
| **Access link** | A space's single universal link where a reader enters their email to receive a magic link. |
| **Magic link** | One-time emailed link that establishes a reader session for a trusted email. |
| **Dashboard** | Logged-in users' management surface: posts, circles, access link, settings. |
| **Feed** | Logged-in users' aggregate view of posts. |
| **Audience** | A post's visibility: `public`, `outer`, or `inner`. |

## Amendments

Edit this file deliberately and note the change in the commit message. Glossary additions
need no ADR; changes to principles do.
