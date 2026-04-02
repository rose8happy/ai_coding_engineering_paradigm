# 仓库 AI 指令生成协议用户指南 v9

[English](./repo_ai_instruction_generation_protocol_user_guide_v9.md)

> 本中文文件是 `repo_ai_instruction_generation_protocol_user_guide_v9.md` 的同步 Simplified Chinese companion；如有歧义，以英文标准稿为准。

本指南解释应如何在实际工作中使用 `repo_ai_instruction_generation_protocol_v9.md`。

它面向这样的读者：希望 agent 先检查仓库、尽可能从证据推断信息、只提少量高影响问题，然后生成仓库专用的 AI 指令文件，以及真正有价值的支持记忆文件。

这是一份**面向任务的指南**。它重点说明：
- 何时使用该协议
- 如何运行它
- agent 应该做什么
- 如何决定启用哪些可选层
- 如何避免引入不必要的流程

文末还包含一份面向 v8 用户的 v9 变更摘要。

---

## 1. 这个协议是做什么的

当你想创建或刷新仓库专用的 AI 指令文件时，可以使用这个协议，例如：
- `AGENTS.md`
- `CLAUDE.md`
- `GEMINI.md`
- `COPILOT_INSTRUCTIONS.md`
- `.github/copilot-instructions.md`

本协议**对文件名不设限制**，也**对目录布局不设限制**。它不假定固定的仓库结构。相反，它要求 agent 先检查仓库，从证据中尽可能推断信息，再把职责映射到仓库已经存在的结构上。

换句话说：
- 它**不是**一份可以机械粘贴到每个仓库里的静态模板
- 它**是一套**根据仓库真实结构和工作流生成仓库专用指令的流程

---

## 2. 协议应该产出什么

正确使用时，本协议应产出：

1. 仓库的主 AI 指令文件，并使用最合适的文件名
2. 对该仓库来说有明确正当性的支持记忆文件
3. 只有在确实会影响结果时，才保留一份简短的未决假设 / 问题清单
4. 在最终完成后清理复制进来的 protocol 文件和 user guide 文件，除非用户明确要求保留

这个协议的目标是创建真实输出，而不只是描述“以后别人应该去创建什么”。

---

## 3. 什么时候使用这个协议

适合在以下情况使用：
- 你第一次为一个仓库建立 AI 指令
- 现有指令文件已经偏离了仓库现实
- 仓库已经增长到需要更清晰的规则、结构或记忆文件
- 仓库里已经有多个 AI 指令文件，而你需要决定哪个是 canonical，或它们应如何保持同步
- 你希望 agent 从仓库证据中推断结构，而不是从头手写说明

通常**不需要**在以下情况使用：
- 你只是在一个已经不错的指令文件上做一个很小的一行改动
- 仓库太小，以至于手写一个很短的指令文件比跑完整流程更容易
- 仓库已经有准确、被维护、且仓库专用的 AI 指令，并且工作流没有发生实质变化

---

## 4. 快速开始

对大多数仓库来说，实用工作流是：

1. 把 `repo_ai_instruction_generation_protocol_v9.md` 放进仓库，或以其他方式让 agent 可以读取到它。
2. 要求 agent 检查仓库并生成仓库最终的 AI 指令文件。
3. 先让 agent 检查仓库，再回答问题。
4. 让 agent 起草仓库专用指令文件，并提出它认为有必要的支持文件。
5. 只回答那些 agent 无法从仓库证据推断出来的少量高影响问题。
6. 让 agent 最终完成输出，并在你不要求保留的前提下清理复制进来的 protocol 文件。

一个好的结果通常应满足：
- 复用仓库现有约定
- 除非仓库真的需要，否则不发明额外流程
- 只创建真正有价值的文件
- 把稳定规则留在主指令文件里，只在有正当理由时把易变状态放进别处

如果你直接把本仓库当作源模板包使用，最新推荐版本位于仓库根目录和 `.ai/templates/` 下。
可以通过 `VERSIONS.md` 在当前版本和 `versions/<release-id>/` 下的归档快照之间进行选择。
本仓库自己的 `.ai/tracking.md`、`.ai/plans/` 和 `.ai/decisions/` 是源仓库的维护记忆，不是应该盲目复制出去的 starter 文件。

