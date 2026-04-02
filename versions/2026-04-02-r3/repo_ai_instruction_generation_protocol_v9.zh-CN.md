# 仓库 AI 指令生成协议 v9

[English](./repo_ai_instruction_generation_protocol_v9.md)

> 本中文文件是 `repo_ai_instruction_generation_protocol_v9.md` 的同步 Simplified Chinese companion；如有歧义，以英文标准稿为准。
> 使用本协议生成或刷新仓库专用的 AI 指令文件。
> 本协议对文件名和目录布局均不设限制。
> 常见输出文件名包括 `AGENTS.md`、`CLAUDE.md`、`GEMINI.md`、`COPILOT_INSTRUCTIONS.md` 和 `.github/copilot-instructions.md`。
> 只有在没有更强信号时，才默认使用 `AGENTS.md`。

# <PROJECT_NAME> 仓库 AI 指令生成协议

## 0. 目的
本协议告诉 agent 应如何发现一个仓库、从现有证据中尽可能多地推断信息、只提出少量高影响问题，然后产出该仓库最终的 AI 指令文件以及所需的支持记忆文件。

本协议定义了：
- 职责
- 适配工作流
- 确认规则
- 输出规则
- 质量与验证规则
- 维护与演化规则

本协议**不会**定义固定的仓库布局。
agent 应在可能时把这些职责映射到仓库已经存在的结构中。

---

## 1. 核心原则
1. **证据优先**
   - 先检查仓库，再提问。
   - 优先相信文件、脚本、manifest、CI 和现有文档里可见的事实。

2. **按职责，而不是按固定路径**
   - 协议定义的是记忆 / 文档职责。
   - 仓库专用输出决定这些职责具体落在哪些路径。

3. **先复用，再新增**
   - 如果现有目录和文档约定已经运转良好，优先复用。
   - 不要只是为了匹配模板就迁移或重命名结构。

4. **问题集保持精简**
   - 只询问那些无法从仓库证据中落地、且影响很大的歧义。

5. **能先做的先做**
   - 明显的部分应自动补全。
   - 在提问前先起草最终指令文件，并把问题限制在尚未解决的部分。

6. **稳定规则与易变状态分离**
   - 持久的工作流和仓库规则放进主 AI 指令文件。
   - 任务状态、计划、报告、实验和 handoff 这类易变信息，只在项目确实需要时再放进支持文件。

7. **指令文件是活文档**
   - 生成出的 AI 指令文件不是冻结的。
   - 当仓库现实发生变化，或文件本身变得不准确时，agent 之后可以继续补充、删除或修订内容。

8. **临时 bootstrap 文件是可丢弃的**
   - 一旦最终的仓库专用 AI 指令文件和支持记忆文件已经生成完成，就应删除复制进来的 protocol 文件和 protocol user guide，除非用户明确要求保留。

9. **独立验证优于自我评估**
   - agent 对自己产出的评估，系统性地倾向于正面判断。
   - 当质量重要时，应把生成角色与评估角色分开。
   - 验证应尽量基于可观察行为，例如运行 app、执行测试、检查真实输出，而不是只读代码。

10. **acceptance criteria 会引导结果**
    - 描述“done”和“good”的语言，会直接塑造最后生成出的内容。
    - acceptance criteria 应在执行前定义，而不是事后补写。
    - 要有意识地选择 criteria 的措辞；模糊 criteria 会产出模糊结果，精确 criteria 会产出精确结果。
    - plan 应定义需要交付的 `WHAT` 和 acceptance boundary，不要无谓锁死 `HOW`，除非仓库本身已经要求固定实现路径。
    - 对更大或 multi-agent 的任务，任务契约可以单独放进 `spec` / requirements 文档，让 plan 保持专注于执行。

11. **scaffolding 会随能力演进**
    - 随着模型能力提升，并非每一层结构或每一道流程都还需要保留。
    - 要定期重新评估哪些 scaffolding 组件仍然是承重件。
    - 移除已经不再带来价值的流程；目标是最小但有效的结构。

---

## 2. 输出契约
使用本协议时，应产出：

