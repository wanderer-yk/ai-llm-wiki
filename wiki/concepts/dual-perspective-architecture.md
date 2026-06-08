---
type: concept
title: 双视角架构
tags: [sse, 实时推送, 观战, 复盘, spring-webflux, 事件过滤]
related: [agentscope, ai-werewolf-game, sinks-one-async-pattern]
created: 2026-06-08
updated: 2026-06-08
sources: ["什么我的狼人杀水平还不如AI.html"]
---
# 双视角架构

AI 狼人杀中同时满足观战沉浸感与赛后复盘需求的 SSE 实时推送架构。

## 两种视角

| 视角 | 推送方式 | 用途 |
|------|----------|------|
| 玩家视角 | 实时过滤推送，按角色权限过滤事件 | 观战沉浸体验 |
| 上帝视角 | 全量保存，游戏结束后通过 `/api/game/replay` 复盘 | "原来 5 号真的是预言家" |

## 事件可见性过滤

`EventVisibility.isVisibleTo(role)` 控制不同角色看到不同事件流：
- 村民看不到狼人夜间讨论
- 观战者默认使用"村民视角"

## 技术实现

- **后端**：Spring WebFlux `Flux<ServerSentEvent>` 原生 SSE 支持
- **前端**：浏览器原生 `EventSource` API 按事件类型分发（`player_speak`、`phase_change`、`game_end`）
- **异步模型**：游戏逻辑通过 `Schedulers.boundedElastic()` 后台运行，事件流通过 SSE 实时推送

## GameEventEmitter

双视角事件发射器，包含 `playerSink`（实时过滤推送）和 `godViewHistory`（全量保存），在发言、阶段切换、投票结果、玩家淘汰等关键节点调用 `emit` 方法。