# Repository AI Instruction Generation Protocol v9

> Use this protocol to generate or refresh repository-specific AI instruction files.
> This protocol is filename-agnostic and layout-agnostic.
> Common output filenames include `AGENTS.md`, `CLAUDE.md`, `GEMINI.md`, `COPILOT_INSTRUCTIONS.md`, and `.github/copilot-instructions.md`.
> Default only when no stronger signal exists: use `AGENTS.md`.

# <PROJECT_NAME> Repository AI Instruction Generation Protocol

## 0. Purpose
This protocol tells an agent how to discover a repository, infer what it can, ask only a small number of high-impact questions, and then produce the repository's final AI instruction file and any supporting memory files.

This protocol defines:
- responsibilities
- adaptation workflow
- confirmation rules
- output rules
- quality and validation rules
- maintenance and evolution rules

This protocol does **not** define a fixed repository layout.
The agent must map responsibilities onto the repository's existing structure when possible.

---

## 1. Core Principles
1. **Evidence first**
   - Inspect the repository before asking questions.
   - Prefer facts visible in files, scripts, manifests, CI, and existing docs.

2. **Responsibilities, not fixed paths**
   - The protocol defines memory/document responsibilities.
   - The repository-specific output defines where those responsibilities live.

3. **Reuse before introducing**
   - Prefer existing directories and document conventions when they are already working.
   - Do not migrate or rename structure just to match a template.

4. **Small question set**
   - Ask only about high-impact ambiguity that cannot be grounded from repository evidence.

5. **Do what can already be done**
   - Fill obvious parts automatically.
   - Draft the final instruction file before asking about only the unresolved items.

6. **Stable rules separate from volatile state**
   - Keep durable workflow and repository rules in the main AI instruction file.
   - Put volatile task status, plans, reports, experiments, and handoffs in supporting files only when the project needs them.

7. **Living instruction file**
   - The generated AI instruction file is not frozen.
   - Agents may later add, remove, or revise content when repository reality changes or the file becomes inaccurate.

8. **Temporary bootstrap files are disposable**
   - Once the final repository-specific AI instruction file is produced and supporting memory files are created, delete the copied protocol file and copied protocol user guide unless the user explicitly wants them kept.

9. **Independent validation over self-assessment**
   - An agent evaluating its own output is systematically biased toward positive assessment.
   - When quality matters, separate the role that generates from the role that evaluates.
   - Validation should use observable behavior (running the app, executing tests, checking real output) rather than only reading code.

10. **Acceptance criteria are steering**
    - The language used to describe "done" and "good" directly shapes what gets generated.
    - Define acceptance criteria before execution, not after.
    - Choose criteria wording deliberately — vague criteria produce vague results; precise criteria produce precise results.
    - Plans should define the `WHAT` to be delivered and the acceptance boundary, not unnecessarily lock the `HOW`, unless the repository already requires a fixed approach.

11. **Scaffolding evolves with capabilities**
    - Not every layer or process remains necessary as models improve.
    - Periodically re-evaluate which scaffolding components are still load-bearing.
    - Remove process that no longer adds value; the goal is minimum effective structure.

---

## 2. Output Contract
When using this protocol, produce:

1. the repository's final AI instruction file under the most appropriate filename
2. any supporting memory files that are clearly needed for this repository
3. a brief list of assumptions or questions only for unresolved high-impact items
4. deletion of the copied protocol file and copied protocol user guide after finalization, unless the user explicitly asks to keep them

Rules:
- Do not merely describe files that should exist; create the files that are clearly needed.
- Do not force optional supporting files into small repositories that do not benefit from them.
- Do not create a new parallel documentation tree if the repository already has a workable equivalent.
- If multiple AI instruction files already exist, do not overwrite blindly; determine synchronization strategy first.

---

## 3. Repository Discovery Workflow
Follow this order.

