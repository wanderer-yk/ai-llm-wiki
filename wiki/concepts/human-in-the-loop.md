---
type: concept
title: Human in the Loop
tags: [人机交互, UserAgent, 异步等待, SSE]
related: [agentscope, ai狼人杀]
created: 2026-06-12
updated: 2026-06-12
sources: ["[202601082000]什么我的狼人杀水平还不如AI.html"]
---
# Human in the Loop

## 简介

Human in the Loop 是[[agentscope|AgentScope]] 框架的核心能力之一，使人类玩家能无缝加入多 Agent 对局。其核心设计思想是**接口一致性**——UserAgent 与 ReActAgent 实现同一接口，游戏编排器无需区分人类和 AI 玩家。

## 核心机制

### UserAgent

- 与 ReActAgent 实现同一接口
- 通过注入不同的 `UserInputBase` 获取用户输入
- 编排器代码完全不变，仅初始化时将某个 ReActAgent 替换为 UserAgent

### WebUserInput 异步等待

基于 Project Reactor 的 `Sinks.One` 实现异步等待：

1. **`waitForInput()`** — 创建 `Sinks.One` → 放入 `pendingInputs` 映射 → SSE 发送 `WAIT_USER_INPUT` 事件 → 返回 Mono 挂起游戏线程
2. **`submitInput()`** — 前端调用 `/api/game/input` → 从 `pendingInputs` 取出对应 Sink → `tryEmitValue` 解除等待

### 完整数据流

```
游戏等待 → SSE 通知前端 → 用户在浏览器输入 → REST 提交 → 游戏线程唤醒继续
```

## 双视角事件系统

| 视角 | 特点 | 用途 |
|------|------|------|
| 玩家视角 | 实时推送 + 按角色过滤信息 | 正常对局体验 |
| 上帝视角 | 全量保存不过滤 | 赛后复盘（`/api/game/replay`） |

GameEventEmitter 采用双轨架构：`playerSink` 按 EventVisibility 过滤后实时推送，`godViewHistory` 无条件全量保存。

## 玩家体验

- 可选择任意角色加入（预言家、狼人或随机角色）
- 无需凑齐人数即可开局
- SSE 实时展示 AI 对局过程，前端按事件类型（player_speak / phase_change / game_end）分别渲染