1. 仓库最终的 AI 指令文件，并使用最合适的文件名
2. 这个仓库明显需要的支持记忆文件
3. 仅针对尚未解决的高影响问题的一份简短假设 / 问题清单
4. 在最终完成后删除复制进来的 protocol 文件和 protocol user guide，除非用户明确要求保留

规则：
- 不要只描述“应该有哪些文件”，而是要真正创建那些显然需要的文件。
- 对于小型仓库，不要强行塞入它们并不受益的可选支持文件。
- 如果仓库已经有可用的等价文档结构，不要再平行造一棵新文档树。
- 如果已经存在多个 AI 指令文件，不要盲目覆盖；先确定同步策略。

---

## 3. 仓库发现工作流
按以下顺序执行。

### 3.1 Discover
在提问前先检查仓库。若存在，应检查：
- `README*`
- 顶层目录
- `package.json`、`pyproject.toml`、`Cargo.toml`、`go.mod`、`pom.xml`、`build.gradle*` 等 manifest
- `Makefile`、`justfile`、`Taskfile.yml`、shell scripts、helper scripts
- `.github/workflows/` 这类 CI 文件
- container、environment 与 dependency 相关文件
- 已有的 docs、notes、reports、runbooks、plans、research files、experiments 或 task trackers
- 已有 AI 指令文件
- remote execution、长时间运行作业、GPU、scheduler 或 artifact 目录的证据
- multi-agent workflow、evaluation pipeline 或 quality gate 的证据

### 3.2 Infer
尽可能从证据中推断信息，在可能时包括：
- 项目名称
- 可能的 canonical working directory
- source、test、script 与 docs 所在位置
- build、test、lint、run 与 validation 命令
- 仓库是 simple、multi-service、monorepo、research、training、infra 还是 mixed
- 是否存在长时间运行任务
- 是否存在 remote execution
- 是否已经存在多个 AI 指令文件
- 是否已经存在一套可运转的记忆 / 文档系统
- 是否存在 multi-agent coordination 或 evaluation workflow
- 项目是否已有 quality gate、acceptance tests 或 validation pipeline

### 3.3 Draft
在提问前，先起草：
- 最终 AI 指令文件的结构
- 从职责到现有仓库路径的映射建议
- 明显需要的支持文件
- 一份简短的、高影响未决项清单

### 3.4 Confirm
只向用户询问那些会实质性影响最终输出的歧义。
典型的高影响例子包括：
- 当多个文件名都合理时，偏好的指令文件名是什么
- 现有多个 AI 指令文件里，是否有一个才是 canonical
- 现有文档目录是否应继续作为权威位置
- 项目是否涉及仓库中看不清楚的 remote servers、GPUs、schedulers 或 shared machines
- 当无法从证据推断时，是否需要启用 task tracking、plans、reports、experiments 或 handoffs
- 生成的记忆文件是否应提交到版本库
- 是否存在仓库中无法看出的隐藏 setup、approval、安全或安全性规则
- 生成文件偏好使用哪种语言
- 是否已经在使用或希望引入 multi-agent coordination 或独立 validation workflow

### 3.5 Finalize
在用户确认之后：
- 更新草稿
- 对于未回答但风险较低的问题，保留有依据的默认值
- 生成最终 AI 指令文件
- 生成被选中的支持文件
- 尽可能删除残留的 assumption marker
- 删除复制进来的 protocol 文件和 protocol user guide，除非用户明确要求保留

---

## 4. 职责层
agent 应按职责层思考，而不是按固定路径思考。
对下面每一层，都应判断这个仓库是否真的需要它，以及它应该放在哪里。
优先映射到现有路径。

### 4.1 稳定规则层
目的：
- 持久的仓库指导
- 结构、命令、工作流、安全、验证和维护规则

典型输出：
- 一个或多个仓库 AI 指令文件

### 4.2 当前状态层
目的：
- dashboard 风格的当前任务可见性
- 活跃 blocker、当前重点和下一步

典型输出：
- 一个紧凑的 tracking 或 status 文件

### 4.3 规划层
目的：
- 在执行前为中型或大型任务制定 plan
- scope、steps、validation、risks、rollback、deliverables
- **completion contract**：在写代码前先定义“done”是什么意思，让生成者与评估者（人或 agent）共享同一预期
- 在有帮助时，把任务契约（`spec` / requirements）与执行 plan 分开，让 `WHAT` 和 `HOW` 保持区分

