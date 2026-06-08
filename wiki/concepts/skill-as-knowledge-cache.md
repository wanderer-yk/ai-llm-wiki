---
type: concept
title: Skill 作为知识缓存
tags: [skill, 缓存, 经验沉淀]
related: [skill, skill-based-organization, openclaw, agent-architecture-design]
created: 2026-06-08
updated: 2026-06-08
sources: ["从OpenClaw看Agent架构设计.html"]
---

# Skill 作为知识缓存

**Skill 作为知识缓存** 揭示了 [[skill|Skill]] 的本质：它是**工具调用知识的缓存层**，建立在搜索与上下文追加之上。

## 核心论点

- Skill **不替代**搜索或 [[append-only-context|追加上下文]]，而是其上的缓存层。
- 将频繁使用的工具调用知识预先组织，避免重复探索。
- 形成**经验沉淀**，降低 Token 消耗。

## 自我优化循环

- Skill 可由 Agent 自动总结生成，无需完全依赖人工编写。
- Agent 在执行任务过程中积累经验 → 自动生成 Skill → 提升未来效率。
- 这构成了 Agent 的**自我优化闭环**。

## 证据

文章提供了有/无 Skill 的 Token 消耗对比，证明 Skill 能显著压缩执行步骤与成本。

## 参见

- [[skill]]
- [[skill-based-organization]]
- [[conversation-driven-vs-task-driven]]