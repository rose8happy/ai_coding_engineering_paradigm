# AI Coding Engineering Paradigm

[Home](./README.md) | English | [简体中文](./README.zh-CN.md)

This repository publishes a versioned source template package for generating repository-specific AI instruction files, with `AGENTS.md` as the default fallback filename when the target repository has no stronger signal.

The two root v9 protocol documents remain canonically maintained in English, and the repository also publishes aligned Simplified Chinese companion documents for readers who prefer Chinese.

It is meant for repositories where AI has to stay coherent across many sessions, contributors, and handoffs by leaving durable, repository-visible working memory behind.

This is a documentation package and operating pattern, not a software framework, starter codebase, or orchestration platform.

## Why This Exists

AI can be productive in short conversations and still perform poorly over long-running work.

Common failure modes are predictable:

- it forgets decisions, assumptions, and background context
- it repeats discovery work that was already done
- it loses track of current status and the real next step
- it makes handoffs between sessions or collaborators harder than they should be
- it leaves behind work that is difficult to audit or maintain

For long-lived tasks, the problem is usually not raw model capability alone. Too much project memory stays trapped in transient chat context.

This template package addresses that by helping an agent generate a canonical repository instruction file plus only the supporting memory structure the target repository actually needs.

## Core Idea

Treat repository documents as externalized working memory.

Instead of expecting the model to remember everything from prior chats, require it to keep important state in files that humans and future AI sessions can read:

- stable rules
- current status when needed
- task plans when work is non-trivial
- durable decisions when reasoning will matter later
- reusable starter templates

The repository becomes the memory surface. Context survives session resets, long pauses, model upgrades, and human or AI handoffs.

## What The Template Produces

The generated repository should end up with only the layers that are justified by its actual workflow.

- one canonical AI instruction file for the target repository
- optional current-status, planning, and decision layers when they prevent real context loss
- an optional `spec` / requirements layer only for longer, ambiguous, review-heavy, or multi-agent tasks
- path choices that fit the target repository instead of mechanically mirroring this one

The point is to keep stable rules separate from volatile working state without forcing a fixed directory tree.

## Where This Helps

This template is useful when work is long-running, iterative, or easy to lose track of.

Typical cases include:

- software development that spans many sessions or contributors
- model training and evaluation loops
- research and experiment-heavy repositories
- projects with frequent context switching
- human and AI collaboration that depends on clean handoffs
- work that needs traceability and maintainable decision history

## Repository Contents

This repository keeps the latest recommended template source bundle at the repository root and under `.ai/templates/`.

- Latest public template assets: `AGENTS.md`, `README.md`, `README.en.md`, `README.zh-CN.md`, `repo_ai_instruction_generation_protocol_v9.md`, `repo_ai_instruction_generation_protocol_v9.zh-CN.md`, `repo_ai_instruction_generation_protocol_user_guide_v9.md`, `repo_ai_instruction_generation_protocol_user_guide_v9.zh-CN.md`, and `.ai/templates/`
- [VERSIONS.md](VERSIONS.md): version index for the current release and historical snapshots
- [versions/](versions/): archived public snapshots such as `versions/2026-04-02/`
- Local-only maintenance files such as `.ai/tracking.md`, `.ai/plans/`, and `.ai/decisions/` may be used to maintain this repository in local working copies, but they are not part of the published template package or remote snapshots.

The same document-driven approach can be used locally to maintain this repository, but those maintenance files are intentionally kept out of the public source bundle and remote snapshots. A target repository should not copy them by default.

## How Versioning Works

- Protocol filenames such as `repo_ai_instruction_generation_protocol_v9.md` keep the protocol-generation label. `v9` identifies the protocol line, not the template package release date.
- Template package releases use date-based ids such as `2026-04-02`.
- Same-day follow-up releases use `2026-04-02-r2`, `2026-04-02-r3`, and so on.
- The repository root and `.ai/templates/` always reflect the latest recommended release. Historical public snapshots live under `versions/<release-id>/`.
- Git tags for published template releases should use `template-<release-id>`. The existing `V9` tag remains a historical protocol milestone.

## How To Use This Template

1. Start from [VERSIONS.md](VERSIONS.md). Use the repository root and `.ai/templates/` for the latest release, or pick a historical snapshot from `versions/<release-id>/`.
2. Generate or refresh one canonical AI instruction file for the target repository. Use `AGENTS.md` only as the default fallback filename when the target repository has no stronger signal.
3. Add only the supporting memory layers the target repository actually needs.
4. Reuse existing target-repository paths when they already work. Do not copy this repository mechanically.
5. If you maintain a local clone of this source repository, treat `.ai/tracking.md`, `.ai/plans/`, and `.ai/decisions/` as optional local maintenance files for the source repository itself, not as public template assets to copy by default.

## License

This repository is released under the terms in [LICENSE](LICENSE).