### 3.1 Discover
Inspect the repository before asking questions. Check, when present:
- `README*`
- top-level directories
- manifests such as `package.json`, `pyproject.toml`, `Cargo.toml`, `go.mod`, `pom.xml`, `build.gradle*`
- `Makefile`, `justfile`, `Taskfile.yml`, shell scripts, helper scripts
- CI files such as `.github/workflows/`
- container, environment, and dependency files
- existing docs, notes, reports, runbooks, plans, research files, experiments, or task trackers
- existing AI instruction files
- evidence of remote execution, long-running jobs, GPU use, schedulers, or artifact directories
- evidence of multi-agent workflows, evaluation pipelines, or quality gates

### 3.2 Infer
Infer what can be grounded from evidence, including when possible:
- project name
- likely canonical working directory
- source, test, script, and docs locations
- build, test, lint, run, and validation commands
- whether the repository is simple, multi-service, monorepo, research, training, infra, or mixed
- whether long-running tasks exist
- whether remote execution exists
- whether multiple AI instruction files already exist
- whether there is already a functioning memory/document system
- whether multi-agent coordination or evaluation workflows exist
- whether the project has existing quality gates, acceptance tests, or validation pipelines

### 3.3 Draft
Before asking questions, draft:
- the final AI instruction file structure
- a proposed mapping from responsibilities to existing repository paths
- any obviously needed supporting files
- a short list of unresolved high-impact items

### 3.4 Confirm
Ask the user only about ambiguities that materially affect generated outputs.
Typical high-impact examples:
- preferred instruction filename when several are equally plausible
- whether one existing AI instruction file is canonical over others
- whether existing doc directories should remain authoritative
- whether the project involves remote servers, GPUs, schedulers, or shared machines not clearly visible in the repo
- whether task tracking, plans, reports, experiments, or handoffs should be enabled when not inferable
- whether generated memory files should be committed
- any hidden setup, approval, security, or safety rule not inferable from the repo
- preferred language for generated files when not inferable
- whether multi-agent coordination or independent validation workflows are in use or desired

### 3.5 Finalize
After user confirmation:
- update the draft
- keep grounded defaults for unanswered low-risk items
- generate the final AI instruction file
- generate the selected supporting files
- remove leftover assumption markers where possible
- delete the copied protocol file and copied protocol user guide unless the user explicitly wants them kept

---

## 4. Responsibility Layers
The agent should reason in terms of responsibilities, not fixed paths.
For each layer below, decide whether the repository needs it and where it should live.
Prefer mapping onto existing paths.

### 4.1 Stable Rules Layer
Purpose:
- durable repository guidance
- structure, commands, workflows, safety, validation, and maintenance rules

Typical output:
- one or more repository AI instruction files

### 4.2 Current Status Layer
Purpose:
- current dashboard-style task visibility
- active blockers, current focus, next step

Typical output:
- a compact tracking or status file

### 4.3 Planning Layer
Purpose:
- plan for medium or large tasks before execution
- scope, steps, validation, risks, rollback, deliverables
- **completion contract**: define what "done" means before writing code, so that generator and evaluator (human or agent) share the same expectations

A completion contract should include:
- observable acceptance criteria, not just descriptions of intent
- validation method (how will "done" be verified — tests, manual check, running app, screenshot comparison, etc.)
- scope boundary (what is explicitly out of scope for this task)
- implementation boundary: define `WHAT` must be delivered, but do not over-prescribe `HOW` unless the repository or task already requires a fixed implementation path

Typical output:
- a task plan document or existing planning area, with acceptance criteria stated upfront

### 4.4 Active Task Memory Layer
Purpose:
- current task working memory for long or multi-step tasks
- latest state, next step, links, blockers

Typical output:
- task-specific status files or an equivalent active-work area

### 4.5 Archive Layer
Purpose:
- preserve completed task history without bloating active tracking

Typical output:
- archived task files, completion index, or equivalent

### 4.6 Decision Layer
Purpose:
- durable record of major decisions and tradeoffs

Typical output:
- decision records, ADRs, or equivalent notes

### 4.7 Deep Analysis / Report Layer
Purpose:
- detailed analysis, writeups, evaluations, investigations

Typical output:
- reports, research notes, investigations, or equivalent

### 4.8 Experiment / Run Layer
Purpose:
- experiment, training, evaluation, or benchmark records when relevant

Typical output:
- run registry, experiment log, or equivalent

