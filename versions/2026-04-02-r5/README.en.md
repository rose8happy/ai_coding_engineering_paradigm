# AI Coding Engineering Paradigm

[中文](./README.md) | English

This repository publishes a versioned source template package for generating repository-specific AI instruction files plus only the supporting memory structure that the target repository actually needs.

The two root v9 protocol reference documents remain canonically maintained in English, with aligned Simplified Chinese companion documents for readers who prefer Chinese.

This is a documentation package and operating pattern, not a software framework, starter codebase, or orchestration platform.

## Quick Start

For most repositories, the simplest workflow is to copy the root [repo_ai_instruction_generation_protocol_v9.md](repo_ai_instruction_generation_protocol_v9.md) into the target repository and let AI run the generation flow there.

1. Copy [repo_ai_instruction_generation_protocol_v9.md](repo_ai_instruction_generation_protocol_v9.md) into the target repository.
2. Ask AI to inspect the target repository first, then generate that repository's own canonical AI instruction file plus only the supporting directories / files that are justified.
3. Use `AGENTS.md` only as the default fallback filename when the target repository has no stronger signal; otherwise keep the repository's existing canonical convention.
4. After finalization, delete the copied protocol / user-guide bootstrap files unless you explicitly want them kept.

You can give AI a prompt like this:

```text
Use repo_ai_instruction_generation_protocol_v9.md to inspect this repository, infer as much as possible from repository evidence, draft the final AI instruction file and any justified supporting files, and ask me only the small number of unresolved high-impact questions. After finalization, delete the copied protocol bootstrap files unless I explicitly ask to keep them.
```

If you want more background, prompt examples, or optional-layer guidance, copy [repo_ai_instruction_generation_protocol_user_guide_v9.md](repo_ai_instruction_generation_protocol_user_guide_v9.md) as well.

## What You Get

- one canonical AI instruction file for the target repository
- tracking, plan, decision, or `spec` layers only when they provide real value
- responsibility-to-path mapping that prefers the target repository's existing structure instead of mechanically copying this one
- minimal leftover bootstrap material by default

## Where This Helps

This template is especially useful when:

- work spans multiple sessions, contributors, or handoffs
- repositories keep losing context or repeating discovery work
- you want AI to inspect the repository before writing rules
- stable rules and volatile work state should be kept separate

## What This Repository Publishes

- [README.md](README.md): Chinese homepage and fastest entrypoint
- [README.en.md](README.en.md): full English companion
- [VERSIONS.md](VERSIONS.md): current recommended release and historical snapshot index
- [repo_ai_instruction_generation_protocol_v9.md](repo_ai_instruction_generation_protocol_v9.md): protocol reference (English canonical)
- [repo_ai_instruction_generation_protocol_v9.zh-CN.md](repo_ai_instruction_generation_protocol_v9.zh-CN.md): Simplified Chinese protocol companion
- [repo_ai_instruction_generation_protocol_user_guide_v9.md](repo_ai_instruction_generation_protocol_user_guide_v9.md): user guide (English canonical)
- [repo_ai_instruction_generation_protocol_user_guide_v9.zh-CN.md](repo_ai_instruction_generation_protocol_user_guide_v9.zh-CN.md): Simplified Chinese user guide companion
- [.ai/templates/](.ai/templates/): reusable starter templates
- [versions/](versions/): archived public snapshots

## Versioning And Snapshots

- The repository root and `.ai/templates/` always reflect the current recommended release.
- Historical public snapshots live under `versions/<release-id>/` and are indexed from [VERSIONS.md](VERSIONS.md).
- Template package releases use `YYYY-MM-DD`; same-day follow-up releases use `YYYY-MM-DD-rN`.
- The `v9` in `repo_ai_instruction_generation_protocol_v9.md` identifies the protocol line, not the template package release date.
- Git tags for template releases should use `template-<release-id>`. The existing `V9` tag remains a protocol milestone.

## Additional Notes

- Most target repositories only need the protocol file itself; they do not need a mechanical copy of this repository's README files, user guide, or starter templates.
- [repo_ai_instruction_generation_protocol_user_guide_v9.md](repo_ai_instruction_generation_protocol_user_guide_v9.md) is best used when you want extra prompt wording, process explanation, or optional-layer guidance.
- [.ai/templates/](.ai/templates/) contains starters, not a default bundle to copy wholesale. Use them only when the agent determines those layers are actually useful.

## License

This repository is released under the terms in [LICENSE](LICENSE).