一个 completion contract 应包含：
- 可观察的 acceptance criteria，而不只是意图描述
- validation method（怎样验证“done”——测试、手动检查、运行 app、截图对比等）
- scope boundary（当前任务明确不包含什么）
- implementation boundary：定义必须交付的 `WHAT`，但不要过度规定 `HOW`，除非仓库或任务已经要求固定实现路径

典型输出：
- 一个任务 plan 文档或现有 planning 区域；对复杂任务，有时会配套一个独立的 `spec` / requirements 文档，并在一开始就写明 acceptance criteria

### 4.4 活跃任务记忆层
目的：
- 长任务或多步骤任务的当前工作记忆
- 最新状态、下一步、链接、blocker

典型输出：
- 任务专用状态文件，或等价的 active-work 区域

### 4.5 归档层
目的：
- 在不让 active tracking 膨胀的前提下保留已完成任务历史

典型输出：
- 已归档任务文件、完成索引或等价结构

### 4.6 决策层
目的：
- 对重大决策与权衡做持久记录

典型输出：
- decision records、ADR 或等价说明

### 4.7 深度分析 / 报告层
目的：
- 详细分析、writeup、evaluation、investigation

典型输出：
- reports、research notes、investigations 或等价文档

### 4.8 实验 / Run 层
目的：
- 在相关场景下记录 experiment、training、evaluation 或 benchmark

典型输出：
- run registry、experiment log 或等价结构

### 4.9 Runbook 层
目的：
- 可重复执行的流程

典型输出：
- runbooks、操作文档、checklists 或等价资料

### 4.10 Handoff 层
目的：
- 在需要时保障跨会话或跨 agent 的连续性

典型输出：
- handoff notes 或等价资料

### 4.11 Remote Execution 层
只有当项目确实使用 remote machines、shared compute、GPUs、schedulers 或 remote artifact locations 时才启用。
目的：
- 区分 local 与 remote 的角色边界
- remote 回写规则
- runtime state 引用
- remote job 观察方式与 artifact 位置

典型输出：
- 集成在主指令文件中的 remote execution 说明；必要时也可写入任务文档或 run 文档

### 4.12 多指令同步层
只有仓库使用不止一个 AI 指令文件时才启用。
目的：
- 确定 canonical source
- 定义 mirror 或 sync 规则
- 记录允许存在的 model-specific 差异

典型输出：
- 生成出的指令文件中的 sync policy 段落

### 4.13 Validation & Quality Assurance 层
默认关闭。只有当仓库证据或用户明确意图能证明需要结构化质量评估时才启用，尤其适用于输出质量具有主观性或多维度的任务。
不要对小仓库、二值正确性任务，或已经有足够自动检查的工作流默认启用这一层。
目的：
- 将生成与评估分开，避免自我评估偏差
- 定义与项目相关的质量维度和评分标准
- 指定验证方法（自动测试、真实交互、视觉检查等）
- 在合适时建立 pass/fail threshold 或 quality gate

质量维度应针对项目本身。例子：
- UI 工作：design quality、originality、craft、functionality
- API：correctness、performance、error handling、documentation
- 数据 pipeline：accuracy、completeness、latency、fault tolerance
- research：reproducibility、rigor、clarity、novelty

关键规则：
- validation criteria 应在工作开始前同时共享给 generator 和 evaluator
- 优先选择真实世界验证（运行 app、执行测试、操作系统）而不是只做 code review
- 在可能时使用 few-shot examples 或 reference implementations 来校准质量预期
- criteria 的措辞会直接影响输出风格，要刻意选择
- 如果评估失败，至少要记录可执行的 delta feedback，包括：失败 criterion、观察到的行为、证据或验证方法，以及下一轮必须修改的内容
- 不要 cargo-cult evaluator loop；只有当它们真的能捕捉失败、或相对轻量验证能额外改进决策时才保留

典型输出：
- 任务 plan 或主指令文件中的 quality criteria 段落
- 针对重复任务类型的 evaluation checklist 或 rubric
- 评估失败时的可执行迭代反馈文件或段落
- 当自动检查可行时的 validation scripts 或 test harnesses