### 4.9 Runbook Layer
Purpose:
- repeatable procedures

Typical output:
- runbooks, operational docs, checklists, or equivalent

### 4.10 Handoff Layer
Purpose:
- cross-session or cross-agent continuity when needed

Typical output:
- handoff notes or equivalent

### 4.11 Remote Execution Layer
Enable only if the project actually uses remote machines, shared compute, GPUs, schedulers, or remote artifact locations.
Purpose:
- local vs remote role boundaries
- remote write-back rules
- runtime state references
- remote job observation and artifact locations

Typical output:
- remote execution notes integrated into the main instruction file and, if needed, task or run documents

### 4.12 Multi-Instruction Synchronization Layer
Enable only if the repository uses more than one AI instruction file.
Purpose:
- determine canonical source
- define mirror or synchronization rules
- document allowed model-specific differences

Typical output:
- a sync policy section in the generated instruction files

### 4.13 Validation & Quality Assurance Layer
Disabled by default. Enable only when repository evidence or explicit user intent justifies structured quality evaluation, especially for tasks where output quality is subjective or multi-dimensional.
Do not enable by default for small repositories, binary-correctness tasks, or workflows that already have sufficient automated checks.
Purpose:
- separate generation from evaluation to avoid self-assessment bias
- define quality dimensions and scoring criteria relevant to the project
- specify validation methods (automated tests, real interaction, visual inspection, etc.)
- establish pass/fail thresholds or quality gates when appropriate

Quality dimensions should be project-specific. Examples:
- for UI work: design quality, originality, craft, functionality
- for APIs: correctness, performance, error handling, documentation
- for data pipelines: accuracy, completeness, latency, fault tolerance
- for research: reproducibility, rigor, clarity, novelty

Key rules:
- validation criteria should be shared with both the generator and evaluator before work begins
- prefer real-world validation (running the app, executing tests, operating the system) over code-only review
- use few-shot examples or reference implementations to calibrate quality expectations when possible
- criteria wording directly influences output style — choose words deliberately
- if evaluation fails, record actionable delta feedback with at least: failed criterion, observed behavior, evidence or validation method, and required change for the next iteration
- do not cargo-cult evaluator loops; keep them only when they catch failures or improve decisions beyond lighter validation

Typical output:
- quality criteria section in task plans or the main instruction file
- evaluation checklists or rubrics for recurring task types
- actionable iteration feedback files or sections when evaluation fails
- validation scripts or test harnesses when automated checking is feasible

### 4.14 Multi-Agent Coordination Layer
Disabled by default. Enable only when repository evidence or explicit user intent shows that multiple agents are actually coordinating on the same codebase or workflow.
Do not enable for single-agent workflows, small repositories, or cases where a human operator already provides sufficient coordination.
Purpose:
- define agent roles and boundaries (e.g., planner, implementer, reviewer, evaluator)
- establish communication protocol between agents
- prevent conflicting changes and redundant work

Key rules:
- prefer file-based communication over message queues or shared state — one agent writes, another reads
- coordination files should be simple, auditable, and human-readable
- each agent should have a clear scope of responsibility
- define handoff points explicitly: what triggers the next agent, what constitutes a valid handoff
- do not cargo-cult multi-agent coordination; introduce it only when explicit role boundaries or handoffs exist
- evaluator-to-implementer handoffs should carry actionable delta feedback, not only restate the rubric

Typical output:
- agent role definitions in the main instruction file or a dedicated coordination document
- shared contract files (e.g., sprint contracts, review checklists) that multiple agents read
- status or signal files that agents use to coordinate sequencing

---

## 5. Adaptation Rules
### 5.1 Prefer mapping over migration
If the repository already has useful places for planning, reports, task memory, research, or runbooks, reuse them.

### 5.2 Introduce only what is justified
Create new supporting files only when they provide clear value.
Typical signals:
- multi-step work
- long-running work
- repeated sessions
- experiments or reports
- multiple contributors or agents
- frequent context loss risk
- quality-sensitive deliverables needing structured validation

