# Template Versions

## Current Recommended Release

- Template package release: `2026-04-02-r6`
- Protocol baseline: `v9`
- The latest recommended source bundle lives at the repository root and under [.ai/templates/](.ai/templates/).
- Historical public snapshots live under [versions/](versions/).

## Version Model

- Protocol filenames such as `repo_ai_instruction_generation_protocol_v9.md` keep the protocol-generation label. `v9` identifies the protocol line, not the template package release date.
- Template package releases use `YYYY-MM-DD`.
- Same-day follow-up releases use `YYYY-MM-DD-rN`.
- Archive directories use `versions/<release-id>/`.
- Git tags for published template releases should use `template-<release-id>`.

## Historical Note

- The existing Git tag `V9` marks the protocol milestone where the v9 reference documents were introduced.
- `V9` is not a template package release tag and should not be reused for future template releases.

## Release Index

- `2026-04-02-r6`: trims the Chinese homepage surface by removing redundant published-bundle inventory and matching framing, aligns `README.en.md` to the same compact homepage shape, and archives the updated public bundle under [versions/2026-04-02-r6/](versions/2026-04-02-r6/).
- `2026-04-02-r5`: removes the source-repository `AGENTS.md` and duplicate `README.zh-CN.md` from the published template bundle, promotes `README.md` to a full Chinese quick-start homepage, aligns `README.en.md` and both root user guides with the copy-the-protocol bootstrap flow, and archives the updated public bundle under [versions/2026-04-02-r5/](versions/2026-04-02-r5/).
- `2026-04-02-r4`: removes source-repository local-maintenance guidance from the public README and user guide documents, trims `README.md` back to a bilingual entry page, and archives the updated public template bundle under [versions/2026-04-02-r4/](versions/2026-04-02-r4/).
- `2026-04-02-r3`: adds project-type-sensitive validation and defensive-coding guidance to the main instruction starter, README variants, and user guides, then archives the updated public template bundle under [versions/2026-04-02-r3/](versions/2026-04-02-r3/).
- `2026-04-02-r2`: adds Simplified Chinese companion documents for the two root v9 protocol references and archives the updated public template bundle under [versions/2026-04-02-r2/](versions/2026-04-02-r2/).
- `2026-04-02`: first formal dated template package release. The latest public files live at the repository root and under [.ai/templates/](.ai/templates/), and the matching archived snapshot lives at [versions/2026-04-02/](versions/2026-04-02/).
