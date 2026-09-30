# Domain Docs

How the engineering skills should consume this repo's domain documentation when exploring the codebase.

## Before exploring, read these

Use the document ownership entry referenced by project instructions (normally `docs/AGENTS.md`) to distinguish current decision owners from historical sources. Read decisions from the current owner; historical ADRs provide background, not future design authority. File existence or an old `accepted` status does not establish current authority.

Resolve the document owners and root paths from the repository management root established by project instructions and registration, not the current subproject directory. Multiple contexts share that entry and root `.agents/notes/`; historical owners remain as registered. The context map selects domain documents. Its layout and initialization scope require user selection; discovering projects does not initialize contexts automatically. When Notes is enabled, its root configuration separately maps user-selected contexts to isolated record directories within `.agents/notes/`; use the read-only `node scripts/decisions/list.mjs` navigation for the relevant context and selected shared area. Contexts do not imply record categories or automatic Notes enrollment. Read context-local paths from the map and document entry rather than assuming every project lives under `src/`. Ordinary Markdown links resolve relative to their containing file.

- **`CONTEXT.md`** at the repo root, or
- **`CONTEXT-MAP.md`** at the repo root if it exists — it points at one `CONTEXT.md` per context. Read each one relevant to the topic.
- **Current Agent Notes or independently registered decision records** — read decisions that apply to the area you're about to work in. Consult historical ADRs for context when relevant. In multi-context repos, use the context map and document ownership entry to locate context-scoped owners; project directories do not have a fixed `src/` prefix.

If any of these files don't exist, **proceed silently**. Don't flag their absence; don't suggest creating them upfront. The `/domain-modeling` skill (reached via `/grill-with-docs` and `/improve-codebase-architecture`) creates them lazily when terms or decisions actually get resolved.

## File structure

A single-context project may use a root `CONTEXT.md`; a root `CONTEXT-MAP.md` points to selected context glossaries in a multi-context project. Resolve decision record paths separately from the document ownership entry and, when selected, the Agent Notes record-root configuration. Historical ADR paths remain discoverable without becoming destinations for new decisions.

## Use the glossary's vocabulary

When your output names a domain concept (in an issue title, a refactor proposal, a hypothesis, a test name), use the term as defined in `CONTEXT.md`. Don't drift to synonyms the glossary explicitly avoids.

If the concept you need isn't in the glossary yet, that's a signal — either you're inventing language the project doesn't use (reconsider) or there's a real gap (note it for `/domain-modeling`).

## Flag decision conflicts

If your output contradicts a current decision, surface it explicitly rather than silently overriding. A conflicting historical ADR is evidence to examine, not a constraint to obey:

> _Contradicts the current Agent Note on order events — revisit that decision because…_
