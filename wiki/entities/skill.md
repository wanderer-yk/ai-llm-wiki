---
type: entity
title: Skill（技能）
tags: [skill, 工具聚合, 缓存层]
related: [skill-as-knowledge-cache, skill-based-organization, openclaw]
created: 2026-06-08
updated: 2026-06-08
sources: ["从OpenClaw看Agent架构设计.html"]
---

# Skill（技能）

**Skill（技能）** 是 [[openclaw|OpenClaw]] 提出的工具组织单元，指**按功能维度聚合多个工具及其调用方法的说明书**。

## 核心思想

- 解决传统工具查找中"接口维度查找 vs 功能维度需求"的矛盾（参见 [[skill-based-organization]]）。
- 将搜索空间从数十个 API 接口缩减为几个功能模块。
- 本质上是 [[skill-as-knowledge-cache|工具调用知识的缓存层]]，建立在搜索与上下文追加之上。

## 特征

- **不替代搜索或追加上下文**，而是其上的缓存层。
- **可由 Agent 自动总结生成**，形成自我优化循环，无需完全依赖人工编写。
- 能极大降低 Token 消耗，并形成经验沉淀。

## 参见

- [[skill-as-knowledge-cache]]
- [[skill-based-organization]]
- [[conversation-driven-vs-task-driven]]