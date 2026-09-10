# Architecture

Status: **P0 — planned, no code.** These diagrams are normative intent: implementations
follow them, and deviations require an ADR. Vocabulary follows the
[`CONSTITUTION.md`](../CONSTITUTION.md) glossary.

## Post types at a glance

| Type | Shape | Rules |
|---|---|---|
| **Yarn** | title + long-form body | Markdown source retained (portability hedge) |
| **Ember** | caption *or* image *or* both | caption ≤250 chars; 0..1 images |
| **Kindling** | image group + one caption | 1..N images, N ≤ group max (global default 4, hard cap 12, feature-flag adjustable); caption optional, ≤250 chars |

Every post carries an **audience**: `public`, `outer`, or `inner`.

## System context

```mermaid
flowchart TB
    subgraph people["People"]
        A["Author (user)"]
        RU["Logged-in reader (user)"]
        RM["Anonymous reader (magic link)"]
        S["Stranger"]
    end

    subgraph hearthside["Hearthside"]
        WEB["Web (landing, spaces, dashboard, SPA)"]
        API["ASP.NET API"]
    end

    SMTP["SMTP provider / Mailpit (magic links)"]
    CDN["CDN (media, provider TBC)"]

    A -->|"password / passkey / magic link"| API
    RU -->|"cookie session"| API
    RM -->|"magic link from access link"| API
    S -->|"public content only"| WEB
    WEB --> API
    API --> SMTP
    API -->|"via storage interface"| CDN
```

## Containers — local development

The concrete environment. All of it runs via one compose file (`infra/`), created
in P1 with `api`, `content-db`, `identity-db`, and `mailpit` only — `web` joins
in P4, `storage` in P5.

```mermaid
flowchart LR
    BR["Browser"]

    subgraph compose["docker compose"]
        WEB["web — static SPA\n(joins in P4)"]
        API["api — ASP.NET"]
        CDB[("content-db — Postgres\nposts, allow lists, settings")]
        IDB[("identity-db — Postgres\nusers, credentials, sessions")]
        MP["mailpit — local SMTP + inbox UI"]
        ST["storage — local disk volume\n(storage interface impl, joins in P5)"]
    end

    BR --> WEB
    BR --> API
    API --> CDB
    API --> IDB
    API --> MP
    API --> ST
```

**Deploy variant (TBC, P6):** same services on a self-managed host behind a reverse
proxy, with a CDN (Cloudflare preferred, not committed) replacing the local storage
implementation via configuration only. Record as an ADR when scoped.

## Access & circle resolution — the heart of Hearthside

```mermaid
sequenceDiagram
    participant R as Reader (no account)
    participant P as Access link page
    participant API as Hearthside API
    participant CDB as Content DB
    participant SMTP as SMTP / Mailpit

    R->>P: opens a space's access link
    R->>API: submits email
    API->>API: normalize (trim, lowercase) + HMAC-SHA256 thumbprint
    API->>CDB: thumbprint in this space's allow list?
    alt not on list
        API-->>R: generic "check your inbox if invited" (no membership leak)
    else on list
        API->>SMTP: sends one-time magic link
        R->>API: clicks magic link
        API->>API: verifies token, issues session cookie (trusted-email claim)
    end
    R->>API: requests posts
    API->>CDB: re-checks thumbprint against allow list (per request)
    API-->>R: posts visible to that circle — with no indication of circle
```

**Design notes**

- The access link is **universal per space** — one link, one allow list, the server
  silently resolves circles. The link itself can never leak circle membership.
- Sessions carry a **trusted-email claim**, not cached grants. Circle membership is
  re-derived per request (one HMAC + set lookup), so removing a thumbprint revokes
  access immediately.
- Authors reading other authors' gated content is the same rule in its logged-in
  form: the account's email is the trusted email; check it against each space's
  allow list. No special cases.

## Domain model

```mermaid
erDiagram
    USER ||--|| SPACE : "one space per user"
    SPACE ||--o{ POST : publishes
    POST {
        string id PK
        string type "yarn | ember | kindling"
        string audience "public | outer | inner"
        datetime published_at_utc
    }
    POST ||--o| YARN_BODY : "title + markdown source"
    POST ||--o| EMBER_BODY : "caption<=250 and-or image"
    POST ||--o| KINDLING_BODY : "single caption<=250"
    KINDLING_BODY ||--o{ IMAGE : "1..N, N <= group max"
    EMBER_BODY ||--o| IMAGE : "0..1"
    SPACE ||--o{ ALLOW_LIST_ENTRY : curates
    ALLOW_LIST_ENTRY {
        string thumbprint "HMAC-SHA256"
        bool is_inner
    }
    SPACE ||--|| ACCESS_LINK : "one universal"
```

## Data ownership — two databases, one boundary

| Identity DB | Content DB |
|---|---|
| Users (accounts) | Posts (yarns, embers, kindling) |
| Credentials (password hashes, passkeys) | Allow lists (thumbprints + `is_inner`) |
| Sessions & magic-link tokens | Access links, settings, images metadata |

The identity DB is the **swappable auth boundary**: replacing the auth model
(OIDC, another IdP, anything) touches exactly one database and one adapter.
Allow lists are *audience curation owned by each author* — content, not identity —
so they survive any auth swap.

## Authentication summary

| Who | Mechanism |
|---|---|
| Authors (users) | Password or passkey; magic link optional |
| Readers | Magic link only (via access link) |
| Everyone | Cookie sessions; the cookie carries the trusted-email claim the frontend needs |

## Swappable boundaries

| Boundary | Dev implementation | Deploy implementation | Swap cost |
|---|---|---|---|
| Auth | Local identity DB | Same (or future IdP) | One DB + adapter |
| Media storage | Local disk volume | CDN-backed (Cloudflare/AWS/Azure, TBC) | Config + one implementation |

## Fediverse hedges (compatibility, not federation)

1. Stable **permalinks** for every post
2. **Portable content** — Markdown/HTML source retained, not just rendered output
3. **Author-as-actor identity** — stable IDs, display name, avatar fields
4. **UTC timestamps** with explicit `published_at` semantics
5. Content addressing that **never depends on reader accounts**

No ActivityPub fields, inboxes, or outboxes until federation is scoped.
