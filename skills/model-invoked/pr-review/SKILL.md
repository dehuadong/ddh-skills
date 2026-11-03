---
name: pr-review
description: Independently review an open pull request before merge authorization, checking the complete PR diff, project standards, specification coverage, architecture, regressions, scope, and required CI evidence.
---

# PR Review

Perform an independent, pre-merge review of an open pull request. This is not a substitute for Implementation Review or final Verify, and it does not authorize approval or merge.

## 1. Pin the review target

Establish and record:

- repository and PR number / URL
- base branch and its current SHA
- PR head branch and exact head SHA
- merge base and complete changed-file list
- governing work item, accepted requirements, design and decisions
- final Verify result and the commit SHA it covered

Review the complete PR diff against its merge base, not just the latest commit or a hand-picked file list. Confirm the PR base/head and changed files match the authorized scope. If the PR head differs from the commit covered by Verify, determine whether the difference is documentation-only or affects implementation/acceptance; affected changes require Verify again before PASS.

If the base branch has advanced, check the current merge base, mergeability and whether the new base changes the review or verification assumptions. Do not silently treat stale verification as current.

## 2. Review the change

Use the repository's code-review skill for the Standards and Spec axes, supplying the PR merge base, complete PR diff, governing contract and current standards. In addition, independently assess:

### Architecture

- module and component ownership, cohesion and boundaries
- dependency direction, cycles and unnecessary coupling
- API, data model, persistence and integration contracts
- control flow, lifecycle, error handling and observability
- whether the solution fits existing architecture without speculative abstractions

### Regression and risk

- changed and adjacent behavior, compatibility and migration implications
- edge cases, failure paths, security/privacy boundaries and data integrity
- tests that should cover changed behavior and meaningful regressions
- rollout, reversibility and blast radius

### Scope and delivery integrity

- every changed file and commit belongs to the authorized work
- no unrelated, generated, secret, local, or accidental files
- PR description accurately states behavior, evidence, risks and merge danger
- no unreviewed commits or post-Verify changes are hidden in the PR

Do not manufacture findings from hypothetical concerns. Each material finding must identify the affected file/behavior, expected contract or standard, observed issue, impact, and a practical resolution. Separate blocking defects from optional improvements.

## 3. Check CI and verification evidence

Inspect the checks for the exact current PR head SHA.

- Required checks must be completed and successful.
- Pending, skipped, missing, cancelled, or failed required checks are not PASS; explain whether they are BLOCKED or FAIL according to the project gate.
- Confirm the final Verify evidence covers the implementation commit currently proposed.
- Do not infer that a workflow passed from its presence or from a prior run on another SHA.
- If no CI is configured, report that fact; do not invent a CI requirement unless the project contract requires one.

## 4. Findings and correction loop

Classify the review result:

- **PASS** — no unresolved material findings; scope and evidence are sound; required checks pass.
- **FAIL** — at least one demonstrated contract violation, important defect, unauthorized change, or failed required check.
- **BLOCKED** — a required judgement cannot be made because a necessary input, permission, or evidence is unavailable and no demonstrated failure already determines FAIL.

For FAIL, route each finding to its owner. Implementation defects return to Implement; missing or invalid acceptance evidence returns to Verify; contract or scope changes return to Discuss / Planning under docs/agents/GATES.md. After correction, update the PR, rerun affected tests and Verify as required, then review the latest PR head again.

Limit PR Review correction cycles to five rounds. Repeating the same finding without new evidence is not a new round; reaching the limit does not imply PASS.

## 5. Report

Report:

1. PR identity, base/head SHAs and merge-base status.
2. Overall result: PASS, FAIL or BLOCKED.
3. Findings grouped under Standards, Spec, Architecture, Regression, Scope and CI/Evidence. State “none” where a category has no material finding.
4. Exact checks and their status, with run links or identifiers when available.
5. Verify commit SHA and whether it covers the current PR head.
6. Remaining risks, blockers and the required next action.

A PASS is a recommendation that the PR is ready to enter the separate merge-authorization stage. Do not submit a GitHub APPROVE review, request changes, merge, enable auto-merge, or delete branches unless separately and explicitly authorized.
