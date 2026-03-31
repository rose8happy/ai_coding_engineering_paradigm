# Repository AI Instruction Generation Protocol User Guide v8

This guide explains how to use `repo_ai_instruction_generation_protocol_v8.md`.

v8 changes the model from a fixed layout template to a **generation protocol**.
It is meant to help an agent create the right repository-specific AI instruction file for the actual project instead of forcing one default directory tree onto every repository.

---

## 1. What changed in v8
Older versions leaned toward a default memory layout.
v8 deliberately changes the center of gravity.

v8 is based on these ideas:
- the template should stay generic
- the agent should inspect the repository first
- the agent should infer as much as possible without asking
- the agent should ask only a short list of high-impact questions
- the final structure should fit the project, not the other way around
- responsibilities matter more than fixed paths

That means v8 no longer assumes paths such as `docs/plans/` or `docs/tasks/active/`.
Instead, it asks the agent to determine where planning, tracking, reports, decisions, experiments, runbooks, and handoffs already live or should live.

---

## 2. What the protocol is for
Use v8 when you want an agent to:
- inspect a repository before drafting instructions
- reuse existing directory conventions
- decide which memory layers are actually needed
- ask only a few structural questions
- generate a repository-specific AI instruction file
- optionally generate supporting memory files only where justified
- remove the copied bootstrap protocol files after finalization unless you want them kept

This works better than a rigid default layout for repositories that already have their own structure.

---

## 3. What the protocol is not for
v8 is not a one-size-fits-all directory prescription.
It should not be used to force all repositories into the same tree.

In particular, the agent should not:
- create a large new documentation structure just because the protocol mentions many possible layers
- rename or migrate existing paths purely for template symmetry
- ask the user about facts that are already visible in the repository
- treat every optional layer as mandatory

---

## 4. The operating model
The intended workflow is:
1. discover repository facts
2. infer what can be grounded
3. draft the output structure and memory mapping
4. ask only about high-impact ambiguity
5. finalize the repository-specific instruction file and supporting files
6. delete the copied protocol files unless the user wants them kept

This model intentionally pushes more work onto the agent before it asks the user anything.

---

## 5. Responsibility layers instead of fixed directories
v8 defines responsibility layers that a project may or may not need.
The agent decides whether to enable them and where they should live.

The layers are:
- stable rules
- current status
- planning
- active task memory
- archive
- decisions
- deep analysis / reports
- experiments / runs
- runbooks
- handoffs
- remote execution notes
- multi-instruction synchronization

The important thing is not the path name.
The important thing is that the project has a place for a responsibility **if** it needs one.

Example:
- one project may use `docs/plan`
- another may use `notes/plans`
- another may keep planning inside task files

All three can be valid if they are clear and durable.

---

## 6. What the agent should usually do without asking first
Before asking the user anything, the agent should usually:
- inspect top-level files and directories
- inspect manifests and automation files
- identify existing docs, reports, plans, research notes, runbooks, and trackers
- identify existing AI instruction files
- infer commands, working directory, and project shape when possible
- propose a filename for the final AI instruction file
- propose which responsibility layers are needed
- map those layers onto existing paths where possible
- draft the final instruction file structure

This reduces unnecessary back-and-forth.

---

## 7. What the agent should still confirm
The agent should still ask about items that can materially change the generated output.
Typical examples:
- which AI instruction filename should be canonical
- whether several AI instruction files should stay synchronized
- whether the repository uses remote servers, GPUs, schedulers, or shared machines not visible in the repo
- whether generated memory files should be committed
- whether certain existing directories are intended to remain authoritative
- any hidden setup or approval requirement not visible in files
- preferred language when unclear

The protocol expects these questions to be short and focused.

---

## 8. Choosing supporting files
v8 does not assume every project needs every supporting file.
The agent should enable only what helps the project.

Common guidance:
- enable a compact tracking file when work spans multiple sessions or multiple active tasks
- enable plans for medium or large tasks
- enable task-specific memory when a one-line dashboard is not enough
- enable archive when active task history would otherwise become noisy
- enable decisions for important tradeoffs
- enable reports for deep analysis or writeups
- enable experiment/run tracking for training, benchmarking, or repeated runs
- enable runbooks for repeated operational procedures
- enable handoffs for multi-session or multi-agent continuity
- enable remote execution notes only when remote work is real
- enable multi-instruction sync rules only when multiple instruction files are part of the workflow

---

## 9. Existing repositories should usually be adapted, not rebuilt
This is one of the most important v8 rules.

If the repository already has useful places for:
- plans
- reports
- research notes
- runbooks
- tracking
- experiments

then the agent should usually map protocol responsibilities onto those places.

Examples:
- use existing `reports/` instead of creating `docs/reports/`
- use existing `docs/plan/` instead of creating `docs/plans/`
- use existing research notes as the deep analysis layer
- keep current root-level tracking if it already works

The goal is to improve coherence, not to trigger document migration.

---

## 10. Multiple AI instruction files
v8 is intentionally filename-agnostic.
Some repositories use only one AI instruction file.
Some use several, such as:
- `AGENTS.md`
- `CLAUDE.md`
- `GEMINI.md`

When several exist, the agent should determine:
- whether one is canonical
- whether others are mirrors
- whether there are allowed model-specific differences
- how synchronization should work

This should be reflected in the final generated instructions.

---

## 11. Living document rule
The final AI instruction file is not treated as immutable.
The protocol explicitly allows later agents to update it when necessary.

That means the file can evolve when:
- commands change
- structure changes
- working directories change
- supporting memory mapping changes
- sync policy changes
- old instructions become misleading

This helps the instruction file stay useful over time.

---

## 12. Template cleanup rule
The copied protocol file and copied protocol user guide are temporary scaffolding.
Default behavior:
- once the final repository-specific instruction file is generated and supporting files are in place, delete the copied protocol files
- keep them only if the user explicitly wants them preserved

This prevents template clutter from remaining in the repo.

---

## 13. Recommended way to use v8
A practical flow is:
1. give the agent the protocol file and guide
2. let the agent inspect the repository and draft a repository-specific instruction file
3. let the agent ask a short checklist of unresolved structural questions
4. confirm those items
5. let the agent generate the final instruction file and needed supporting files
6. let the agent remove the bootstrap protocol files

This keeps the protocol generic while still producing a project-specific result.

---

## 14. Why this is better for mixed or unusual repositories
v8 is especially useful when a repository:
- already has a non-standard doc layout
- has several existing AI instruction files
- mixes software, research, training, evaluation, or operations
- needs some memory layers but not all
- has conventions that should be preserved

In these cases, a protocol-driven approach is usually better than imposing a fresh default tree.

---

## 15. Suggested default stance for the agent
Unless the repository strongly suggests otherwise, the agent should default to:
- inspect first
- infer first
- reuse first
- ask little
- create only what is justified
- keep the final instruction file concise and durable
- keep volatile details in supporting files only when needed
- delete temporary bootstrap protocol files after finalization

That default stance is the heart of v8.