### 5.3 Avoid giant catch-all instruction files
When detail becomes too large or volatile, move it into supporting files and leave durable pointers in the main instruction file.

### 5.4 Avoid giant catch-all tracking files
Current status should stay concise.
Detailed history belongs in task files, reports, or archives.

### 5.5 Respect repository reality
If the project already uses root `reports/`, `docs/plan`, `docs/research`, or other established conventions, do not force a different layout.

### 5.6 Match scaffolding to current needs
Do not retain process layers purely because they existed before.
If a layer no longer adds value — because models improved, the project simplified, or the team changed — remove or simplify it.
The right amount of scaffolding is the minimum that prevents quality loss or context loss.

---

## 6. Question Discipline
The agent should **not** ask the user about items that can usually be inferred directly from the repository, such as:
- programming language
- package manager
- obvious build/test/lint commands
- existing directories
- existence of existing AI instruction files
- existence of existing reports, notes, or docs

The agent **should** ask about items that materially affect structure or safety, such as:
- which AI instruction filename should be canonical when multiple are possible
- whether multiple existing AI instruction files should remain in sync
- whether remote execution exists outside what is visible in the repo
- whether expensive or risky workflows need stronger tracking
- whether generated memory files should be committed
- hidden operational constraints, approval requirements, or team conventions
- whether independent validation or multi-agent workflows are desired

The question list should stay short.
Prefer a compact checklist over a long interview.

---

## 7. Main AI Instruction File Requirements
The generated repository AI instruction file should usually contain:
- project scope and purpose
- working directory rules
- important paths and repository structure
- setup, build, test, lint, and run commands when visible
- validation expectations and quality criteria when applicable
- editing and change rules
- documentation and memory mapping
- task planning and tracking rules if enabled
- acceptance criteria guidance if the validation layer is enabled
- multi-agent coordination rules if enabled
- remote execution rules if enabled
- multi-instruction sync rules if enabled
- Git and traceability rules when needed
- maintenance rule stating that the file may be updated by agents when necessary
- scaffolding evolution note when the project is expected to change over time

Rules:
- Keep stable rules here.
- Link or point to volatile or large supporting documents instead of embedding everything.
- Do not include unsupported claims.
- Do not freeze outdated instructions.

---

## 8. Supporting File Selection Rules
The agent should decide which supporting files to create.
Use these as capability modules, not mandatory outputs.

### Usually enable current status tracking when:
- work will span multiple sessions
- there are multiple active tasks
- the repository has ongoing project work beyond trivial edits

### Usually enable planning when:
- a task is medium or large
- the task has multiple major steps
- validation is non-trivial
- rollback or risk matters
- the work may span multiple sessions

### Usually enable active task memory when:
- a task needs durable state beyond one dashboard line
- ongoing work is easy to lose across sessions

### Usually enable archive when:
- active task memory would otherwise accumulate completed history and become noisy

### Usually enable decisions when:
- major tradeoffs or non-obvious design choices are expected

### Usually enable reports when:
- deep analysis, investigation, evaluation, or writeups are expected

### Usually enable experiment/run tracking when:
- the repository includes training, benchmarking, evaluation, ablations, or repeated operational runs

### Usually enable runbooks when:
- procedures are repeated and error-prone

### Usually enable handoffs when:
- multiple sessions, people, or agents will continue work asynchronously

### Usually enable remote execution notes when:
- execution or artifacts live on remote machines or shared compute

### Usually enable multi-instruction sync when:
- more than one AI instruction file is part of the repository workflow

### Usually enable validation & quality assurance when:
- repository evidence or explicit user intent indicates structured evaluation is needed
- output quality is subjective or multi-dimensional (e.g., UI, UX, design)
- the project has a history of "looks done but doesn't actually work" issues
- multiple iteration rounds are common because acceptance criteria were unclear
- automated or semi-automated evaluation is feasible and valuable
- the cost of a failed delivery is high relative to the cost of structured evaluation

### Usually enable multi-agent coordination when:
- repository evidence or explicit user intent indicates multiple agents are actually part of the workflow
- multiple agents operate on the same codebase concurrently or sequentially
- there is a clear division of labor (planning, implementation, review, evaluation)
- agent-to-agent handoffs are part of the workflow
- the project uses or plans to use an orchestration harness