### 4.14 Multi-Agent Coordination 层
默认关闭。只有当仓库证据或用户明确意图表明多个 agent 确实在同一代码库或工作流上协同时才启用。
不要为单 agent 工作流、小仓库，或已经由人类操作者提供足够协调的场景启用这一层。
目的：
- 定义 agent role 与边界（如 planner、implementer、reviewer、evaluator）
- 建立 agent 间的 communication protocol
- 防止冲突修改和重复工作

关键规则：
- 优先使用文件作为通信媒介，而不是消息队列或共享状态；一个 agent 写，另一个读
- coordination 文件应保持简单、可审计、可供人类阅读
- 每个 agent 都应有清晰的职责范围
- 明确定义 handoff 点：什么触发下一个 agent，什么构成有效 handoff
- 不要 cargo-cult multi-agent coordination；只有在确实存在角色边界或 handoff 时才引入
- evaluator 到 implementer 的 handoff 必须携带可执行的 delta feedback，而不是只重复 rubric

典型输出：
- 主指令文件或专用 coordination 文档中的 agent 角色定义
- 多个 agent 共读的 shared contract files（例如 sprint contracts、review checklists）
- agent 用于协调顺序的 status 或 signal files

---

## 5. 适配规则
### 5.1 优先映射，而不是迁移
如果仓库已经有适合 planning、reports、task memory、research 或 runbooks 的位置，就复用它们。

### 5.2 只引入被证明有价值的层
只有当支持文件确实能带来明确价值时才创建。
常见信号包括：
- 多步骤工作
- 长周期工作
- 重复会话
- experiments 或 reports
- 多个贡献者或 agents
- 高概率的上下文丢失风险
- 需要结构化 validation 的质量敏感交付物

### 5.3 避免巨大的“什么都往里塞”的指令文件
当细节变得过大或过于易变时，把它们移入支持文件，并在主指令文件中保留稳定指针。

### 5.4 避免巨大的“什么都往里塞”的 tracking 文件
当前状态应保持简洁。
详细历史应进入任务文件、报告或归档。

### 5.5 尊重仓库现实
如果项目已经使用根目录 `reports/`、`docs/plan`、`docs/research` 等成熟约定，就不要强行改布局。

### 5.6 让 scaffolding 与当前需求匹配
不要只因为某些层之前存在过，就一直保留它们。
如果某一层已经不再带来价值，例如模型变强、项目简化，或团队变化，就应移除或简化。
正确的结构强度，是刚好足以避免质量流失或上下文流失的最小值。

---

## 6. 提问纪律
对于通常可以直接从仓库推断出来的内容，agent **不应**向用户提问，例如：
- 编程语言
- package manager
- 明显的 build / test / lint 命令
- 现有目录
- 是否存在 AI 指令文件
- 是否存在 reports、notes 或 docs

对于会实质影响结构或安全的内容，agent **应当**提问，例如：
- 当存在多个可能文件名时，哪个 AI 指令文件名应是 canonical
- 多个现有 AI 指令文件是否应保持同步
- 是否存在仓库中看不到的 remote execution
- 昂贵或高风险工作流是否需要更强 tracking
- 生成的记忆文件是否应提交
- 隐藏的运维约束、审批要求或团队约定
- 是否希望引入独立 validation 或 multi-agent workflow

问题列表应保持简短。
优先使用简洁 checklist，而不是冗长访谈。

---

## 7. 主 AI 指令文件要求
生成出的仓库 AI 指令文件通常应包含：
- 项目 scope 与 purpose
- working directory rules
- 重要路径与仓库结构
- 在可见时写明 setup、build、test、lint 与 run 命令
- 在适用时写明 validation 预期与质量标准
- editing 与 change rules
- documentation 与 memory mapping
- 如果启用了支持层，写明哪些层已启用以及它们所在位置
- 如果启用了 planning 与 tracking，写明对应规则
- 如果启用了 validation layer，写明 acceptance criteria 指导
- 如果启用了 multi-agent coordination，写明相关规则
- 如果启用了 remote execution，写明相关规则
- 如果启用了多指令同步，写明 sync 规则
- 在需要时写明 Git 与 traceability 规则
- 一条 maintenance rule，说明当有必要时 agent 可以更新该文件
- 当项目预期会变化时，加入 scaffolding evolution 说明

