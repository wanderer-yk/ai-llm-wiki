---
type: concept
title: Blocker Gate（阻断门控）
tags: [门控, 流程控制, sdd, 质量保障]
related: [specflow, 规格驱动ai开发, 单指令状态机]
created: 2026-06-12
updated: 2026-06-12
sources: ["[202603261200]治愈CursorAI编程的幻觉用它就够了.html"]
---
# Blocker Gate（阻断门控）

Blocker Gate 是 [[specflow|Specflow]] 的核心纪律机制，是一种**硬性门控**——当需求对齐未完成时，强制阻断后续开发阶段的进入。

## 核心原则

- "先想清楚，再写清楚"——从根源消灭因理解偏差导致的无效重工
- 防止"带病"进入开发阶段

## 具体实现

在 Specflow 中，Blocker Gate 通过以下机制落地：

- **`[Block]` 问题标记**：Plan 阶段中的强制阻断类问题，未回答则无法进入 Implement
- **`[?]` 问题标记**：可选建议类问题，不阻塞流程流转
- **Specify 阶段完成后**需再次输入 `/specflow` 并通过校验，才能触发 Plan 阶段

## 双级门控体系

`[Block]` 与 `[?]` 的双级问题标记提供了**灵活的门控粒度**——不是所有问题都硬性阻断，区分了"必须回答"与"建议回答"两个层级。

## 待澄清问题

- Blocker Gate 的判定逻辑是纯文件存在性检查还是包含语义校验？
- 流程是否支持回退（如 Plan 审查不通过回到 Specify）？