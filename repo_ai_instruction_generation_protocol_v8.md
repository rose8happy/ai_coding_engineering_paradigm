# Repository AI Instruction Generation Protocol v8

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
- maintenance rules

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

Typical output:
- a task plan document or existing planning area

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

### 5.3 Avoid giant catch-all instruction files
When detail becomes too large or volatile, move it into supporting files and leave durable pointers in the main instruction file.

### 5.4 Avoid giant catch-all tracking files
Current status should stay concise.
Detailed history belongs in task files, reports, or archives.

### 5.5 Respect repository reality
If the project already uses root `reports/`, `docs/plan`, `docs/research`, or other established conventions, do not force a different layout.

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

The question list should stay short.
Prefer a compact checklist over a long interview.

---

## 7. Main AI Instruction File Requirements
The generated repository AI instruction file should usually contain:
- project scope and purpose
- working directory rules
- important paths and repository structure
- setup, build, test, lint, and run commands when visible
- validation expectations
- editing and change rules
- documentation and memory mapping
- task planning and tracking rules if enabled
- remote execution rules if enabled
- multi-instruction sync rules if enabled
- Git and traceability rules when needed
- maintenance rule stating that the file may be updated by agents when necessary

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

When updating the instruction file:
- preserve repository-specific conventions
- preserve user-confirmed decisions unless new confirmation is needed
- update synchronized AI instruction files according to the repository's sync policy

---

## 11. Finalization and Cleanup
After generating the final repository-specific output:
- ensure the main AI instruction file exists
- ensure selected supporting files exist
- ensure unresolved assumptions are either confirmed or clearly marked
- ensure the instruction file explains where volatile project memory lives
- delete the copied protocol file and copied protocol user guide unless the user explicitly wants them kept

Do not leave temporary bootstrap files behind by default.

---

## 12. Compact Generation Checklist
1. inspect the repository
2. infer everything possible
3. identify existing doc/memory structure
4. choose the instruction filename and sync strategy
5. decide which responsibility layers are needed
6. map layers onto existing paths when possible
7. draft the final AI instruction file
8. ask only a short list of high-impact questions
9. finalize the file and create needed supporting files
10. delete copied bootstrap protocol files unless told otherwise

---

## 13. Optional Starter Snippets
Use only when the repository needs them.
Adjust names and locations to fit the repository.

### 13.1 Compact Tracking Starter
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

### 13.2 Plan Starter
```md
# <TASK_ID_OR_TITLE>

## Goal
- <GOAL>

## Scope
- In:
- Out:

## Steps
1. <STEP>
2. <STEP>

## Validation
- <CHECK>

## Risks / Rollback
- <RISK>
- <ROLLBACK>
```

### 13.3 Active Task Starter
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

### 13.4 Decision Starter
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

### 13.5 Experiment / Run Starter
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