规则：
- 稳定规则放在这里。
- 易变或较大的支持文档应通过链接或指针引用，而不是整块嵌入。
- 如果你从中性的 main-instruction starter 开始，应按仓库现实适配并重命名为仓库真正的 canonical instruction filename。
- 不要把主指令文件变成任务日志或 investigation dump。
- 不要写入缺乏证据支撑的声明。
- 不要冻结已经过时的指令。

---

## 8. 支持文件选择规则
agent 应自行判断应创建哪些支持文件。
把这些层当作能力模块，而不是强制输出项。

如果有 starter templates，只选择那些与仓库真实需要的职责层对应的模板。
一个常见的核心 starter 集包括：
- 一个中性的 main instruction starter
- 一个紧凑的 current-status starter
- 一个带 completion contract 的 plan starter
- 一个 decision starter

更高级的可选 starter 还可以包含 `spec` / requirements 文档，用于长任务、模糊任务、review-heavy 任务或 multi-agent 任务。
应把它视为 planning 的扩展，而不是默认核心集的一部分。

不要机械地把整套模板复制进每一个仓库。

### 通常在以下情况启用 current status tracking：
- 工作会跨越多个会话
- 同时有多个 active task
- 仓库中存在超出琐碎修改的持续项目工作

### 通常在以下情况启用 planning：
- 任务是中型或大型
- 任务包含多个主要步骤
- validation 并不简单
- rollback 或风险很重要
- 工作可能跨越多个会话

### 通常在以下情况把任务契约拆进单独的 `spec` / requirements 文档：
- 任务很长或很模糊
- review 质量依赖于执行前就明确的任务契约
- 多个贡献者或多个 agent 会进行 handoff
- 把 `WHAT` 与 `HOW` 分开能降低迭代漂移

### 通常在以下情况启用 active task memory：
- 一个任务需要比 dashboard 一行更持久的状态
- 持续中的工作很容易在跨会话时丢失

### 通常在以下情况启用 archive：
- 否则 active task memory 会不断积累已完成历史并变得嘈杂

### 通常在以下情况启用 decisions：
- 预计会出现重大 tradeoff 或非显然设计选择

### 通常在以下情况启用 reports：
- 预计需要深度分析、调查、评估或 writeup

### 通常在以下情况启用 experiment / run tracking：
- 仓库包含 training、benchmarking、evaluation、ablation 或重复性的操作运行

### 通常在以下情况启用 runbooks：
- 某些流程会重复执行，并且容易出错

### 通常在以下情况启用 handoffs：
- 多个会话、多人或多个 agent 将异步继续工作

### 通常在以下情况启用 remote execution notes：
- 执行环境或 artifact 位于 remote machines 或 shared compute 上

### 通常在以下情况启用多指令同步：
- 不止一个 AI 指令文件是该仓库工作流的一部分

### 通常在以下情况启用 Validation & Quality Assurance：
- 仓库证据或用户明确意图表明需要结构化评估
- 输出质量具有主观性或多维度，例如 UI、UX、design
- 项目有“看起来完成了但其实不能用”的历史问题
- 因 acceptance criteria 不清晰而反复迭代很常见
- 自动或半自动评估既可行也有价值
- 失败交付的成本，相比结构化评估的成本更高

### 通常在以下情况启用 Multi-Agent Coordination：
- 仓库证据或用户明确意图表明多个 agent 确实是工作流的一部分
- 多个 agent 会并发或串行地操作同一代码库
- 存在清晰的分工，例如 planning、implementation、review、evaluation
- agent-to-agent handoff 是工作流的一部分
- 项目正在使用或计划使用 orchestration harness

---

## 9. Git 与可追溯性
当项目规模已经大到足以受益时，再启用 traceability 规则。

推荐做法：
- 对中型或大型任务使用 task ID
- 在任务 plan、task memory、reports 以及相关 branch 或 commit 中，尽可能带上 task ID
- 让代码变更与状态文档变更保持一致
- 在相关场景中，把 commit hash 或 artifact reference 记进 report 或 experiment record
- 把 Git history 和项目记忆视为互补，而不是互相替代