---

## 5. agent 应该做什么

协议要求 agent 按以下顺序工作。

### 5.1 先检查

在提问之前，agent 应先检查仓库中的证据，例如：
- `README*`
- 顶层目录
- `package.json`、`pyproject.toml`、`Cargo.toml`、`go.mod`、`pom.xml`、`build.gradle*` 等 manifest
- `Makefile`、`justfile`、`Taskfile.yml`、scripts 与 helper commands
- `.github/workflows/` 这类 CI 文件
- docs、notes、reports、plans、experiments 与 runbooks
- 已有 AI 指令文件
- remote execution、长时间运行作业、schedulers、artifact 目录、evaluation pipeline 或 multi-agent workflow 的证据

agent 应优先依赖仓库里能直接看见的事实，而不是先盘问用户。

### 5.2 推断一切可推断信息

从仓库证据中，agent 应尽可能多地推断信息，在可见时包括：
- 项目名称
- 重要路径
- setup、build、test、lint、run 与 validation 命令
- 仓库形态（simple repo、monorepo、research repo、infra repo、mixed repo 等）
- 是否存在长时间运行任务或 remote execution
- 仓库是否已经有一套可运转的记忆 / 文档系统
- 是否已经存在多个指令文件或 sync 规则
- validation workflow 或 multi-agent coordination 是否已经是项目现实的一部分

### 5.3 提问前先起草

在向用户提任何问题之前，agent 应先起草：
- 最可能的最终指令文件结构
- 从职责层到仓库路径的映射建议
- 明显有必要的支持文件
- 一份简短的高影响未决歧义清单

这很重要，因为它能防止 agent 在过早阶段提出宽泛而低价值的问题。

### 5.4 只问少量高影响问题

协议在这里是刻意严格的。agent 只应询问那些会实质影响结构、安全或工作流的问题。

好的例子：
- 当多个文件名都合理时，哪个指令文件名应是 canonical
- 多个指令文件是否应保持同步
- 仓库之外是否存在 remote servers、shared GPUs 或 schedulers
- 生成的支持文件是否应提交
- 是否存在隐藏的 approval、安全或安全性规则
- 当仓库里看不清楚时，是否希望引入 multi-agent coordination 或独立 validation

不好的例子：
- manifest 已经写清楚了，还要问使用什么 package manager
- 仓库结构已经很清楚了，还要问 source directory 在哪里
- scripts 或 CI 已经有命令了，还要问 build / test 命令是什么

### 5.5 最终完成并清理

在用户回答剩余少量问题后，agent 应：
- 完成最终指令文件
- 创建有充分理由的支持文件
- 尽可能移除临时 assumption marker
- 删除复制进来的 protocol 文件和 user guide 文件，除非用户明确要求保留

---

## 6. 如何决定使用哪个指令文件名

协议并不强制要求某一个固定文件名。正确选择取决于仓库现实。

### 当已有约定在工作时，优先沿用

如果仓库已经把以下某个文件当作真正的 AI 指令文件使用，就应继续沿用它，除非有非常强的理由要改：
- `AGENTS.md`
- `CLAUDE.md`
- `GEMINI.md`
- `COPILOT_INSTRUCTIONS.md`
- `.github/copilot-instructions.md`

### 仅把 `AGENTS.md` 当默认回退

只有在没有更强信号时，协议才默认使用 `AGENTS.md`。

### 为什么模板仓库可能使用中性的 starter 文件名

当一个仓库提供可复用 starter files 时，主指令 starter 往往更适合使用像 `main-instruction.md` 这样的中性文件名。
这样可以避免暗示生成结果一定必须命名为 `AGENTS.md`。
目标仓库最后仍应落到它真实工作流所需要的 canonical filename 上。

### 如果已经存在多个指令文件

不要盲目覆盖。先确定：
- 哪个文件是 canonical
- 其他文件是否只是 mirror
- model-specific 差异是否是有意设计
- 是否应记录同步规则

如果真实工作流中不止一个指令文件，那仓库可能需要一条**多指令同步规则**。

---

## 7. 如何选择支持文件

