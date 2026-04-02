# Repository AI Instruction Generation Protocol User Guide v9

[简体中文](./repo_ai_instruction_generation_protocol_user_guide_v9.zh-CN.md)

> This English document is the canonical maintenance source for this protocol pair.

This guide explains **how to use** `repo_ai_instruction_generation_protocol_v9.md` in practice.

It is written for people who want an agent to inspect a repository, infer as much as possible from evidence, ask only a small number of high-impact questions, and then generate a repository-specific AI instruction file plus any supporting memory files that are actually useful.

This is a **task-oriented guide**. It focuses on:
- when to use the protocol
- how to run it
- what the agent should do
- how to decide which optional layers to enable
- how to avoid adding unnecessary process

A summary of what changed in v9 is included near the end for readers upgrading from v8.

---

## 1. What this protocol is for

Use this protocol when you want to create or refresh repository-specific AI instructions such as:
- `AGENTS.md`
- `CLAUDE.md`
- `GEMINI.md`
- `COPILOT_INSTRUCTIONS.md`
- `.github/copilot-instructions.md`

The protocol is **filename-agnostic** and **layout-agnostic**. It does not assume a fixed repository structure. Instead, it tells the agent to inspect the repository first, infer what it can from evidence, and map responsibilities onto the repository's existing structure when possible.

In other words:
- it is **not** a static template to paste blindly into every repository
- it **is** a procedure for generating repository-specific instructions grounded in the repository's actual structure and workflows

---

## 2. What the protocol should produce

When used correctly, the protocol should produce:

1. the repository's main AI instruction file under the most appropriate filename
2. any supporting memory files that are clearly justified for that repository
3. a short list of unresolved assumptions or questions only when they materially affect the result
4. cleanup of the copied protocol file and copied user guide after finalization, unless the user explicitly wants them kept

The protocol is meant to create real outputs, not just describe what someone else should create later.

---

## 3. When to use this protocol

Use it when:
- you are setting up AI instructions for a repository for the first time
- an existing instruction file has drifted away from repository reality
- the repository has grown and now needs clearer rules, structure, or memory files
- the repository has multiple AI instruction files and you need to decide which one is canonical or how they should stay in sync
- you want the agent to infer structure from the repository instead of hand-writing instructions from scratch

You usually do **not** need this protocol when:
- you are making a tiny one-line edit to an already-good instruction file
- the repository is so small that a very short manual instruction file is easier than running the full generation process
- the repository already has accurate, maintained, repository-specific AI instructions and no meaningful workflow changes have occurred

---

## 4. Quick start

For most repositories, the practical workflow is:

1. Place `repo_ai_instruction_generation_protocol_v9.md` in the repository or otherwise make it available to the agent.
2. Ask the agent to inspect the repository and generate the repository's final AI instruction file.
3. Let the agent inspect the repository before answering questions.
4. Let the agent draft a repository-specific instruction file and propose any supporting files.
5. Answer only the small number of high-impact questions that the agent could not infer from repository evidence.
6. Let the agent finalize the output and clean up the copied protocol files unless you want to keep them.

A good outcome is one where the agent:
- reuses existing repository conventions
- avoids inventing extra process unless the repository needs it
- creates only the files that are actually useful
- keeps stable rules in the main instruction file and volatile state elsewhere only when justified

If you are using this repository directly as the source bundle, the latest recommended release lives at the repository root and under `.ai/templates/`.
Use `VERSIONS.md` to choose between the current release and an archived snapshot under `versions/<release-id>/`.
The repository's own `.ai/tracking.md`, `.ai/plans/`, and `.ai/decisions/` files are maintenance memory for the source repository, not starter files to copy blindly.

---

## 5. What the agent should do

The protocol expects the agent to work in this order.

### 5.1 Inspect first

Before asking questions, the agent should inspect the repository for evidence such as:
- `README*`
- top-level directories
- manifests like `package.json`, `pyproject.toml`, `Cargo.toml`, `go.mod`, `pom.xml`, `build.gradle*`
- `Makefile`, `justfile`, `Taskfile.yml`, scripts, and helper commands
- CI files such as `.github/workflows/`
- docs, notes, reports, plans, experiments, and runbooks
- existing AI instruction files
- evidence of remote execution, long-running jobs, schedulers, artifact directories, evaluation pipelines, or multi-agent workflows

The agent should prefer facts visible in the repository over user interrogation.

### 5.2 Infer what can be inferred

