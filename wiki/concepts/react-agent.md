---
type: concept
title: ReActAgent
tags: [ReAct, Agent, LLM推理, 持续思考]
related: [agentscope, ai狼人杀, 多智能体消息机制]
created: 2026-06-12
updated: 2026-06-12
sources: ["[202601082000]什么我的狼人杀水平还不如AI.html"]
---
# ReActAgent

## 简介

ReActAgent 是 [[agentscope|AgentScope]] 框架中基于 ReAct（Reasoning + Acting）范式的 Agent 实现，用于让每个 AI 玩家具备持续思考和对话的能力。它是[[ai狼人杀|AI 狼人杀]]游戏中所有 AI 角色的基础实现。

## 核心设计

### 四个关键配置

| 配置 | 说明 |
|------|------|
| `name` | 玩家标识，如"1号玩家"、"3号玩家" |
| `sysPrompt` | 角色提示词，定义角色的行为规则和策略 |
| `model` | 底层 LLM 模型（如 qwen3-plus） |
| `memory` | 对话记忆（InMemoryMemory） |

### ReAct 范式

ReAct 让 LLM 遵循"思考-行动-再思考"的循环：

1. **Reasoning** — 基于历史对话和当前局势进行推理
2. **Acting** — 根据推理结果做出发言或决策
3. **循环** — 将新信息写入 Memory，作为下一轮推理的输入

## 角色提示词设计

不同角色的 sysPrompt 差异显著：

- **狼人**: 需学会撒谎配合，内置悍跳狼（冒充预言家）和深水狼（伪装村民）两种策略
- **神职角色**: 需懂得使用技能（预言家查验、女巫救人/毒人、猎人开枪）
- **村民**: 需分析逻辑，识别可疑发言

## 与 UserAgent 的关系

ReActAgent 与 [[human-in-the-loop|UserAgent]] 实现同一接口。在[[ai狼人杀|AI 狼人杀]]中，只需将某个位置的 ReActAgent 替换为 UserAgent 即可让人类玩家加入，编排器代码无需任何修改。