协议是按职责层工作，而不是按固定路径工作。agent 只应在支持文件确实有明确价值时才创建它们。

如果你直接使用本仓库，最新的核心可复用 starter 集位于 `.ai/templates/`：
- `main-instruction.md`
- `tracking.md`
- `plan.md`
- `decision.md`

把它们当成 starter，不要机械地整包复制。
最新版本保留在仓库根目录和 `.ai/templates/`；历史公开快照位于 `versions/<release-id>/`，并由 `VERSIONS.md` 统一索引。
它们展示的是常见职责层形态，但目标仓库在已有合适路径和约定时，仍应优先复用自己的结构。
不要默认复制本仓库的 `.ai/tracking.md`、`.ai/plans/` 或 `.ai/decisions/`。这些文件是用来维护源仓库本身的。
对于长任务、模糊任务、review-heavy 任务或 multi-agent 任务，可选的 `spec.md` 或 `requirements.md` starter 也能帮助把任务契约和执行 plan 分开。
应把它视为高级 planning 支持，而不是默认核心集的一部分。

### 核心模板集的推荐顺序

当一个仓库需要的不止一个指令文件时，实用顺序通常是：
1. 从中性的 main-instruction starter 开始，并把它重命名为仓库真实的 canonical instruction filename
2. 只有在 current-status 可见性值得维护时才加 tracking
3. 对中型或大型工作使用 plan starter；如果任务契约复杂，就再加一个单独的 `spec` / requirements 文档，让 plan 保持专注于执行
4. 当非显然决策需要持久记录时，使用 decision starter

### 在更大或更长周期的工作中通常有价值的层

当工作跨越多个会话、任务或贡献者时，这些层往往值得启用：
- **Current Status**：活跃工作、blocker 和下一步的紧凑 dashboard
- **Planning**：中型或大型任务的 plan
- **Active Task Memory**：长任务的持久工作记忆
- **Archive**：一个存放已完成任务历史的位置，避免 active files 越来越长
- **Decision Records**：重大权衡的持久记录
- **Reports / Deep Analysis**：调查、评估与 writeup
- **Runbooks**：可重复执行的操作流程
- **Handoffs**：跨会话或跨 agent 的连续性

### 在非常小的仓库里通常没有必要

在以下情况下，不要创建一片 tracking、planning、status、archive 与 coordination 文件森林：
- 仓库非常小
- 工作很琐碎
- 团队实际上并不会使用这些文件
- 维护成本会高于收益

协议明确偏好**最小但有效的结构**。

---

## 8. 使用 completion contract 做 planning

对于中型或大型任务，Planning Layer 应在执行开始前定义好 **completion contract**。

在很多仓库中，plan 本身就足够了。
对于更长或 multi-agent 的任务，把任务契约放进单独的 `spec` / requirements 文档里，并让 plan 专注于顺序、检查点、validation 与 rollback，会更清晰。

一个好的 completion contract 应包含：
- **可观察的 acceptance criteria**
- **validation method**
- **scope boundary**
- 一条明确声明：必须交付的 **what** 是什么，但除非仓库要求固定路径，否则不要过度规定 **how**

### 为什么这很重要

没有 completion contract 时，执行工作的人 / agent 与评审工作的人 / agent 往往会各自按照不同的“done”定义工作。这个落差是返工的常见来源。

### 例子

弱的 criterion：
- 把文档写得更好

更好的 criterion：
- 新 clone 下的安装步骤完整可用
- quick start 里的每条命令都已在仓库中验证过
- troubleshooting 章节覆盖了两个已知 setup 失败场景
- 所有内部链接都可正确解析

---

## 9. 可选层：什么时候启用

最常见的错误，就是因为协议支持，就把所有可选层都启用。不要这么做。

### 9.1 Validation & Quality Assurance

这一层**默认关闭**。当输出质量具有主观性、多维度，或历史上经常不可靠时，再启用它。

适合的场景：
- UI、UX、design、写作或其它“done”并不只是二值正确性的工作
- 输出经常“看起来完成了”，但在真实使用里失败的项目
- 由于 acceptance criteria 模糊，导致反复迭代的工作流
- 真实验证是可行的场景，例如运行 app、走通流程或执行结构化检查

