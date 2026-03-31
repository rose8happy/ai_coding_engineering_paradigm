# Repository AI Instruction Generation Protocol User Guide v9

This guide explains how to use `repo_ai_instruction_generation_protocol_v9.md`.

v9 builds on v8's protocol-driven, responsibility-based approach and adds structured quality assurance, multi-agent coordination, and scaffolding evolution — informed by Anthropic's harness design research on long-running application development. These additions remain opt-in and should stay off unless repository evidence or explicit user intent justifies them.

---

## 1. What changed in v9

v9 retains all of v8's core philosophy (inspect first, infer first, reuse first, responsibilities over paths) and adds:

### 1.1 Three new core principles

| Principle | Source insight | What it means |
|---|---|---|
| **Independent validation over self-assessment** | Generator-Evaluator separation (GAN-inspired) | An agent reviewing its own output is biased. Separate generation from evaluation when quality matters. |
| **Acceptance criteria are steering** | Evaluation rubric language shapes output | The words you use to define "done" and "good" directly influence what gets generated. Define criteria before work begins. |
| **Scaffolding evolves with capabilities** | Harness complexity should decrease as models improve | Not every process layer stays necessary. Periodically prune scaffolding that no longer prevents real problems. |

### 1.2 Enhanced Planning Layer (Completion Contract)

The Planning Layer now includes a **completion contract** — the idea that generator and evaluator should agree on the definition of "done" before work begins. This prevents the common failure mode where work is technically "complete" but doesn't meet expectations because criteria were implicit. The contract should define `WHAT` must be delivered, the validation method, and the scope boundary, while avoiding unnecessary prescriptions about `HOW` implementation is done unless repository constraints require it.

### 1.3 New: Validation & Quality Assurance Layer (4.13)

A new responsibility layer for projects where output quality is subjective, multi-dimensional, or historically unreliable. Includes:
- project-specific quality dimensions
- scoring criteria and thresholds
- validation methods (prefer real interaction over code-only review)
- guidance on calibrating evaluator expectations

### 1.4 New: Multi-Agent Coordination Layer (4.14)

A new responsibility layer for projects involving multiple agents. Covers:
- agent role definitions and boundaries
- file-based communication protocol
- handoff rules and iteration limits
- coordination file conventions

### 1.5 New: Scaffolding Evolution section (Section 11)

Dedicated guidance on when and how to simplify or remove scaffolding layers, including:
- periodic re-evaluation triggers
- simplification signals
- evolution records to prevent cargo-culting removed layers back in

### 1.6 New starter snippets

Three new optional templates:
- **Plan with Completion Contract** (replaces the v8 plan starter)
- **Quality Evaluation** — structured scoring rubric
- **Multi-Agent Coordination** — agent roles, communication protocol, handoff rules

---

## 2. When to use the new layers

### Validation & Quality Assurance Layer

Default stance: leave this layer disabled unless repository evidence or explicit user intent justifies it.

Enable when:
- output quality is subjective (UI, UX, design, writing)
- "looks done but doesn't work" has been a recurring problem
- multiple revision cycles happen because criteria were unclear
- you can feasibly validate output by running it (not just reading code)
- the cost of shipping a bad result is high

