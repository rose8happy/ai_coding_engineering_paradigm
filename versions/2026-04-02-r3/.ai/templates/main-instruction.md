<!--
Use this as a starter for a repository's stable AI operating contract.
Use it when the repository needs one canonical main instruction file.
Do not use it for temporary task status, long investigations, experiment logs, repeated handoff notes, or stale planning detail.
Example support paths below use `.ai/...` only as a suggested mapping. Reuse existing repository paths when they already work.
If the repository already has a real canonical instruction filename, keep it. Otherwise `AGENTS.md` is a reasonable default fallback.
-->

# Repository Working Instructions

## Purpose And Scope

- This repository is <WHAT_IT_IS>.
- AI work here is primarily about <WHAT_AGENTS_SHOULD_OPTIMIZE_FOR>.
- Do not <OUT_OF_SCOPE_OR_FORBIDDEN_CHANGE_TYPES>.

## Working Directory Rules

- Treat <PRIMARY_WORKING_DIRECTORIES> as the main work area.
- Do not assume generated, vendored, or external paths are safe to edit unless the repository says so.
- <OTHER_WORKDIR_RULE>

## Repository Map

- <MAIN_ENTRY_OR_README>: <ROLE>
- <SOURCE_OR_DOCS_PATH>: <ROLE>
- <TEST_OR_VALIDATION_PATH>: <ROLE>
- <SCRIPTS_OR_TOOLS_PATH>: <ROLE>

## Commands

- Setup: <COMMAND_OR_NA>
- Build: <COMMAND_OR_NA>
- Test: <COMMAND_OR_NA>
- Lint / Format / Validate: <COMMAND_OR_NA>
- Run: <COMMAND_OR_NA>

Only keep commands that are visible in the repository or explicitly confirmed.

## Editing And Change Rules

- Keep changes aligned with existing repository conventions and file organization.
- Preserve user-confirmed decisions unless new evidence or explicit confirmation changes them.
- Record repository-specific safety, review, or edit restrictions here.

## Documentation And Memory Map

- Keep stable, global, repository-wide rules in this file.
- Move volatile, detailed, or task-specific material into support files only when the repository actually benefits from them.
- Task-specific contracts or requirements may sit beside plans when separating `WHAT` from `HOW` adds clarity.
- Example status path: <STATUS_PATH_OR_REMOVE>
- Example planning path: <PLANS_PATH_OR_REMOVE>
- Optional task contract path: <SPEC_OR_REQUIREMENTS_PATH_OR_REMOVE>
- Example decisions path: <DECISIONS_PATH_OR_REMOVE>
- Example reports or other optional paths: <OPTIONAL_PATHS_OR_REMOVE>

## Enabled Support Layers

Keep only the layers the repository actually uses. Delete unused lines instead of leaving a full disabled-layer inventory.

- Current Status Layer: <STATUS_PATH_OR_REMOVE>
- Planning Layer: <PLANS_PATH_OR_REMOVE>
- Active Task Memory Layer: <TASK_MEMORY_PATH_OR_REMOVE>
- Archive Layer: <ARCHIVE_PATH_OR_REMOVE>
- Decision Layer: <DECISIONS_PATH_OR_REMOVE>
- Deep Analysis / Report Layer: <REPORTS_PATH_OR_REMOVE>
- Experiment / Run Layer: <RUNS_PATH_OR_REMOVE>
- Runbook Layer: <RUNBOOKS_PATH_OR_REMOVE>
- Handoff Layer: <HANDOFFS_PATH_OR_REMOVE>
- Remote Execution Layer: <REMOTE_EXECUTION_NOTES_OR_REMOVE>
- Validation & Quality Assurance Layer: <VALIDATION_PATH_OR_REMOVE>
- Multi-Agent Coordination Layer: <COORDINATION_PATH_OR_REMOVE>

## Canonical Filename And Sync Policy

Keep this section only if more than one instruction file is part of the real workflow.

- Canonical instruction file: <PATH>
- Mirror or companion files: <PATHS_OR_NONE>
- Sync rule: <HOW_CHANGES_STAY_ALIGNED>

## Validation And Quality Expectations

Keep this section only if the repository has real validation, review, or quality gates worth stating here.

- For software or product development repositories, require complete, layered testing appropriate to the repository's risk profile.
- For model training, algorithm, or experiment-heavy repositories, keep tests as sparse and targeted as practical, avoid unnecessary defensive code, and rely on experiments, metrics, benchmarks, or run validation where that matches the real workflow.
- For other repository types, set testing depth and defensive-coding expectations according to actual failure modes, operational risk, and maintenance cost.
- <VALIDATION_RULE_1>
- <VALIDATION_RULE_2>

## Git And Traceability

Keep only the rules that matter for this repository.

- <TRACEABILITY_RULE_1>
- <TRACEABILITY_RULE_2>

## Maintenance

- This file is a living repository contract.
- Update it when repository structure, workflows, enabled layers, or stable rules change.
- Remove or simplify scaffolding when it no longer prevents real context loss, quality loss, or coordination failures.
