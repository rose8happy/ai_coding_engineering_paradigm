# AI Coding Engineering Paradigm

[English](./README.en.md) | 简体中文

本仓库发布一套带版本管理的源模板包，用于在目标仓库中生成仓库专用 AI 指令文件，以及目标仓库真正需要的最小支持记忆结构。

根目录两个 v9 协议参考文档仍以英文版作为标准维护稿，同时提供结构对齐的简体中文 companion 文档。

这是一套文档包和工作模式，不是软件框架、starter codebase，也不是 orchestration platform。

## 快速开始

对大多数仓库，最简单的使用方式就是把根目录的 [repo_ai_instruction_generation_protocol_v9.md](repo_ai_instruction_generation_protocol_v9.md) 复制到目标仓库，然后让 AI 在目标仓库里完成生成。

1. 把 [repo_ai_instruction_generation_protocol_v9.md](repo_ai_instruction_generation_protocol_v9.md) 复制到目标仓库。
2. 让 AI 先检查目标仓库，再生成该仓库自己的规范 AI 指令文件和必要支持目录 / 文件。
3. 如果目标仓库没有更强信号，AI 可以默认使用 `AGENTS.md`；如果仓库已有更合适的 canonical filename，就应沿用现有约定。
4. 输出完成后，默认删除复制进去的 protocol / user guide bootstrap 文件；只有在你明确要求时才保留。

可直接对 AI 这样说：

```text
请使用 repo_ai_instruction_generation_protocol_v9.md 检查这个仓库，尽可能从仓库证据推断信息，先起草最终 AI 指令文件和必要支持文件，只在确实高影响且无法推断时再问我少量问题。完成后，除非我明确要求保留，否则删除复制进来的 protocol bootstrap 文件。
```

如果你还需要更多背景说明、prompt 示例或可选层说明，再一起复制 [repo_ai_instruction_generation_protocol_user_guide_v9.md](repo_ai_instruction_generation_protocol_user_guide_v9.md)。

## 你会得到什么

- 一个目标仓库自己的 canonical AI instruction file
- 只在确有价值时才启用的 tracking、plan、decision、spec 等支持层
- 优先复用目标仓库现有路径和约定的职责映射，而不是机械照搬本仓库
- 在默认情况下不留下多余 bootstrap 文件

## 什么时候适合用

这个模板特别适合以下场景：

- 工作会跨越多个会话、贡献者或交接
- 仓库容易发生上下文丢失、重复调研或规则漂移
- 你希望 AI 先检查仓库再生成规则，而不是手写一份通用说明
- 你需要把稳定规则和易变工作状态分开

## 本仓库公开提供什么

- [README.md](README.md)：中文首页与最快使用入口
- [README.en.md](README.en.md)：完整英文 companion
- [VERSIONS.md](VERSIONS.md)：当前推荐版本与历史快照索引
- [repo_ai_instruction_generation_protocol_v9.md](repo_ai_instruction_generation_protocol_v9.md)：协议参考文档（英文标准稿）
- [repo_ai_instruction_generation_protocol_v9.zh-CN.md](repo_ai_instruction_generation_protocol_v9.zh-CN.md)：协议简体中文 companion
- [repo_ai_instruction_generation_protocol_user_guide_v9.md](repo_ai_instruction_generation_protocol_user_guide_v9.md)：用户指南（英文标准稿）
- [repo_ai_instruction_generation_protocol_user_guide_v9.zh-CN.md](repo_ai_instruction_generation_protocol_user_guide_v9.zh-CN.md)：用户指南简体中文 companion
- [.ai/templates/](.ai/templates/)：可复用 starter templates
- [versions/](versions/)：历史公开快照

## 版本与快照

- 仓库根目录和 `.ai/templates/` 始终表示当前推荐版本。
- 历史公开快照位于 `versions/<release-id>/`，并由 [VERSIONS.md](VERSIONS.md) 统一索引。
- 模板包版本号使用日期格式 `YYYY-MM-DD`；同日后续修订使用 `YYYY-MM-DD-rN`。
- `repo_ai_instruction_generation_protocol_v9.md` 里的 `v9` 表示协议代际，不表示模板包发布日期。
- 模板包发布 tag 应使用 `template-<release-id>`；现有 `V9` tag 仍表示协议里程碑。

## 进一步说明

- 对大多数目标仓库，你只需要复制协议文档本身；不需要机械复制本仓库的 README、用户指南或 starter 文件。
- [repo_ai_instruction_generation_protocol_user_guide_v9.md](repo_ai_instruction_generation_protocol_user_guide_v9.md) 更适合在你需要更多示例 prompt、流程解释或可选层判断时再一起参考。
- [.ai/templates/](.ai/templates/) 是 starter，不是默认整包复制清单；只有在 agent 判断这些层确实有价值时才使用。

## 许可协议

本仓库按 [LICENSE](LICENSE) 中的条款发布。