From repository evidence, the agent should infer as much as possible, including when visible:
- project name
- important paths
- setup, build, test, lint, run, and validation commands
- repository shape (simple repo, monorepo, research repo, infra repo, mixed repo, etc.)
- whether long-running or remote execution exists
- whether the repository already has a working memory/document system
- whether multiple instruction files or sync rules already exist
- whether validation workflows or multi-agent coordination are already real parts of the project

### 5.3 Draft before asking questions

Before asking the user anything, the agent should draft:
- the likely final instruction file structure
- a proposed mapping from responsibility layers to repository paths
- any obviously necessary supporting files
- a short list of unresolved, high-impact ambiguities

This is important because it prevents the agent from asking broad, low-value questions too early.

### 5.4 Ask only a small number of high-impact questions

The protocol is intentionally strict here. The agent should ask only about issues that materially affect structure, safety, or workflow.

Good examples:
- which instruction filename should be canonical when several are plausible
- whether multiple instruction files should stay synchronized
- whether remote servers, shared GPUs, or schedulers exist outside what the repository reveals
- whether generated supporting files should be committed
- whether hidden approval, security, or safety rules exist
- whether multi-agent coordination or independent validation is desired when not clearly visible in the repository

Bad examples:
- asking which package manager is used when a manifest makes it obvious
- asking what the source directory is when the repository makes it obvious
- asking for build/test commands that already exist in scripts or CI

### 5.5 Finalize and clean up

After the user answers the few remaining questions, the agent should:
- finalize the instruction file
- create the justified supporting files
- remove temporary assumption markers where possible
- delete the copied protocol file and copied user guide unless the user explicitly wants to keep them

---

## 6. How to decide which instruction filename to use

The protocol does not require a single filename. The correct choice depends on repository reality.

### Prefer an existing convention when one already works

If the repository already uses one of these files as its real AI instruction file, keep using it unless there is a strong reason to change:
- `AGENTS.md`
- `CLAUDE.md`
- `GEMINI.md`
- `COPILOT_INSTRUCTIONS.md`
- `.github/copilot-instructions.md`

### Use `AGENTS.md` only as a default fallback

If no stronger signal exists, the protocol defaults to `AGENTS.md`.

### Why a template repository may use a neutral starter filename

When a repository ships reusable starter files, the main-instruction starter is often better named something neutral such as `main-instruction.md`.
That avoids implying that the generated output must be named `AGENTS.md`.
The target repository should still end up with whatever canonical filename its real workflow needs.

### If multiple instruction files already exist

Do not overwrite blindly. First determine:
- which file is canonical
- whether the others are mirrors
- whether model-specific differences are intentional
- whether a synchronization rule should be documented

If more than one instruction file is part of the real workflow, the repository may need a **multi-instruction synchronization rule**.

---

## 7. How to choose supporting files

The protocol works by responsibility layers, not fixed paths. The agent should create supporting files only when they provide clear value.

If you are using this repository directly, the latest core reusable starter set lives in `.ai/templates/`:
- `main-instruction.md`
- `tracking.md`
- `plan.md`
- `decision.md`

Use these as starters, not as a bundle to copy mechanically.
The latest release stays at the repository root and `.ai/templates/`; historical public snapshots live under `versions/<release-id>/` and are indexed from `VERSIONS.md`.
They demonstrate common layer shapes, but the target repository should still reuse its own paths and conventions when those already exist.
Do not copy this repository's `.ai/tracking.md`, `.ai/plans/`, or `.ai/decisions/` by default. Those files are used to maintain the source repository itself.
For long, ambiguous, review-heavy, or multi-agent tasks, an optional `spec.md` or `requirements.md` starter can also help separate the task contract from the execution plan.
Treat that as advanced planning support, not as part of the default core bundle.

### Recommended order for the core template set

When the repository needs more than a single instruction file, a practical sequence is:
1. start with the neutral main-instruction starter and rename it to the repository's real canonical instruction filename
2. add tracking only if current-status visibility is worth maintaining
3. use the plan starter for medium or large work; if the task contract is complex, add a separate `spec` / requirements doc and keep the plan focused on execution
4. use the decision starter when non-obvious choices need a durable record

### Usually useful in larger or longer-running work

These layers are often worth enabling when work spans multiple sessions, tasks, or contributors:
- **Current Status** — a compact dashboard of active work, blockers, and next steps
- **Planning** — plans for medium or large tasks
- **Active Task Memory** — durable working memory for long tasks
- **Archive** — a place to move completed task history so active files stay short
- **Decision Records** — durable records of major tradeoffs
- **Reports / Deep Analysis** — investigations, evaluations, and writeups
- **Runbooks** — repeatable operational procedures
- **Handoffs** — cross-session or cross-agent continuity

### Usually unnecessary in very small repositories

