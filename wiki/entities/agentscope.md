---
type: entity
title: AgentScope
tags: [多智能体框架, 阿里达摩院, java, python, agent]
related: [agentscope-java, bailian, qwen3-plus, msg-hub, multi-agent-formatter, react-paradigm]
created: 2026-06-08
updated: 2026-06-08
sources: ["什么我的狼人杀水平还不如AI.html"]
---
# AgentScope

阿里达摩院推出的多智能体框架，支持 Java 和 Python 两个版本。文章中通过 Java 版本（[[agentscope-java|AgentScope Java]]，release/1.0.5）构建 AI 狼人杀游戏展示了其六大核心能力。

## 六大核心能力

1. **ReActAgent**：基于 [[react-paradigm|ReAct 范式]]（Reasoning + Acting）的持续推理循环，每个 Agent 独立持有 [[in-memory-memory|InMemoryMemory]] 对话记忆
2. **MsgHub**：基于发布-订阅模式的 [[msg-hub|消息频道机制]]，支持频道隔离与广播控制
3. **多智能体格式化器**：[[multi-agent-formatter|消息标记 + 消息合并]]，解决 LLM API 三角色限制下的多说话者问题
4. **结构化输出**：[[function-calling-structured-output|基于 Function Calling]] 的结构化输出，确保决策可靠解析
5. **UserAgent**：[[agent-interface-polymorphism|人类玩家 Agent]]，与 ReActAgent 同接口，替换即可接入
6. **流式输出 + SSE**：[[dual-perspective-architecture|双视角实时推送]]，玩家视角（角色过滤）+ 上帝视角（全量复盘）

## 多模型支持

为通义千问（[[dashscope-multi-agent-formatter|DashScopeMultiAgentFormatter]]）、GPT、Claude、Gemini 等均提供对应的格式化器实现。

## 相关资源

- GitHub（Java 版）：`agentscope-ai/agentscope-java`
- Python 版狼人杀示例：`agentscope-ai/agentscope/tree/main/examples/game/werewolves`
- 钉钉交流群：146730017349