不要对真正很小的仓库强加沉重的 traceability 负担。

---

## 10. 维护规则
生成出的 AI 指令文件是活文档。
在以下情况，agent 可以且应该更新它：
- 仓库结构变化
- 命令或 setup 规则变化
- workflow 或 validation 规则变化
- 文档本身变得不准确、误导，或体积过大
- 支持记忆映射发生变化
- 多文件 sync policy 发生变化
- 质量标准或 validation method 演化

更新指令文件时：
- 保留仓库专用约定
- 保留用户已确认的决策，除非确实需要再次确认
- 按仓库自己的 sync policy 更新已同步的 AI 指令文件

---

## 11. Scaffolding 演化
随着模型和工具改进，协议本身以及生成出的指令文件都应演化。
为 validation 或 multi-agent coordination 加入的层，应被视为情境化 scaffolding，而不是永久性仪式。

### 11.1 周期性重新评估
在回访一个已生成的指令文件时，考虑：
- 哪些 scaffolding layer 仍在防止真实问题
- 哪些层已经变成不必要的额外负担
- 新的模型能力是否让某些 workaround 过时
- 项目规模或团队是否已经变化

### 11.2 可简化信号
在以下情况考虑移除或简化某一层：
- 它解决的问题已经不再出现（例如更强模型缓解了 context anxiety）
- 维护这一层的开销已经超过它的价值
- 团队已经成长到不再受它原本防止的失败模式影响
- 已经有更简单的替代方案
- 某个 validation 或 coordination layer 虽然反复存在，但并不会改变决策、捕捉失败或改进 handoff

### 11.3 演化记录
在移除或显著修改 scaffolding 时，应在指令文件或 decision record 中简要说明：
- 移除了或改变了什么
- 为什么它已经不再需要
- 有什么替代了它（如果有）

这样可以防止未来的 agent 或团队成员在不了解原因的情况下把已移除的 scaffolding 再引入回来。

---

## 12. 最终化与清理
在生成最终的仓库专用输出之后：
- 确保主 AI 指令文件存在
- 确保被选中的支持文件存在
- 确保未解决的假设不是已确认，就是被明确标注
- 确保指令文件解释了易变项目记忆放在哪里
- 删除复制进来的 protocol 文件和 protocol user guide，除非用户明确要求保留

默认不要留下临时 bootstrap 文件。

---

## 13. 紧凑生成检查清单
1. 检查仓库
2. 推断一切可推断信息
3. 识别现有文档 / 记忆结构
4. 选择指令文件名与 sync 策略
5. 判断需要哪些职责层（包括 validation 和 multi-agent coordination）
6. 在可能时把职责层映射到现有路径
7. 起草最终 AI 指令文件，或适配一个中性的 main-instruction starter
8. 只选择这个仓库真正需要的 starter templates
9. 只提一份简短的高影响问题清单
10. 完成最终文件并创建所需支持文件
11. 除非另有要求，否则删除复制进来的 bootstrap protocol 文件

---

## 14. 可选 Starter 片段与模板文件
只在仓库真的需要时使用。
名称与位置应按仓库现实调整。
如果模板仓库提供了对应的 standalone starter files，应把这里的片段与那些文件都视为可复用起点，而不是强制输出。
默认核心集仍是 main instruction、tracking、plan 和 decision starters；`spec` / requirements 是高级的可选 planning starter。
不要默认整套复制。
为保持与英文标准稿及 `.ai/templates/` 中的 starter 文件对齐，下面的 starter snippets 保留英文原文。

