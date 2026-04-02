# AI Coding Engineering Paradigm

[English](./README.en.md) | [简体中文](./README.zh-CN.md)

This repository publishes a versioned template package for generating repository-specific AI instruction files (default fallback `AGENTS.md`) and the minimal supporting directory structure they may need.

本仓库发布一套带版本管理的模板包，用于生成仓库级 AI 指令文件（默认回退文件名为 `AGENTS.md`）及其可能需要的最小支持目录结构。

GitHub README files are static, so language switching is handled through linked documents rather than a dynamic toggle.

GitHub 的 README 是静态文档，因此这里通过链接跳转实现语言切换，而不是动态按钮切换。

## Choose A Language / 选择语言

- [Read the full English README](./README.en.md)
- [阅读完整的简体中文 README](./README.zh-CN.md)

## Latest Template Package / 最新模板包

- [AGENTS.md](AGENTS.md): latest repository contract for this template repository / 本模板仓库的最新仓库契约
- [VERSIONS.md](VERSIONS.md): latest and historical template package index / 最新与历史模板版本索引
- [.ai/templates/](.ai/templates/): latest reusable starter templates / 最新可复用起始模板
- [versions/](versions/): archived public template snapshots / 历史公开模板快照
- [repo_ai_instruction_generation_protocol_v9.md](repo_ai_instruction_generation_protocol_v9.md): protocol reference (English canonical) / 协议参考文档（英文标准稿）
- [repo_ai_instruction_generation_protocol_v9.zh-CN.md](repo_ai_instruction_generation_protocol_v9.zh-CN.md): Simplified Chinese protocol companion / 协议简体中文 companion
- [repo_ai_instruction_generation_protocol_user_guide_v9.md](repo_ai_instruction_generation_protocol_user_guide_v9.md): user guide reference (English canonical) / 用户指南参考文档（英文标准稿）
- [repo_ai_instruction_generation_protocol_user_guide_v9.zh-CN.md](repo_ai_instruction_generation_protocol_user_guide_v9.zh-CN.md): Simplified Chinese user guide companion / 用户指南简体中文 companion

Starter guidance now distinguishes repository types for validation: development repositories should usually use complete layered tests, training or algorithm repositories should usually keep only as many targeted tests as practical with minimal defensive code, and other repository types should choose based on real risk.

模板起始指导现在按仓库类型区分验证策略：开发类仓库通常应采用完整的分级测试；训练或算法类仓库通常应尽量少做测试，只保留必要且有针对性的测试，并尽量减少防御性代码；其他类型仓库按实际风险决定。

## Internal Maintenance / 内部维护

- Local-only maintenance files such as `.ai/tracking.md`, `.ai/plans/`, and `.ai/decisions/` may be used to maintain this repository in local working copies, but they are not part of the published template package or remote snapshots / 本仓库在本地工作副本中可以使用这类维护文件，但它们不属于对外发布的模板包，也不应成为远端快照的依赖

## License / 许可协议

This repository is released under the terms in [LICENSE](LICENSE).

本仓库按 [LICENSE](LICENSE) 中的条款发布。
