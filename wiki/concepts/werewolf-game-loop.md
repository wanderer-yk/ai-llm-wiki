---
type: concept
title: 狼人杀游戏循环
tags: [游戏循环, 编排, 多智能体, 狼人杀, game-state]
related: [agentscope, msg-hub, function-calling-structured-output, ai-werewolf-game]
created: 2026-06-08
updated: 2026-06-08
sources: ["什么我的狼人杀水平还不如AI.html"]
---
# 狼人杀游戏循环

AI 狼人杀的完整游戏阶段循环，由编排器（GameState/AgentRun）统一调度。

## 四阶段循环

### 1. 夜晚阶段
- **狼人讨论**：通过 [[msg-hub|MsgHub]] 私密频道讨论，[[function-calling-structured-output|结构化输出]]收集击杀目标
- **神职行动**：预言家/女巫/猎人独立调用（无需 MsgHub），信息直接写入 Memory 实现隐私

### 2. 白天讨论
- 全员通过 MsgHub 自动广播，轮流发言
- [[multi-agent-formatter|格式化器]]处理多说话者消息

### 3. 投票放逐
- 关闭自动广播，改为手动广播
- 结构化输出收集投票结果
- 统一 `broadcast()` 公布，防止跟票

### 4. 胜负判定
- 每阶段结束调用 `checkWerewolvesWin()` 和 `checkVillagersWin()`
- GameState 的 `currentRound` 追踪轮次
- 未分胜负则进入下一轮

## 编排器职责

编排器统一管理四项核心：GameState（游戏状态）、MsgHub 生命周期、Memory 上下文、结构化输出收集。