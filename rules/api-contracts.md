# API Contracts (HARD RULE)

**Never guess at API contracts.** This applies to humans and AI agents alike.

1. The single source of truth is [`docs/contracts/openapi.yaml`](../docs/contracts/openapi.yaml).
   If an endpoint, field, or response is not in the spec, **it does not exist**.
2. Do not implement, call, mock, or document an API surface from memory, conversation,
   or assumption. Read the spec.
3. If the spec is missing something you need:
   - Small addition → edit the spec first, then implement, in that order.
   - Significant change → write an ADR (`docs/decisions/`), update the spec, then implement.
4. The Postman collection (`docs/contracts/postman/`) is updated **in the same commit**
   as any spec change.
5. Contract vocabulary must match the [`CONSTITUTION.md`](../CONSTITUTION.md) glossary —
   `yarn`, `ember`, `kindling`, `circle`, `allow list`, `access link`, `feed`.
