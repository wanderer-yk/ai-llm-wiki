---
type: entity
title: OpenClaw
tags: [agent, 架构设计, 分析对象]
related: [claude-code, agent-architecture-design, compression-strategy, task-isolation, skill-as-knowledge-cache, conversation-driven-vs-task-driven]
created: 2026-06-08
updated: 2026-06-08
sources: ["从OpenClaw看Agent架构设计.html"]
---

# OpenClaw

**OpenClaw** 是《从 OpenClaw 看 Agent 架构设计》一文的核心分析对象，作为大模型 Agent 的代表性实现被系统剖析。

## 架构特征

### 上下文管理

- 采用 [[append-only-context|追加式上下文]] 模式，与 [[claude-code]] 一致。
- 压缩策略上，OpenClaw 采用**记忆落盘 + 渐进压缩**，区别于 Claude Code 的摘要前置。
- 支持 [[task-isolation|任务隔离]]，将不同任务分配独立上下文窗口。

### 工具加载

- 本地使用统一 `execute` 控制台工具（参见 [[console-vs-mcp-strategy]]），借助大模型世界知识调用终端命令。
- 远程服务使用 [[mcp]] 协议。
- 支持 [[progressive-tool-loading|渐进式加载]]，缓解与 [[prompt-cache-mechanism|Prompt 缓存]] 的冲突。

### 工具查找

- 引入 [[skill|Skill]] 概念，按功能维度聚合工具，作为 [[skill-as-knowledge-cache|工具调用知识的缓存层]]。
- Skill 可由 Agent 自动总结生成，形成自我优化循环。

### 主循环

- 倾向 [[conversation-driven-vs-task-driven|任务驱动]] 模式，强调 [[perceive-think-act-loop|感知-思考-行动]] 循环。
- 将[[thought-process-as-first-class-citizen|思考过程]]作为核心输出，增强可观测性。

## 参见

- [[agent-architecture-design]]
- [[claude-code]]
- [[architectural-decision-interdependence]]