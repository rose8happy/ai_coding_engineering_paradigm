<!--
Use this for medium, large, or otherwise non-trivial work.
Skip it for trivial edits that do not need explicit scope, validation, or rollback thinking.
For larger, ambiguous, review-heavy, or multi-agent tasks, pair this with a separate `spec` / requirements doc so the task contract and execution plan stay distinct.
Example path: `.ai/plans/YYYY-MM-DD-<slug>.md`. A related spec can live beside it or in an existing planning/spec area when one already works.
-->

# <TASK_ID_OR_TITLE>

## Goal

- <GOAL>

## Scope

- In:
- Out:

## Related Spec / Requirements (Optional)

- <PATH_OR_NA>

Use this when you need a separate task contract. Let the spec define `WHAT`, and keep this plan focused on sequencing, validation, and rollback.

## Completion Contract

> Define what "done" means before work starts. For medium tasks, this section can stand alone. If a separate spec exists, keep this section as a concise acceptance boundary and point to the spec instead of repeating every requirement. The contract should define `WHAT` must be delivered, keep `HOW` flexible unless the repository already requires a fixed implementation path, and make reviewable outcomes explicit.

### Acceptance Criteria

- [ ] <OBSERVABLE_OUTCOME_1>
- [ ] <OBSERVABLE_OUTCOME_2>
- [ ] <OBSERVABLE_OUTCOME_3>

Acceptance criteria should be observable and reviewable, not vague intent statements.

### Validation Method

- <HOW_DONE_WILL_BE_VERIFIED - e.g., run tests, manual walkthrough, browser check, screenshot comparison>

Validation should state how "done" will be checked in practice.

## Execution Steps

1. <STEP_OR_PHASE>
2. <STEP_OR_PHASE>

## Risks / Rollback

- <RISK>
- <ROLLBACK>
