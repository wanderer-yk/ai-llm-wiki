---
type: entity
title: test-driven-development（Skill）
tags: [skill, tdd, 循环迭代, 防偷懒]
related: [obra-superpowers, skill循环迭代模式, 防止llm偷懒4种武器, 安全边界三原则, 自我说服效应]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604270830]工作流的Skill怎么写从7个顶级Skill中提炼的模式与最佳实践.html"]
---
# test-driven-development（Skill）

`test-driven-development` 是 [[obra-superpowers|obra/superpowers]] 仓库中的一个 371 行工作流型 Skill，是 [[青斧]] 七个分析对象中的第 4 个、循环迭代模式（见 [[skill循环迭代模式]]）的代表。其设计目标是"堵死 LLM 偷懒的所有退路"：以 Iron Law 铁律开篇（不可违反的核心原则），循环体采用 Red-Green-Refactor（RED → Verify RED → GREEN → Verify GREEN → REFACTOR → Repeat），随后以 Common Rationalizations 表预先反驳 12 种 LLM 典型借口，以 8 项 Verification Checklist 充当退出条件，最后以"ask your human partner"实现人类兜底。它同时是多个通用写作技巧的实例来源：强硬语气（"Delete it. Start over."）、借口反驳表、`<Good>`/`<Bad>` 对比教学、人类兜底原则（见 [[防止llm偷懒4种武器]]、[[教学三种有效方式]]、[[安全边界三原则]]）。其借口反驳表与 [[自我说服效应]] 概念强互证。