Avoid creating a forest of tracking, planning, status, archive, and coordination files when:
- the repository is tiny
- the work is trivial
- the team does not actually use those files
- the maintenance cost would exceed the benefit

The protocol explicitly prefers **minimum effective structure**.

---

## 8. Planning with a completion contract

For medium or large tasks, the Planning Layer should define a **completion contract** before execution begins.
In many repositories, the plan alone is sufficient.
For longer or multi-agent tasks, it can be cleaner to keep the task contract in a separate `spec` / requirements document and let the plan focus on sequencing, checkpoints, validation, and rollback.

A good completion contract includes:
- **observable acceptance criteria**
- **validation method**
- **scope boundary**
- a clear statement of **what** must be delivered without over-prescribing **how** unless the repository requires a fixed approach

### Why this matters

Without a completion contract, the person or agent doing the work and the person or agent reviewing the work may be working against different definitions of done. That gap is a common cause of rework.

### Example

Weak criterion:
- make the docs better

Better criterion:
- installation steps are complete for a fresh clone
- every command in the quick start was verified against the repository
- the troubleshooting section covers the two known setup failures
- all internal links resolve

---

## 9. Optional layers: when to enable them

The most common mistake is enabling every optional layer just because the protocol supports it. Do not do that.

### 9.1 Validation & Quality Assurance

This layer is **off by default**. Enable it when output quality is subjective, multi-dimensional, or historically unreliable.

Good fit:
- UI, UX, design, writing, or other work where “done” is not just binary correctness
- projects where output often looks done but fails in real use
- workflows where iteration keeps happening because the acceptance criteria are vague
- cases where real validation is feasible, such as running the app, walking through the flow, or performing structured checks

Usually skip it when:
- correctness is binary and existing tests already cover it well
- the repository is small and informal
- lighter validation already catches the relevant failure modes

### 9.2 Multi-Agent Coordination

This layer is **off by default**. Enable it only when multiple agents actually coordinate on the same codebase or workflow.

Good fit:
- planner / implementer / evaluator workflows
- orchestration harnesses
- real handoffs between agents
- clearly separated scopes of responsibility

Usually skip it when:
- only one agent is doing the work
- a human is already providing all necessary coordination
- the repository is too small to justify extra coordination files

### 9.3 Remote Execution

Enable it only if the project truly uses:
- remote machines
- shared servers
- GPUs
- schedulers
- remote artifact locations

If remote execution is not real, do not add remote execution ceremony.

### 9.4 Multi-Instruction Synchronization

Enable it only if the repository genuinely relies on more than one AI instruction file.

---

## 10. How to keep the main instruction file useful

The main AI instruction file should contain **stable repository guidance**, such as:
- repository purpose and scope
- important paths
- working directory rules
- setup, build, test, lint, and run commands when visible
- validation expectations when relevant
- editing and change rules
- documentation and memory mapping
- sync, remote execution, or coordination rules only if those workflows are real
- a maintenance note explaining that the file is a living document

It should **not** become a giant dumping ground for:
- temporary task status
- task-specific contracts that belong in a separate `spec` / requirements doc
- long investigations
- experiment logs
- repeated handoff notes
- stale planning details

When information is volatile, detailed, or task-specific, move it into supporting files only if the repository actually benefits from them.

In a template repository, this is also why a reusable main-instruction starter should stay neutrally named.
The template file is a source asset; the generated repository file should use the canonical filename that fits the target repository.

---

## 11. Common patterns

### Pattern A: Small single-repository app

Usually enough:
- one main AI instruction file
- maybe a compact status or plan file if work spans sessions

Usually not needed:
- coordination files
- quality rubrics
- run registries
- heavy handoff machinery

### Pattern B: Active product repository with ongoing tasks

Often useful:
- main instruction file
- compact tracking/status file
- per-task plans for medium or large work
- decision records for major tradeoffs
- archive area to prevent active files from growing indefinitely

### Pattern C: Research, training, or evaluation repository

Often useful:
- main instruction file
- experiment/run tracking
- reports or analysis documents
- remote execution notes if jobs run elsewhere
- stronger validation rules if results are subtle or high cost to misinterpret

### Pattern D: Multi-agent repository or harness-based workflow

Often useful:
- main instruction file
- clear role definitions
- contract files or checklists used across agents
- feedback files that carry actionable iteration deltas
- handoff rules and iteration limits

But even here, use only the minimum coordination that actually improves outcomes.

---

## 12. Common mistakes to avoid

### Mistake 1: Treating the protocol as a fixed template

The protocol defines responsibilities, not a mandatory directory tree. Reuse the repository's existing structure when possible.

