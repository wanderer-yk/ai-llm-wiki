---
type: entity
title: discovery-process（Skill）
tags: [skill, 产品管理, 多阶段, 检查点, 编排器]
related: [deanpeters-product-manager-skills, skill多阶段检查点编排模式, 验证门禁化, skill嵌套编排]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604270830]工作流的Skill怎么写从7个顶级Skill中提炼的模式与最佳实践.html"]
---
# discovery-process（Skill）

`discovery-process` 是 [[deanpeters-product-manager-skills|deanpeters/Product-Manager-Skills]] 仓库中的一个 502 行 Skill（7 个分析对象中最长），是 [[青斧]] 归纳的多阶段+检查点模式（见 [[skill多阶段检查点编排模式]]）的代表，一句话精髓为"编排器模式，调度 10+ 子 Skill"。

结构为：Key Concepts（含反模式）→ Phase 1-6 → Complete Workflow → Common Pitfalls → References（子 Skill 列表）。关键设计：每个 Phase 均采用 Activities → Outputs → Decision Point 的统一三段式模板，降低 LLM 理解成本；关键节点设 Go/No-Go 决策检查点（如"达到饱和了吗？YES → 下一阶段，NO → +1 周"），NO 路径显式标注时间影响（"+2-3 days"、"+1 week"）；通过 References 列表显式调度 10+ 个子 Skill（含 `workshop-facilitation`，用于交互协议分离，实现关注点分离）。其 Go/No-Go 检查点与 [[验证门禁化]] 互证，编排器模式与 [[skill嵌套编排]] 直接互证。
