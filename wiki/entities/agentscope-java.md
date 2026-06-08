---
type: entity
title: AgentScope Java
tags: [agentscope, java, spring-boot, 多智能体]
related: [agentscope, bailian, werewolf-hitl, qwen3-plus]
created: 2026-06-08
updated: 2026-06-08
sources: ["什么我的狼人杀水平还不如AI.html"]
---
# AgentScope Java

[[agentscope|AgentScope]] 的 Java 版本实现，当前使用 release/1.0.5。

## 技术栈

- **语言**：Java 17+
- **构建**：Maven
- **运行时**：Spring Boot（[[werewolf-hitl|werewolf-hitl]] 示例基于 Spring Boot 启动，localhost:8080 访问）
- **LLM 后端**：[[bailian|阿里云百炼]]（DashScope），环境变量 `DASHSCOPE_API_KEY`
- **响应式**：Spring WebFlux + Reactor（SSE 推送与异步用户输入）

## 核心组件

| 组件 | 说明 |
|------|------|
| ReActAgent | 基于 [[react-paradigm|ReAct 范式]]的 AI 玩家 Agent |
| MsgHub | [[msg-hub|发布-订阅消息频道]]，支持隔离与广播控制 |
| InMemoryMemory | 每个 Agent 独立持有的对话记忆 |
| MultiAgentFormatter | [[multi-agent-formatter|多人对话格式化器]] |
| UserAgent | [[agent-interface-polymorphism|人类玩家 Agent]]，与 ReActAgent 同接口 |
| WebUserInput | 基于 Reactor `Sinks.One` 的浏览器输入桥接组件 |

## 与 Python 版的差距

Python 版狼人杀示例已实现 TTS 与全模态交互，Java 版尚在规划中（计划引入角色专属声线）。