### Mistake 2: Asking too many questions before inspecting

The agent should inspect first and infer first. Questions are for unresolved, high-impact ambiguity only.

### Mistake 3: Creating every optional file by default

More files do not automatically mean better instructions. Extra scaffolding should be justified by real workflow needs.

### Mistake 4: Putting volatile state in the main instruction file

The main instruction file should stay stable and durable. Temporary state belongs elsewhere, and only when necessary.

### Mistake 5: Keeping obsolete scaffolding forever

If a layer no longer prevents real failure modes, simplify or remove it.

---

## 13. Scaffolding should evolve

The protocol assumes that process should adapt over time.

Re-evaluate scaffolding when:
- the repository changes significantly
- the team or workflow changes
- a much better model is adopted
- a file or process layer is being maintained but rarely consulted
- the maintenance cost exceeds the value it provides

Good signs that simplification is appropriate:
- a handoff file is never read
- a tracking file is always stale
- a validation layer never changes decisions
- coordination rules exist on paper but not in actual workflow

The goal is not maximum structure. The goal is the **minimum structure that reliably prevents quality loss or context loss**.

---

## 14. Practical prompts you can give the agent

### First-time setup

> Inspect this repository using `repo_ai_instruction_generation_protocol_v9.md`, infer as much as possible from repository evidence, draft the final AI instruction file and any justified supporting files, then ask me only the small number of unresolved high-impact questions.

### Refresh an existing instruction file

> Refresh this repository's AI instruction file using `repo_ai_instruction_generation_protocol_v9.md`. Reuse existing repository structure where possible, identify any outdated guidance, and only suggest supporting files that are clearly justified.

### Multiple instruction files already exist

> Inspect this repository and determine whether the existing AI instruction files have a canonical source or need synchronization rules. Do not overwrite blindly.

### Existing project, but keep scaffolding light

> Use `repo_ai_instruction_generation_protocol_v9.md`, but keep scaffolding minimal. Only add planning, tracking, validation, or coordination files if repository evidence clearly justifies them.

---

## 15. What changed in v9

If you are already familiar with v8, these are the main additions.

### 15.1 Three added principles

v9 adds three core ideas:
- **independent validation over self-assessment**
- **acceptance criteria are steering**
- **scaffolding evolves with capabilities**

### 15.2 Planning now includes a completion contract

The Planning Layer now explicitly defines “done” before execution through:
- observable acceptance criteria
- validation method
- scope boundary
- a `WHAT` / `HOW` boundary that avoids unnecessary implementation lock-in

In template repositories, that contract can live directly in the plan for medium tasks or in a separate `spec` / requirements document paired with the plan for more complex work.

### 15.3 New optional Validation & Quality Assurance layer

This adds structured quality dimensions, scoring, and validation guidance for cases where output quality is subjective or multi-dimensional.

### 15.4 New optional Multi-Agent Coordination layer

This adds role definitions, file-based communication patterns, handoff rules, and iteration guidance for repositories where multiple agents are genuinely coordinating.

### 15.5 Scaffolding evolution is now explicit

v9 now explicitly instructs agents to simplify or remove scaffolding that no longer provides value.

---

## 16. Relationship to v8

v9 is a superset of v8, not a replacement that invalidates earlier work.

What still applies from v8:
- inspect first
- infer first
- reuse first
- responsibilities over fixed paths
- small question set
- adaptation to repository reality
- living documents
- cleanup of temporary bootstrap files

What is new is mostly additional guidance for:
- clearer planning
- stronger quality evaluation when justified
- multi-agent coordination when justified
- ongoing simplification of unnecessary scaffolding

Projects that do not need the new layers can ignore them.

---

## 17. Migrating from v8

In most cases, migration is simple:

1. replace the old protocol file with v9
2. let the agent inspect the repository again
3. let the agent decide whether any of the new optional layers are now justified
4. update the repository's AI instruction file only where repository evidence or workflow needs support the change

No automatic restructuring is required just because v9 exists.

---

## 18. Final takeaway

The most important way to use this protocol correctly is:
- **inspect before asking**
- **infer before escalating uncertainty to the user**
- **reuse existing repository structure before introducing new files**
- **enable only the layers that solve real problems**
- **keep the main instruction file stable and repository-specific**
- **treat both instructions and scaffolding as living artifacts**

If the protocol is working well, the result should feel like a repository-specific operating guide grounded in evidence — not a generic template and not a pile of unnecessary process.

|      |      |
|---|---|
|      |      |
|      |      |
|      |      |
|      |      |
|      |      |
|      |      |
|      |      |
|      |      |
|      |      |
