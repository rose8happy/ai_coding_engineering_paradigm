# Anthropic Harness Design for Long-Running Apps — 核心提炼

> 原文：[Harness design for long-running application development](https://www.anthropic.com/engineering/harness-design-long-running-apps)  
> 作者：Prithvi Rajasekaran（Anthropic Labs）  
> 发布：2026-03-24

---

## 一、核心主旨

**同一个模型，在不同 harness（脚手架/编排结构）下产出质量天壤之别。**  
harness 设计本身就是 AI 工程的核心竞争力，而不只是模型能力本身。

> 原话："The space of interesting harness combinations doesn't shrink as models improve. Instead, it moves."

---

## 二、文章解决的两个根本问题

| 问题 | 症状 |
|---|---|
| **自我评估失效** | 模型对自己生成的内容总是给予正面评价，即使质量明显低劣 |
| **长任务上下文退化** | context 填满时模型失去连贯性，甚至提前"放弃"（context anxiety） |

---

## 三、核心解法：GAN 启发的 Generator-Evaluator 分离

从 GAN（生成对抗网络）借鉴结构：
- **Generator**：负责生成/实现
- **Evaluator**：负责批判/评分，且与 Generator 相互独立

关键洞察：
- 让一个专门调教为"挑剔"的独立 Evaluator 来评分，比让 Generator 自评要有效得多
- Evaluator 通过 Playwright MCP **真实操作浏览器**，而非只看代码或截图

---

## 四、完整三 Agent 架构（全栈应用场景）

```
用户 prompt（1-4句话）
       ↓
  [Planner Agent]
  - 扩展为完整 product spec
  - 强调 WHAT（交付物），不限定 HOW（实现细节）
  - 主动发掘 AI 功能融入点
       ↓
  [Generator Agent]  ←─────────────────────┐
  - 按 sprint 逐个 feature 实现             │
  - 每个 sprint 前与 Evaluator 签订"验收合约" │ 迭代循环
  - sprint 结束后提交给 QA                  │
       ↓                                    │
  [Evaluator Agent] ───────────────────────┘
  - 用 Playwright 真实点击运行中的 app
  - 按四维标准打分，有硬性阈值
  - 低于阈值 → 失败，反馈给 Generator
  - 通过 → 进入下一个 sprint
```

### Sprint Contract（验收合约）机制
每个 sprint 开始前，Generator 和 Evaluator **先协商"完成"的定义**，再开始写代码。  
防止 spec 高层抽象与实现细节之间产生歧义。

### Agent 间通信
- **通过文件传递**，不通过消息队列  
- 一个 agent 写文件，另一个读取并响应

---

## 五、评分四维标准（前端 & 全栈均适用）

| 维度 | 说明 | 权重 |
|---|---|---|
| **Design Quality** | 整体感、视觉凝聚力、风格一致性 | 高 |
| **Originality** | 有无定制化创意决策，拒绝"AI slop"（紫渐变白卡片等） | 高 |
| **Craft** | 排版层次、间距一致性、颜色对比度等基础技术执行 | 中 |
| **Functionality** | 用户能否完成核心任务，无需猜测 | 中 |

- 评分标准本身要作为 prompt 同时给 Generator 和 Evaluator，提前对齐期望
- 用 few-shot 示例校准 Evaluator，防止评分漂移
- 标准措辞会直接影响生成风格（如写"museum quality"会收敛到某种视觉风格）

---

## 六、上下文管理策略

| 策略 | 机制 | 适用场景 |
|---|---|---|
| **Compaction** | 压缩早期对话，保持同一 agent 继续 | 模型无 context anxiety 时（如 Opus 4.5+） |
| **Context Reset** | 清空 context，结构化交接给新 agent | 旧模型（Sonnet 4.5）有 context anxiety 时 |

> 随着模型升级（Sonnet 4.5 → Opus 4.5），Context Reset 被移除，改用 SDK 自动 Compaction。  
> **哪些 harness 组件是必需的，要随模型迭代重新验证。**

---

## 七、成本 vs 质量权衡（实测数据）

| 模式 | 耗时 | 费用 | 结果 |
|---|---|---|---|
| 单 agent | 20 分钟 | ~$9 | 有不可用功能，UX 差 |
| 完整三 agent harness | 6 小时 | ~$200 | 可运行，Evaluator 捕获了路由顺序错误、entity wiring 断裂等真实 bug |

> "$200 做一个 demo 很贵，但做一个产品很便宜。"

---

## 八、核心工程原则（可迁移部分）

1. **外部评估 > 自我评估**：分离 Generator 和 Evaluator 是最强的质量杠杆
2. **约束交付物，不约束路径**：Planner 定 WHAT，Generator 定 HOW
3. **Sprint 合约先行**：写代码前先对齐"完成"的定义
4. **文件作为 agent 通信媒介**：简单、可审计、无状态依赖
5. **用真实交互测试**：Evaluator 操作真实浏览器，不只读代码
6. **harness 复杂度应随模型升级而降低**：持续测试哪些脚手架已不再需要
7. **评分标准的语言本身塑造输出风格**：措辞即 steering

---

## 九、与早期 harness 的演进对比

| 版本 | 特征 | 弃用原因 |
|---|---|---|
| 早期（Sonnet 4.5）| initializer + coding agent，context resets | 模型升级后 context anxiety 消失 |
| 当前（Opus 4.5+）| planner + generator + evaluator，无 reset | Compaction 足够；sprint decomposition 也逐步移除 |

---

## 十、适用范围

文章主要面向 **Web 应用全栈自动构建**，但作者认为同样的原则可推广至：
- 科学研究 agent
- 金融建模 agent
- 任何跨多 context window 的长任务场景

---

*以上为内容提炼，可结合你的 agents.md 等项目经验，让 ChatGPT 定制适合你项目的 harness 结构。*
