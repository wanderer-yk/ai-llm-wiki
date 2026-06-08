---
type: concept
title: ReAct 范式
tags: [reAct, 推理, agent, 思考循环]
related: [agentscope, ai-werewolf-game, role-specific-prompt-strategy]
created: 2026-06-08
updated: 2026-06-08
sources: ["什么我的狼人杀水平还不如AI.html"]
---
# ReAct 范式

ReAct（Reasoning + Acting）是一种将 LLM 从单轮问答升级为"思考-行动-再思考"持续推理循环的范式。在 AgentScope 中通过 ReActAgent 实现。

## 核心机制

```
思考（Reasoning）→ 行动（Acting）→ 再思考 → ...
```

区别于传统的单次调用-回复模式，ReActAgent 在游戏过程中持续维护推理链，每个决策都基于历史上下文。

## 创建要素

ReActAgent 创建需配置四个核心要素：
- **name**：Agent 名称/角色名
- **sysPrompt**：系统 Prompt（含 [[role-specific-prompt-strategy|角色专属策略]]）
- **model**：使用的 LLM 模型
- **memory**：独立的对话记忆（InMemoryMemory）

## 关联概念

- [[role-specific-prompt-strategy|角色专属 Prompt 策略]]定义了不同角色的推理行为
- 每个 Agent 独立持有 Memory，实现 [[msg-hub|信息隔离]] 的基础