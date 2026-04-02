# Repository Working Instructions

## Purpose And Scope

- This repository maintains a versioned source template package for generating repository-specific AI instruction files and the minimal supporting directory structure they need.
- AI work here should keep the latest public template assets, their archived releases, and the repository's own maintenance memory aligned and easy to distinguish.
- This is not a runnable software project. Do not introduce `src/`, CI workflows, or other code-project scaffolding unless explicitly requested.

## Working Directory Rules

- Treat the repository root public files and `.ai/templates/` as the latest published template source bundle.
- Treat `versions/` as archived public release snapshots. Add new release directories instead of mutating old ones unless you are explicitly correcting published history.
- Local maintenance work may use `.ai/tracking.md`, `.ai/plans/`, and `.ai/decisions/`, but those files are not part of the published template package and should not be required by remote snapshots.
- Do not assume target repositories must reuse this repository's exact paths; the protocol remains responsibility-based rather than path-prescriptive.

## Repository Map

- [README.md](README.md): root language entry point and latest public navigation.
- [README.en.md](README.en.md): full English overview of the latest template package.
- [README.zh-CN.md](README.zh-CN.md): full Simplified Chinese overview of the latest template package.
- [VERSIONS.md](VERSIONS.md): public version index for latest and archived template releases.
- [repo_ai_instruction_generation_protocol_v9.md](repo_ai_instruction_generation_protocol_v9.md): protocol reference source.
- [repo_ai_instruction_generation_protocol_v9.zh-CN.md](repo_ai_instruction_generation_protocol_v9.zh-CN.md): full Simplified Chinese companion to the protocol reference.
- [repo_ai_instruction_generation_protocol_user_guide_v9.md](repo_ai_instruction_generation_protocol_user_guide_v9.md): protocol user guide reference.
- [repo_ai_instruction_generation_protocol_user_guide_v9.zh-CN.md](repo_ai_instruction_generation_protocol_user_guide_v9.zh-CN.md): full Simplified Chinese companion to the protocol user guide.
- [.ai/templates/](.ai/templates/): latest reusable starter templates.
- [versions/](versions/): archived public template snapshots by release id.
- Local-only maintenance files: `.ai/tracking.md`, `.ai/plans/`, and `.ai/decisions/` when a working clone keeps repository-maintenance memory locally.

## Commands

- Setup: `N/A`
- Build: `N/A`
- Test: `N/A`
- Lint / Format / Validate: `N/A`
- Run: `N/A`

## Editing And Change Rules

- Keep `AGENTS.md`, the README variants, `.ai/templates/`, and any repository-specific direct-use guidance in the user guide aligned when template boundaries or recommended starter usage change.
- Keep the root English v9 protocol documents as the canonical maintenance sources. Maintain `repo_ai_instruction_generation_protocol_v9.zh-CN.md` and `repo_ai_instruction_generation_protocol_user_guide_v9.zh-CN.md` as synchronized Simplified Chinese companion translations; when wording differs, the English source is authoritative.
- Keep public release assets separate from internal maintenance memory. Do not publish `.ai/tracking.md`, `.ai/plans/`, or `.ai/decisions/` inside versioned template snapshots.
- For non-trivial local maintenance work, create a plan file named `YYYY-MM-DD-<slug>.md` under `.ai/plans/`, record durable non-obvious decisions under `.ai/decisions/`, and update `.ai/tracking.md` when work starts or closes.
- Preserve the two root v9 protocol documents as reference material unless explicitly asked to replace or remove them.
- Do not create mirrored model-specific instruction files such as `CLAUDE.md`, `GEMINI.md`, or `COPILOT_INSTRUCTIONS.md` unless explicitly requested.

## Documentation And Memory Map

- Keep stable, repository-wide AI rules in this file.
- Keep public release indexing and version interpretation in [VERSIONS.md](VERSIONS.md).
- The published template package consists of `AGENTS.md`, `README.md`, `README.en.md`, `README.zh-CN.md`, `repo_ai_instruction_generation_protocol_v9.md`, `repo_ai_instruction_generation_protocol_v9.zh-CN.md`, `repo_ai_instruction_generation_protocol_user_guide_v9.md`, `repo_ai_instruction_generation_protocol_user_guide_v9.zh-CN.md`, and `.ai/templates/`.
- Keep volatile repository-maintenance state in local-only `.ai/tracking.md`, `.ai/plans/`, and `.ai/decisions/` files when a working clone uses them.
- Do not put internal task state into the README files, `VERSIONS.md`, or the root protocol reference documents.

## Local Maintenance Practice

- Local current-status file: `.ai/tracking.md` when a working clone keeps repository-maintenance memory locally
- Local planning directory: `.ai/plans/`
- Local decision directory: `.ai/decisions/`
- Keep these files out of the published template bundle and `versions/` snapshots.

## Git And Traceability

- Keep documentation changes and related maintenance-memory updates aligned in the same change set when they belong together.
- When the latest public template assets change materially, update `VERSIONS.md` and add a matching `versions/<release-id>/` snapshot.
- Template release ids use `YYYY-MM-DD`. Same-day follow-up releases use `YYYY-MM-DD-rN`.
- Git tags for published template releases should use `template-<release-id>`. The existing `V9` tag remains a protocol milestone and should not be reused as a template-release tag.
- Use Git history, `VERSIONS.md`, and archived snapshots together: Git records change order, `VERSIONS.md` resolves naming, and `versions/` preserves the public release shape.

## Maintenance

- This file is a living repository contract.
- Update it when repository structure, template package boundaries, versioning rules, or local maintenance practice change.
- Keep the repository doc-first and the process overhead only as heavy as needed to prevent future AI or human confusion.