通常跳过它的场景：
- 正确性是二值的，而且现有测试已经覆盖得很好
- 仓库很小，流程也很轻
- 更轻量的验证已经能捕捉相关失败模式

### 9.2 Multi-Agent Coordination

这一层**默认关闭**。只有当多个 agent 确实在同一代码库或工作流上协同时才启用。

适合的场景：
- planner / implementer / evaluator 工作流
- orchestration harness
- agent 之间存在真实 handoff
- 清晰分离的职责边界

通常跳过它的场景：
- 只有一个 agent 在工作
- 人类已经提供了所有必要协调
- 仓库太小，不值得引入额外 coordination 文件

### 9.3 Remote Execution

只有当项目真的使用以下内容时才启用：
- remote machines
- shared servers
- GPUs
- schedulers
- remote artifact locations

如果 remote execution 并不真实存在，就不要加 remote execution ceremony。

### 9.4 多指令同步

只有当仓库真的依赖不止一个 AI 指令文件时才启用。

---

## 10. 如何让主指令文件保持有用

主 AI 指令文件应包含**稳定的仓库指导**，例如：
- 仓库 purpose 与 scope
- 重要路径
- working directory rules
- 在可见时写明 setup、build、test、lint 和 run 命令
- 在相关时写明 validation expectations
- editing 与 change rules
- documentation 与 memory mapping
- 只有在这些工作流真实存在时，才写入 sync、remote execution 或 coordination 规则
- 一条解释该文件是活文档的 maintenance note

它**不应**变成一个巨大的“什么都往里塞”的地方，用来放：
- 临时任务状态
- 本应属于单独 `spec` / requirements 文档的任务契约
- 冗长 investigation
- experiment logs
- 重复 handoff notes
- 过时的 planning 细节

当信息是易变的、详细的或任务专属的时，只有在仓库确实会受益时，才把它移入支持文件。

在模板仓库里，这也是为什么可复用的 main-instruction starter 应保持中性命名。
模板文件是 source asset；生成出的仓库文件应使用适合目标仓库真实工作流的 canonical filename。

---

## 11. 常见模式

### Pattern A：小型单仓库应用

通常足够：
- 一个主 AI 指令文件
- 如果工作跨会话，可能再加一个紧凑 status 或 plan 文件

通常不需要：
- coordination 文件
- quality rubrics
- run registries
- 沉重的 handoff 机制

### Pattern B：持续活跃的产品仓库

通常有价值：
- 主指令文件
- 紧凑 tracking / status 文件
- 中型或大型工作的 per-task plans
- 重大权衡的 decision records
- 一个 archive 区域，防止 active files 无限增长

### Pattern C：research、training 或 evaluation 仓库

通常有价值：
- 主指令文件
- experiment / run tracking
- reports 或 analysis 文档
- 如果作业在别处运行，则加入 remote execution notes
- 如果结果微妙或误判成本高，则使用更强的 validation rules

### Pattern D：multi-agent 仓库或 harness-based workflow

通常有价值：
- 主指令文件
- 清晰的 role definitions
- 跨 agent 使用的 contract files 或 checklists
- 承载可执行迭代 delta 的 feedback files
- handoff 规则和迭代上限

但即便如此，也只应保留那些真正能改善结果的最小 coordination。

---

## 12. 应避免的常见错误

### 错误 1：把协议当成固定模板

协议定义的是职责，而不是强制目录树。在可能时应复用仓库已有结构。

### 错误 2：在检查仓库前问太多问题

agent 应先检查、先推断。问题只用于尚未解决的高影响歧义。

### 错误 3：默认创建所有可选文件

文件更多并不自动意味着指令更好。额外 scaffolding 必须由真实工作流需求来证明。

### 错误 4：把易变状态塞进主指令文件

主指令文件应保持稳定和持久。临时状态应放在别处，而且只在必要时才放。

### 错误 5：永远保留过时的 scaffolding

如果某一层已经不再防止真实失败模式，就应简化或删除它。

---

## 13. scaffolding 应该演化

协议假设流程应随着时间推移不断适配。

当出现以下情况时，应重新评估 scaffolding：
- 仓库发生了明显变化
- 团队或工作流发生变化
- 引入了明显更强的模型
- 某个文件或流程层一直在维护，却几乎没人看
- 维护成本已经超过它提供的价值

