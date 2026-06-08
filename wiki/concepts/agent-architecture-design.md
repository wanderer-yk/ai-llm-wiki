---
type: concept
title: Agent 架构设计（四大决策维度）
tags: [agent, 架构, 框架]
related: [openclaw, append-only-context, compression-strategy, task-isolation, prompt-cache-mechanism, skill-as-knowledge-cache, conversation-driven-vs-task-driven, architectural-decision-interdependence]
created: 2026-06-08
updated: 2026-06-08
sources: ["从OpenClaw看Agent架构设计.html"]
---

# Agent 架构设计

**Agent 架构设计** 是构建大模型 Agent 时必须做出的系统性工程决策框架。根据《从 OpenClaw 看 Agent 架构设计》，构建 Agent 没有标准答案，但有**四个关键决策维度**，每个维度都有明确的代价与妥协。

## 四大决策维度

### 1. 上下文管理
- [[append-only-context|追加式上下文]]：全量历史发送，只增不减
- [[compression-strategy|压缩策略]]：逼近窗口上限时压缩
- [[task-isolation|任务隔离]]：独立上下文窗口

### 2. 工具加载
- 与 [[prompt-cache-mechanism|Prompt 缓存]] 的冲突
- [[console-vs-mcp-strategy|控制台 + MCP 混合]]
- [[progressive-tool-loading|渐进式加载]]
- [[prompt-based-tool-injection|Prompt 级注入]]

### 3. 工具查找
- [[skill-based-organization|技能维度组织]]
- [[skill-as-knowledge-cache|Skill 作为知识缓存]]

### 4. 主循环设计
- [[conversation-driven-vs-task-driven|对话驱动 vs 任务驱动]]
- [[perceive-think-act-loop|感知-思考-行动循环]]
- [[thought-process-as-first-class-citizen|思考过程作为一等公民]]

## 核心理念

参见 [[architectural-decision-interdependence|架构决策相互关联性]]：四大决策并非孤立，而是深度耦合、相互制约。理解其关联与取舍是设计高效 Agent 的前提。

## 参见

- [[openclaw]]
- [[claude-code]]
- [[architectural-decision-interdependence]]