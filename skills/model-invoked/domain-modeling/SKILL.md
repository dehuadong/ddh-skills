---
name: domain-modeling
description: Build and sharpen a project's domain model. Use when discussing codebase terminology, writing or editing a CONTEXT.md, or recording a durable decision in its selected owner.
---

# Domain Modeling

Actively build and sharpen the project's domain model as you design. This is the *active* discipline: challenging terms, inventing edge-case scenarios, and writing the glossary and decisions down the moment they crystallise. (Merely *reading* `CONTEXT.md` for vocabulary is not this skill: that's a one-line habit any skill can do. This skill is for when you're changing the model, not just consuming it.)

## File structure

Follow the documentation hierarchy referenced by project instructions for current decision ownership. Agent Notes may own durable design and decisions; an existing ADR remains authoritative until an authorized migration. Paths and creation rules below are fallbacks where no project convention overrides them.

Resolve the repository management root from project instructions, using the Git top-level only as a fallback. Working inside a subproject does not change it. A root `CONTEXT-MAP.md` selects domain documents when the repository has multiple contexts. Do not create a separate record system per context or derive record categories automatically from contexts.

Example of a single-context project with independent ADRs (Agent Notes projects use their selected record root instead):

```
/
├── CONTEXT.md
├── docs/
│   └── adr/
│       ├── 0001-event-sourced-orders.md
│       └── 0002-postgres-for-write-model.md
└── src/
```

If a `CONTEXT-MAP.md` exists at the root, the repo has multiple contexts. This example shows an existing ADR-based layout:

```
/
├── CONTEXT-MAP.md
├── docs/
│   └── adr/                          ← system-wide decisions
├── src/
│   ├── ordering/
│   │   ├── CONTEXT.md
│   │   └── docs/adr/                 ← context-specific decisions
│   └── billing/
│       ├── CONTEXT.md
│       └── docs/adr/
```

Create files lazily in the selected context or repository-wide owner: only when you have something to write. Resolve context paths from project instructions and the context map; the `src/` layout above is an example of an existing ADR-based project. An existing context ADR remains the decision's authority; Agent Notes may reference it without copying the decision. When Agent Notes own new decisions, use their selected record root rather than creating an ADR directory.

## During the session

### Challenge against the glossary

When the user uses a term that conflicts with the existing language in `CONTEXT.md`, call it out immediately. "Your glossary defines 'cancellation' as X, but you seem to mean Y. Which is it?"

### Sharpen fuzzy language

When the user uses vague or overloaded terms, propose a precise canonical term. "You're saying 'account': do you mean the Customer or the User? Those are different things."

### Discuss concrete scenarios

When domain relationships are being discussed, stress-test them with specific scenarios. Invent scenarios that probe edge cases and force the user to be precise about the boundaries between concepts.

### Cross-reference with code

When the user states how something works, check whether the code agrees. If you find a contradiction, surface it: "Your code cancels entire Orders, but you just said partial cancellation is possible. Which is right?"

### Update CONTEXT.md inline

When a term is resolved, update `CONTEXT.md` right there. Don't batch these up: capture them as they happen. Use the format in [CONTEXT-FORMAT.md](./CONTEXT-FORMAT.md).

`CONTEXT.md` should be totally devoid of implementation details. Do not treat `CONTEXT.md` as a spec, a scratch pad, or a repository for implementation decisions. It is a glossary and nothing else.

### Record durable decisions sparingly

Only offer a durable decision record when all three are true:

1. **Hard to reverse**: the cost of changing your mind later is meaningful
2. **Surprising without context**: a future reader will wonder "why did they do it this way?"
3. **The result of a real trade-off**: there were genuine alternatives and you picked one for specific reasons

If any of the three is missing, skip the separate record. Invoke the `adr` skill for eligibility and owner selection; use the Agent Notes README for Note format and lifecycle, or the ADR skill format when an independent ADR owns the decision.
