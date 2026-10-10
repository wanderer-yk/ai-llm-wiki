---
type: concept
title: skill 多阶段 + 检查点 + Skill 编排模式（模式 5）
tags: [skill, 设计模式, 多阶段, 检查点, 编排器, go-no-go]
related: [discovery-process, deanpeters-product-manager-skills, skill嵌套编排, 验证门禁化, workshop-facilitation, 模式选择决策树, skill接力棒循环模式]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604270830]工作流的Skill怎么写从7个顶级Skill中提炼的模式与最佳实践.html"]
---
# skill 多阶段 + 检查点 + Skill 编排模式（模式 5）

多阶段 + 检查点 + Skill 编排模式是 [[青斧]] 归纳的 5 种核心设计模式之五，也是 7 个分析对象中行数最长的模式（502 行），适用于跨越多天/多周、有明确阶段划分和 Go/No-Go 决策点的复杂流程。代表案例为 [[deanpeters-product-manager-skills|deanpeters/Product-Manager-Skills]] 的 [[discovery-process]]（502 行），一句话精髓"编排器模式，调度 10+ 子 Skill"。

**结构**：Key Concepts（含反模式）→ Phase 1-6 → Complete Workflow → Common Pitfalls → References（子 Skill 列表）。

**关键技巧**（五条）：统一阶段模板——每个 Phase 均为 Activities → Outputs → Decision Point 三段式，降低 LLM 理解成本；决策检查点——显式 YES/NO 判断防止盲目推进（"达到饱和了吗？YES → 下一阶段，NO → +1 周"），与 [[验证门禁化]] 互证；Skill 编排——References 显式列出被调用的 10+ 子 Skill，大 Skill 调度小 Skill（编排器模式，与 [[skill嵌套编排]] 直接互证）；时间影响——每个 NO 路径标注延迟成本（"+2-3 days"、"+1 week"），让用户了解代价；交互协议分离——主 Skill 引用 `workshop-facilitation` 定义交互方式，实现关注点分离。

**选择判据**：任务跨越多天/多周且有 Go/No-Go 决策点。与 [[skill接力棒循环模式]] 的分野：前者管理"阶段推进"（人类主导决策），后者管理"循环续命"（文件承载状态）。
