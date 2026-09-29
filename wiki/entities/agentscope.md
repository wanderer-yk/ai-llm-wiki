---
type: entity
title: AgentScope
tags: [多智能体框架, Java, 阿里巴巴, 开源]
related: [ai狼人杀, 多智能体消息机制, 结构化输出, human-in-the-loop, 百炼]
created: 2026-06-12
updated: 2026-06-12
sources: ["[202601082000]什么我的狼人杀水平还不如AI.html"]
---
# AgentScope

## 简介

AgentScope 是一个由阿里巴巴团队开发的多智能体应用框架，提供 Java 和 Python 两个版本。该框架旨在解决构建多 Agent 协作系统时的核心技术挑战，包括消息管理、信息隔离、结构化输出和人机交互等。

## 核心组件

### Agent 实现

- **ReActAgent** — 基于 ReAct（Reasoning + Acting）范式的 AI Agent，包含四个核心配置：`name`（标识）、`sysPrompt`（角色提示词）、`model`（底层 LLM）、`memory`（对话记忆）
- **UserAgent** — 人类玩家 Agent，与 ReActAgent 实现同一接口，通过注入 `UserInputBase` 获取用户输入

### 消息管理

- **MsgHub** — 基于发布-订阅模式的消息频道机制，支持自动广播（讨论阶段）、手动广播（投票阶段）和多实例隔离（夜间行动）
- **MultiAgentFormatter** — 多智能体格式化器，通过消息标记（Agent name 绑定到 msg.name）和消息合并（多条消息合并为带 `<history>` 标签的 user 消息）解决 LLM API 三角色限制
- **Memory / InMemoryMemory** — 对话记忆管理

### 输出与编排

- **Structured Output** — 基于 Function Calling 的结构化输出机制
- **多Agent编排器** — 中心协调者，管理游戏流程
- **GameState** — 全局状态对象，追踪角色、存活、轮次等信息

### 实时通信

- **GameEventEmitter** — 双轨事件发射器（playerSink + godViewHistory）
- **SSE 实时推送** — 基于 Spring WebFlux 的 Server-Sent Events
- **WebUserInput** — 通过 Reactor Sinks.One 实现异步等待用户输入

## 支持的模型

为通义千问、OpenAI GPT、Claude、Gemini 等主流模型均提供了对应的 Formatter 实现。

## 技术要求

- Java 17+
- Maven 3.6+
- 底层 LLM 通过阿里云[[百炼]]平台 API 调用

## 项目地址

- Java 版 GitHub: `agentscope-ai/agentscope-java`
- Python 版 GitHub: `agentscope-ai/agentscope`