### Main Instruction Starter
```md
# Repository Working Instructions

## Purpose And Scope
- This repository is <WHAT_IT_IS>.
- AI work here is primarily about <WHAT_AGENTS_SHOULD_OPTIMIZE_FOR>.
- Do not <OUT_OF_SCOPE_OR_FORBIDDEN_CHANGE_TYPES>.

## Working Directory Rules
- Treat <PRIMARY_WORKING_DIRECTORIES> as the main work area.
- Do not assume generated, vendored, or external paths are safe to edit unless the repository says so.

## Repository Map
- <MAIN_ENTRY_OR_README>: <ROLE>
- <SOURCE_OR_DOCS_PATH>: <ROLE>
- <TEST_OR_VALIDATION_PATH>: <ROLE>

## Commands
- Setup: <COMMAND_OR_NA>
- Build: <COMMAND_OR_NA>
- Test: <COMMAND_OR_NA>
- Lint / Format / Validate: <COMMAND_OR_NA>
- Run: <COMMAND_OR_NA>

## Editing And Change Rules
- Keep changes aligned with existing repository conventions.
- Preserve user-confirmed decisions unless new evidence or explicit confirmation changes them.

## Documentation And Memory Map
- Keep stable, global, repository-wide rules in this file.
- Move volatile, detailed, or task-specific material into support files only when the repository actually benefits from them.
- Task-specific contracts or requirements may sit beside plans when separating `WHAT` from `HOW` adds clarity.
- Example status path: <STATUS_PATH_OR_REMOVE>
- Example planning path: <PLANS_PATH_OR_REMOVE>
- Optional task contract path: <SPEC_OR_REQUIREMENTS_PATH_OR_REMOVE>
- Example decisions path: <DECISIONS_PATH_OR_REMOVE>

## Enabled Support Layers
- Current Status Layer: <STATUS_PATH_OR_REMOVE>
- Planning Layer: <PLANS_PATH_OR_REMOVE>
- Decision Layer: <DECISIONS_PATH_OR_REMOVE>

## Canonical Filename And Sync Policy
> Keep this section only if more than one instruction file is part of the real workflow.

- Canonical instruction file: <PATH>
- Mirror or companion files: <PATHS_OR_NONE>
- Sync rule: <HOW_CHANGES_STAY_ALIGNED>

## Maintenance
- This file is a living repository contract.
- Update it when repository structure, workflows, enabled layers, or stable rules change.
```

### 14.1 Compact Tracking Starter
```md
# Tracking

## Active
- <TASK_OR_AREA> - <CURRENT_STATE> - <POINTER>

## Blocked
- <TASK_OR_AREA> - <BLOCKER> - <POINTER>

## Next
- <NEXT_ACTION>

## Last Updated
- <DATE_TIME>
```

### 14.2 Spec / Requirements Starter (Optional)
```md
# <TASK_ID_OR_TITLE> Spec

## Objective
- <OBJECTIVE>

## Context / Problem
- <BACKGROUND_OR_PROBLEM>

## Scope
- In:
- Out:

## Requirements / Expected Outcomes
- <REQUIREMENT_OR_EXPECTED_OUTCOME_1>
- <REQUIREMENT_OR_EXPECTED_OUTCOME_2>

## Constraints / Dependencies
- <CONSTRAINT_OR_DEPENDENCY>

## Acceptance Criteria
- [ ] <OBSERVABLE_OUTCOME_1>
- [ ] <OBSERVABLE_OUTCOME_2>
- [ ] <OBSERVABLE_OUTCOME_3>

Acceptance criteria should define what reviewers can confirm without inferring unstated intent.

## Open Questions / Deferred Items
- <QUESTION_OR_DEFERRED_ITEM>
```

### 14.3 Plan Starter (with Completion Contract)
```md
# <TASK_ID_OR_TITLE>

## Goal
- <GOAL>

## Scope
- In:
- Out:

## Related Spec / Requirements (Optional)
- <PATH_OR_NA>

Use this when you need a separate task contract. Let the spec define `WHAT`, and keep this plan focused on sequencing, validation, and rollback.

## Completion Contract
> Define "done" before starting work. For medium tasks, this section can stand alone. If a separate spec exists, keep this section as a concise acceptance boundary and point to the spec instead of repeating every requirement. The contract should define `WHAT` must be delivered, keep `HOW` flexible unless the repository already requires a fixed implementation path, and make reviewable outcomes explicit.

### Acceptance Criteria
- [ ] <OBSERVABLE_OUTCOME_1>
- [ ] <OBSERVABLE_OUTCOME_2>
- [ ] <OBSERVABLE_OUTCOME_3>

Acceptance criteria should be observable and reviewable, not vague intent statements.

### Validation Method
- <HOW_WILL_DONE_BE_VERIFIED — e.g., run tests, manual walkthrough, browser check, screenshot comparison>

Validation should state how "done" will be checked in practice.

## Execution Steps
1. <STEP_OR_PHASE>
2. <STEP_OR_PHASE>

## Risks / Rollback
- <RISK>
- <ROLLBACK>
```

