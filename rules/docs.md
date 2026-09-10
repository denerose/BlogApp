# Documentation Conventions

1. **Vocabulary** must match the [`CONSTITUTION.md`](../CONSTITUTION.md) glossary.
   No synonyms for yarn, ember, kindling, circle, allow list, access link, feed.
2. **Diagrams:** Mermaid only, embedded in markdown. No binary images.
3. **ADRs:** `docs/decisions/NNN-short-title.md`, numbered sequentially, following
   the template in `docs/decisions/TEMPLATE.md`. Record every significant decision.
4. **No dates, estimates, or timeframes** for work — anywhere. Phase status lives
   in `GOALS.md`; narrative plans live in `docs/plans/`.
5. **README.md and AGENTS.md stay thin** — they point at docs, they don't duplicate them.
6. Documents use the constitution's precedence: code < rules < ADRs < constitution.