适合简化的好信号：
- handoff 文件从来没人读
- tracking 文件永远是过时的
- validation layer 从不改变任何决策
- coordination 规则只存在于纸面上，实际工作流里并不用

目标不是最大结构。目标是**刚好足以稳定防止质量流失或上下文流失的最小结构**。

---

## 14. 你可以直接给 agent 的实用 prompts

### 首次建立

> 使用 `repo_ai_instruction_generation_protocol_v9.md` 检查这个仓库，尽可能从仓库证据推断信息，起草最终 AI 指令文件和有正当性的支持文件，然后只向我提出少量仍未解决的高影响问题。

### 刷新已有指令文件

> 使用 `repo_ai_instruction_generation_protocol_v9.md` 刷新这个仓库的 AI 指令文件。尽可能复用现有仓库结构，识别任何已经过时的指导内容，并且只建议那些明显有价值的支持文件。

### 已经存在多个指令文件

> 检查这个仓库，并判断现有 AI 指令文件是否已经有 canonical source，或者是否需要同步规则。不要盲目覆盖。

### 现有项目，但保持轻量 scaffolding

> 使用 `repo_ai_instruction_generation_protocol_v9.md`，但保持 scaffolding 尽量轻。只有当仓库证据明确证明有必要时，才增加 planning、tracking、validation 或 coordination 文件。

---

## 15. v9 有哪些变化

如果你已经熟悉 v8，下面是主要新增点。

### 15.1 新增了三个原则

v9 新增了三个核心思想：
- **独立验证优于自我评估**
- **acceptance criteria 会引导结果**
- **scaffolding 会随能力演进**

### 15.2 Planning 现在包含 completion contract

Planning Layer 现在会在执行前通过以下内容显式定义“done”：
- 可观察的 acceptance criteria
- validation method
- scope boundary
- 一个 `WHAT` / `HOW` 边界，避免无谓地锁死实现路径

在模板仓库中，对于中型任务，这个契约可以直接写在 plan 里；对于更复杂的任务，则可以放进与 plan 配套的单独 `spec` / requirements 文档中。

### 15.3 新增可选的 Validation & Quality Assurance 层

这一层为输出质量具有主观性或多维度的场景，增加了结构化质量维度、评分与验证指导。

### 15.4 新增可选的 Multi-Agent Coordination 层

这一层为多个 agent 真实协同时的场景增加了 role definition、文件式通信模式、handoff 规则和迭代指导。

### 15.5 现在明确要求 scaffolding 演化

v9 现在明确要求 agent 简化或移除那些已经不再提供价值的 scaffolding。

---

## 16. 与 v8 的关系

v9 是 v8 的超集，而不是一套会使旧工作失效的替代品。

v8 中仍然适用的内容：
- 先检查
- 先推断
- 先复用
- 职责优于固定路径
- 精简问题集
- 适配仓库现实
- 活文档
- 清理临时 bootstrap 文件

新增的内容主要是为了：
- 更清晰的 planning
- 在有必要时更强的质量评估
- 在有必要时的 multi-agent coordination
- 持续简化不再必要的 scaffolding

不需要这些新层的项目可以忽略它们。

---

## 17. 从 v8 迁移

大多数情况下，迁移很简单：

1. 用 v9 替换旧 protocol 文件
2. 让 agent 再次检查仓库
3. 让 agent 决定现在是否有新的可选层已经变得有正当性
4. 只有在仓库证据或工作流需求支持时，才更新仓库的 AI 指令文件

不要因为 v9 存在，就自动对仓库做结构性重组。

---

## 18. 最后的要点

正确使用这个协议最重要的方法是：
- **先检查，再提问**
- **先推断，再把不确定性升级给用户**
- **先复用仓库现有结构，再引入新文件**
- **只启用能解决真实问题的层**
- **让主指令文件保持稳定且仓库专用**
- **把指令和 scaffolding 都视为活的产物**

如果这个协议运转良好，最后的结果应像是一份基于证据、贴合仓库现实的 operating guide，而不是一个通用模板，也不是一堆不必要流程的堆砌。

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