---

## 9. Git and Traceability
Enable traceability rules when the project is large enough to benefit.

Recommended practices:
- use task IDs for medium or large tasks
- include task IDs in task plans, task memory, reports, and related branches or commits when practical
- keep code changes and status-document changes aligned
- record commit hashes or artifact references in reports or experiment records when relevant
- treat Git history and project memory as complementary, not interchangeable

Do not impose heavyweight traceability on genuinely tiny repositories.

---

## 10. Maintenance Rules
The generated AI instruction file is a living document.
Agents may and should update it when:
- repository structure changes
- commands or setup rules change
- workflow or validation rules change
- the document becomes inaccurate, misleading, or too large
- supporting memory mapping changes
- multi-file sync policy changes
- quality criteria or validation methods evolve

When updating the instruction file:
- preserve repository-specific conventions
- preserve user-confirmed decisions unless new confirmation is needed
- update synchronized AI instruction files according to the repository's sync policy

---

## 11. Scaffolding Evolution
The protocol itself and the generated instruction files should evolve as models and tools improve.
Layers added for validation or multi-agent coordination should be treated as situational scaffolding, not permanent ceremony.

### 11.1 Periodic re-evaluation
When revisiting a generated instruction file, consider:
- which scaffolding layers are still preventing real problems
- which layers have become unnecessary overhead
- whether newer model capabilities make certain workarounds obsolete
- whether the project's scale or team has changed

### 11.2 Simplification signals
Consider removing or simplifying a layer when:
- the problem it solved no longer occurs (e.g., context anxiety resolved by better models)
- the overhead of maintaining the layer exceeds the value it provides
- the team has grown past the failure mode the layer prevented
- a simpler alternative has become available
- a validation or coordination layer is repeatedly present but does not change decisions, catch failures, or improve handoffs

### 11.3 Evolution record
When removing or significantly changing scaffolding, briefly note in the instruction file or a decision record:
- what was removed or changed
- why it was no longer needed
- what replaced it (if anything)

This prevents future agents or team members from re-introducing removed scaffolding without understanding why it was removed.

---

## 12. Finalization and Cleanup
After generating the final repository-specific output:
- ensure the main AI instruction file exists
- ensure selected supporting files exist
- ensure unresolved assumptions are either confirmed or clearly marked
- ensure the instruction file explains where volatile project memory lives
- delete the copied protocol file and copied protocol user guide unless the user explicitly wants them kept

Do not leave temporary bootstrap files behind by default.

---

## 13. Compact Generation Checklist
1. inspect the repository
2. infer everything possible
3. identify existing doc/memory structure
4. choose the instruction filename and sync strategy
5. decide which responsibility layers are needed (including validation and multi-agent coordination)
6. map layers onto existing paths when possible
7. draft the final AI instruction file
8. ask only a short list of high-impact questions
9. finalize the file and create needed supporting files
10. delete copied bootstrap protocol files unless told otherwise

---

## 14. Optional Starter Snippets
Use only when the repository needs them.
Adjust names and locations to fit the repository.

### 14.1 Compact Tracking Starter
```md
# Tracking

## Active
- <TASK_OR_AREA> — <CURRENT_STATE> — <POINTER>

## Blocked
- <TASK_OR_AREA> — <BLOCKER> — <POINTER>

## Next
- <NEXT_ACTION>

## Last Updated
- <DATE_TIME>
```

### 14.2 Plan Starter (with Completion Contract)
```md
# <TASK_ID_OR_TITLE>

## Goal
- <GOAL>

## Scope
- In:
- Out:

## Completion Contract
> Define "done" before starting work. Both generator and evaluator must agree on these criteria. The contract should define `WHAT` must be delivered without unnecessarily locking `HOW`.

### Acceptance Criteria
- [ ] <OBSERVABLE_CRITERION_1>
- [ ] <OBSERVABLE_CRITERION_2>
- [ ] <OBSERVABLE_CRITERION_3>

### Validation Method
- <HOW_WILL_DONE_BE_VERIFIED — e.g., run tests, manual walkthrough, browser check, screenshot comparison>

## Steps
1. <STEP>
2. <STEP>

## Risks / Rollback
- <RISK>
- <ROLLBACK>
```

