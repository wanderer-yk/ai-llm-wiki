---
type: concept
title: Agent 接口多态
tags: [多态, user-agent, react-agent, 人机混合, agentscope]
related: [agentscope, ai-werewolf-game, sinks-one-async-pattern]
created: 2026-06-08
updated: 2026-06-08
sources: ["什么我的狼人杀水平还不如AI.html"]
---
# Agent 接口多态

[[agentscope|AgentScope]] 中 UserAgent（人类玩家）与 ReActAgent（AI 玩家）实现同一接口，游戏编排器无需区分人/AI。

## 设计

| 组件 | 说明 |
|------|------|
| UserAgent | 人类玩家 Agent，通过注入 UserInputBase 获取输入 |
| ReActAgent | AI 玩家 Agent，通过 LLM 推理生成响应 |
| UserInputBase | 人类输入抽象接口，不同实现对应不同输入源 |
| WebUserInput | 基于浏览器的 UserInputBase 实现 |

## 接入方式

只需将 ReActAgent 替换为 UserAgent，游戏编排器代码零改动即可实现人机混合对战。

## WebUserInput 机制

详见 [[sinks-one-async-pattern|Sinks.One 异步等待模式]]：通过 Reactor `Sinks.One` 将浏览器异步输入转化为游戏同步等待。