Skip when:
- the task is simple and correctness is binary (it compiles or it doesn't)
- the project is small and informal
- existing tests already provide sufficient quality gates
- lighter validation already catches the relevant failures

### Multi-Agent Coordination Layer

Default stance: leave this layer disabled unless repository evidence or explicit user intent shows that multiple agents are actually part of the workflow.

Enable when:
- multiple agents work on the same codebase (e.g., planner + implementer + evaluator)
- there is a clear division of labor across agents
- agent-to-agent handoffs are part of the workflow
- you are using or building an orchestration harness

Skip when:
- only one agent interacts with the repository
- coordination is handled entirely by a human operator
- the project is small enough that a single agent handles everything
- role boundaries or handoff points are not real enough to justify extra coordination files

---

## 3. The completion contract in practice

The completion contract is the most immediately useful addition from v9. Here is how to use it:

**Before starting any medium or large task:**
1. Write observable acceptance criteria (not "make it look good" but "navigation works on mobile viewports, all links resolve, loading time < 2s")
2. Specify the validation method (run tests? check in browser? manual walkthrough?)
3. Define what is explicitly out of scope
4. State `WHAT` must be delivered; avoid over-prescribing `HOW` unless repository constraints require it
5. Share these criteria with both the person/agent doing the work and the person/agent reviewing it

**Why this matters:**
Without a completion contract, the generator optimizes for what it *thinks* done means, and the evaluator judges against *their* implicit standard. The gap between these two interpretations is where most rework originates.

---

## 4. Quality dimensions are project-specific

v9 does not prescribe universal quality dimensions. The Anthropic harness research used four dimensions (design quality, originality, craft, functionality) because they were building web apps. Your project will have different dimensions.

Examples:
- **Backend API**: correctness, performance, error handling, API ergonomics
- **Data pipeline**: accuracy, completeness, latency, fault tolerance
- **ML model**: accuracy, inference speed, reproducibility, fairness
- **Documentation**: clarity, completeness, accuracy, findability
- **CLI tool**: correctness, UX, error messages, performance

The key insight is: **write your quality dimensions down before work starts**, because the language of the criteria steers the output.

---

## 5. Scaffolding evolution in practice

v8 introduced living documents. v9 adds living scaffolding — the idea that the process itself should evolve.

**When to re-evaluate scaffolding:**
- when upgrading to a significantly better model
- when the project's scale or team changes
- when you notice a layer being maintained but never consulted
- when a layer's maintenance cost exceeds the problems it prevents

**Example evolution:**
The Anthropic team found that context reset mechanisms (needed for older models with "context anxiety") became unnecessary when they upgraded to newer models with better context handling. They removed the layer rather than keeping it out of habit.

Apply the same thinking to your scaffolding:
- If your handoff layer is never read by subsequent sessions, consider removing it
- If your detailed tracking file is always stale, simplify to a one-liner in the main instruction file
- If your multi-agent coordination is handled well by a single agent now, drop the coordination layer

---

## 6. File-based agent communication

v9 recommends file-based communication for multi-agent setups because:
- files are simple, auditable, and human-readable
- no message queue or shared state infrastructure is needed
- any agent (or human) can inspect the current state at any time
- files integrate naturally with version control

Use this only when real handoffs exist; do not add coordination files as ceremony.

Practical patterns:
- **Contract files**: one agent writes the acceptance criteria, another reads them before starting work
- **Signal files**: a file whose presence or content indicates status (e.g., `sprint-03.ready-for-review`)
- **Feedback files**: the evaluator writes specific, actionable feedback that the generator reads for the next iteration; when evaluation fails, record at least the failed criterion, observed behavior, evidence/validation method, and required change

---

## 7. Relationship to v8

v9 is a superset of v8. Everything in v8 still applies:
- inspect first, infer first, reuse first
- responsibilities over fixed paths
- small question set
- adapt to repository reality
- living documents
- template cleanup

v9 adds layers and principles; it does not remove or contradict anything from v8. Projects that don't need the new layers can ignore them — they remain opt-in and should stay off unless repository evidence or explicit user intent justifies them.

---

## 8. Migration from v8

No migration is needed. v9 is backward-compatible:
- existing v8-generated instruction files remain valid
- new layers are opt-in, not mandatory
- the agent will only enable new layers when the project signals a need

To adopt v9 on an existing project:
1. replace the protocol file with v9
2. let the agent re-evaluate which layers are now relevant
3. the agent may suggest adding validation criteria or coordination rules only if the project fits
4. no existing files need to change unless the agent identifies a clear improvement

---

## 9. Summary of all v9 changes

| Area | Change |
|---|---|
| Core Principles | Added 9 (independent validation), 10 (acceptance criteria as steering), 11 (scaffolding evolution) |
| Planning Layer (4.3) | Enhanced with completion contract and a `WHAT` / `HOW` boundary |
| New Layer (4.13) | Validation & Quality Assurance, default-off unless justified |
| New Layer (4.14) | Multi-Agent Coordination, default-off unless justified |
| Adaptation Rules (5.6) | Match scaffolding to current needs |
| Section 7 | Added quality criteria and scaffolding evolution to instruction file requirements |
| Section 8 | Added enablement rules for validation and multi-agent layers; keep them off unless justified |
| New Section 11 | Scaffolding Evolution — periodic re-evaluation, simplification signals, evolution records |
| Starter Snippets | Added Plan with Completion Contract (14.2), Quality Evaluation (14.6), Multi-Agent Coordination (14.7), with actionable next-iteration feedback on failed evaluations |
