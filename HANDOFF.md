# Handoff: setup and Agent Notes ownership

## Resume context

Work in `E:\ai-agent\ddh-skills` on `main`. Read root [AGENTS.md](./AGENTS.md) first: the repository stays in Discuss until the user authorizes `/planning` or `/implement`. The previous `/implement` requests covered the completed setup-path and template-integration changes below. The later discussion about GitHub Issues, RFCs, and Agent Notes has not entered a new stage or been implemented.

The user explicitly requested this handoff at the project root. The handoff skill normally chooses the OS temporary directory; the user's location overrides it.

## Committed work

Commit `89cc347` contains the completed changes to [setup SKILL.md](./skills/user-invoked/setup-matt-pocock-skills/SKILL.md), [artifact-registration.md](./skills/user-invoked/setup-matt-pocock-skills/artifact-registration.md), [decision-records.md](./skills/user-invoked/setup-matt-pocock-skills/decision-records.md), the bundled [Agent Notes README](./skills/user-invoked/setup-matt-pocock-skills/resources/decision-records/.agents/notes/README.md), [lib.mjs](./skills/user-invoked/setup-matt-pocock-skills/resources/decision-records/scripts/decisions/lib.mjs), and the user-restored [record.md](./skills/user-invoked/setup-matt-pocock-skills/resources/decision-records/.agents/notes/templates/record.md). Review that commit before modifying the same files.

Completed work:

- Setup now inspects existing independent Spec/RFC locations and offers `docs/specs/`, `docs/design/`, or custom paths when no locations are defined. It registers selected destinations without creating empty artifacts. Planning's Proposal behavior was not changed; the user specifically rejected a proposed prohibition on adding content to a Proposal.
- The restored `record.md` is now a valid `proposed` Agent Note starter. The bundle lists it as an eighth deployable file, the README points to it, and the checker skips only the root `templates/` resource directory. A temporary target-project fixture passed for an empty deployment and an instantiated template; a record missing `## 备选方案` was rejected. `git diff --check` and two-axis Implementation Review passed. Running the checker directly against the source bundle still reports two `../../docs/AGENTS.md` link failures because those links target a deployed project's document entry.

## Current design discussion

The user observed that real work mostly creates `implemented` Notes, while review and rejection decisions remain in GitHub Proposal comments. The user wants Agent Notes to capture the durable outcome of proposal review, implementation, or rejection, not leave that outcome only in comments. Their required reference direction is **GitHub Issue → Agent Note**, without a reciprocal Issue link from the Note. They asked to establish ownership in a target project's `docs/AGENTS.md` and the Agent Notes README, rather than adding a new creation check to Planning. This repository has no deployed `docs/AGENTS.md`; its source template is [docs-agents.md](./skills/user-invoked/setup-matt-pocock-skills/docs-agents.md).

The user supplied the reference [deepseek-harness Agent Notes README](https://github.com/deepseek-ai/deepseek-harness/blob/master/.agents/notes/README.zh.md). Its [local instructions](https://github.com/deepseek-ai/deepseek-harness/blob/master/.agents/notes/AGENTS.md) describe Agent Notes as RFC-like design documents written by agents. The README defines `proposed` as a pre-implementation reviewed proposal, `implemented` as the shipped decision kept current with reality, and `rejected` as a declined proposal retained only when its rationale prevents an important future mistake. A [proposed example](https://github.com/deepseek-ai/deepseek-harness/blob/master/.agents/notes/proposed/architecture/2026-07-19-required-cancellation-through-tool-capability-seams.md) shows the intended design-document level of detail. A prior assistant suggestion to treat Notes mainly as post-delivery summaries was corrected by this evidence.

The promising model, still a discussion proposal rather than an accepted contract, is: GitHub Issue owns work scope, review conversation, approval evidence, and status, and links to the local Agent Note. The Note owns the current durable design decision, alternatives, rationale, and later the shipped outcome. Substantive review conclusions from Issue comments are applied to the Note; the Note does not copy the comment thread. After verification, rewrite it to describe what shipped and move it to `implemented`. On rejection, retain a `rejected` Note only when its reasoning has future value. For one technical decision, an Agent Note serving as an RFC-equivalent and a separate RFC should not both own the same design. The project must resolve how its [planning RFC rules](./skills/model-invoked/planning/rfc.md) interact with this choice; Spec remains the product-behavior owner.

Potential conflicts to resolve before editing: the bundled Notes README currently asks Notes to reference Proposals/work items and treats RFCs as separate owners; [record.md](./skills/user-invoked/setup-matt-pocock-skills/resources/decision-records/.agents/notes/templates/record.md) also refers back to the Proposal; the checker requires an `approval` field that may duplicate GitHub approval evidence. The existing proposed Note sections can duplicate Issue scope/acceptance or RFC design if ownership is not made precise. Do not silently change these contracts or migrate deployed projects.

Separately, the bundled README and checker hard-code `2026-09-22` as an exception cutoff for old Notes without reconstructible alternatives. The user challenged this global date. A per-project, per-record grandfather list was recommended over a date cutoff, but no change was authorized or made.

## Suggested skills

- `planning` when the user authorizes `/planning` to settle the Issue / Agent Note / RFC ownership and lifecycle contract.
- `writing-for-agents` when editing `docs-agents.md`, the bundled README, the template, or skills.
- `implement` when the user authorizes the agreed new scope with `/implement`; then `code-review` and `verify` as required by root AGENTS.md.

Treat commit `89cc347` as completed work when reviewing future diffs, and distinguish it from any new ownership-model change. No PR has been created.