### 14.3 Active Task Starter
```md
# <TASK_ID_OR_TITLE>

## Summary
- <ONE_LINE_GOAL>

## Status
- Not started / In progress / Blocked / Done

## Latest State
- <LATEST_STATE>

## Next Step
- <NEXT_STEP>

## Links
- Plan:
- Report:
- Artifact:

## Last Updated
- <DATE_TIME>
```

### 14.4 Decision Starter
```md
# <DECISION_TITLE>

## Context
- <CONTEXT>

## Options
- <OPTION_A>
- <OPTION_B>

## Decision
- <CHOSEN_OPTION>

## Consequences
- <IMPACT>
```

### 14.5 Experiment / Run Starter
```md
# <RUN_ID_OR_TITLE>

## Objective
- <OBJECTIVE>

## Inputs
- Commit:
- Config:
- Dataset / Environment:

## Execution
- Command:
- Host / Environment:
- Log / Artifact Path:

## Result
- <KEY_RESULT>

## Conclusion
- <ONE_LINE_CONCLUSION>
```

### 14.6 Quality Evaluation Starter
```md
# <TASK_ID> Quality Evaluation

## Evaluation Date
- <DATE>

## Evaluator
- <HUMAN / AGENT_NAME / AUTOMATED>

## Criteria & Scores

| Dimension | Description | Score (1-5) | Notes |
|---|---|---|---|
| <DIMENSION_1> | <WHAT_IS_BEING_MEASURED> | <SCORE> | <OBSERVATION> |
| <DIMENSION_2> | <WHAT_IS_BEING_MEASURED> | <SCORE> | <OBSERVATION> |
| <DIMENSION_3> | <WHAT_IS_BEING_MEASURED> | <SCORE> | <OBSERVATION> |

## Validation Method
- <HOW_WAS_IT_TESTED — e.g., ran app in browser, executed test suite, manual walkthrough>

## Pass / Fail
- Threshold: <MINIMUM_ACCEPTABLE_SCORE>
- Result: <PASS / FAIL>

## Next Iteration Delta
> Required when result is `FAIL`. Repeat once per failed criterion.

- Failed criterion:
- Observed behavior:
- Evidence / validation method:
- Required change for next iteration:

## Feedback for Next Iteration
- <SPECIFIC_ACTIONABLE_FEEDBACK>
```

### 14.7 Multi-Agent Coordination Starter
```md
# Agent Coordination

## Agents & Roles

| Agent | Role | Scope | Trigger |
|---|---|---|---|
| <AGENT_1> | <ROLE — e.g., Planner> | <WHAT_IT_OWNS> | <WHEN_IT_ACTIVATES> |
| <AGENT_2> | <ROLE — e.g., Implementer> | <WHAT_IT_OWNS> | <WHEN_IT_ACTIVATES> |
| <AGENT_3> | <ROLE — e.g., Evaluator> | <WHAT_IT_OWNS> | <WHEN_IT_ACTIVATES> |

## Communication Protocol
- Medium: file-based (one agent writes, another reads)
- Contract files: <PATH_TO_SHARED_CONTRACTS>
- Signal files: <PATH_TO_STATUS_SIGNALS>
- Feedback files: when evaluation fails, record delta items with failed criterion, observed behavior, evidence / validation method, and required change

## Handoff Rules
- <AGENT_1> → <AGENT_2>: <WHAT_CONSTITUTES_A_VALID_HANDOFF>
- <AGENT_2> → <AGENT_3>: <WHAT_CONSTITUTES_A_VALID_HANDOFF>
- <AGENT_3> → <AGENT_2> (iteration): <WHEN_AND_HOW_FEEDBACK_IS_SENT_BACK; feedback must be specific enough to drive the next pass without re-interpreting the rubric>

## Iteration Limits
- Max iterations per sprint: <N>
- Escalation: <WHAT_HAPPENS_IF_LIMIT_IS_REACHED>
```