### 14.4 Active Task Starter
```md
# <TASK_ID_OR_TITLE>

## Summary
- <ONE_LINE_GOAL>

## Status
- Not started / In progress / Blocked / Done

## Latest State
- <LATEST_STATE>

## Next Step
- <NEXT_STEP>

## Links
- Plan:
- Report:
- Artifact:

## Last Updated
- <DATE_TIME>
```

### 14.5 Decision Starter
```md
# <DECISION_TITLE>

> Use for durable, non-obvious choices that future collaborators would otherwise need to rediscover.

## Context
- <CONTEXT>

## Options
- <OPTION_A>
- <OPTION_B>

## Decision
- <CHOSEN_OPTION>

## Consequences
- <IMPACT>
```

### 14.6 Experiment / Run Starter
```md
# <RUN_ID_OR_TITLE>

## Objective
- <OBJECTIVE>

## Inputs
- Commit:
- Config:
- Dataset / Environment:

## Execution
- Command:
- Host / Environment:
- Log / Artifact Path:

## Result
- <KEY_RESULT>

## Conclusion
- <ONE_LINE_CONCLUSION>
```

### 14.7 Quality Evaluation Starter
```md
# <TASK_ID> Quality Evaluation

## Evaluation Date
- <DATE>

## Evaluator
- <HUMAN / AGENT_NAME / AUTOMATED>

## Criteria & Scores

| Dimension | Description | Score (1-5) | Notes |
|---|---|---|---|
| <DIMENSION_1> | <WHAT_IS_BEING_MEASURED> | <SCORE> | <OBSERVATION> |
| <DIMENSION_2> | <WHAT_IS_BEING_MEASURED> | <SCORE> | <OBSERVATION> |
| <DIMENSION_3> | <WHAT_IS_BEING_MEASURED> | <SCORE> | <OBSERVATION> |

## Validation Method
- <HOW_WAS_IT_TESTED — e.g., ran app in browser, executed test suite, manual walkthrough>

## Pass / Fail
- Threshold: <MINIMUM_ACCEPTABLE_SCORE>
- Result: <PASS / FAIL>

## Next Iteration Delta
> Required when result is `FAIL`. Repeat once per failed criterion.

- Failed criterion:
- Observed behavior:
- Evidence / validation method:
- Required change for next iteration:

## Feedback for Next Iteration
- <SPECIFIC_ACTIONABLE_FEEDBACK>
```

### 14.8 Multi-Agent Coordination Starter
```md
# Agent Coordination

## Agents & Roles

| Agent | Role | Scope | Trigger |
|---|---|---|---|
| <AGENT_1> | <ROLE — e.g., Planner> | <WHAT_IT_OWNS> | <WHEN_IT_ACTIVATES> |
| <AGENT_2> | <ROLE — e.g., Implementer> | <WHAT_IT_OWNS> | <WHEN_IT_ACTIVATES> |
| <AGENT_3> | <ROLE — e.g., Evaluator> | <WHAT_IT_OWNS> | <WHEN_IT_ACTIVATES> |

## Communication Protocol
- Medium: file-based (one agent writes, another reads)
- Contract files: <PATH_TO_SHARED_CONTRACTS>
- Signal files: <PATH_TO_STATUS_SIGNALS>
- Feedback files: when evaluation fails, record delta items with failed criterion, observed behavior, evidence / validation method, and required change

## Handoff Rules
- <AGENT_1> → <AGENT_2>: <WHAT_CONSTITUTES_A_VALID_HANDOFF>
- <AGENT_2> → <AGENT_3>: <WHAT_CONSTITUTES_A_VALID_HANDOFF>
- <AGENT_3> → <AGENT_2> (iteration): <WHEN_AND_HOW_FEEDBACK_IS_SENT_BACK; feedback must be specific enough to drive the next pass without re-interpreting the rubric>

## Iteration Limits
- Max iterations per sprint: <N>
- Escalation: <WHAT_HAPPENS_IF_LIMIT_IS_REACHED>
```
