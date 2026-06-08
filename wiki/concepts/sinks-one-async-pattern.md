---
type: concept
title: Sinks.One 异步等待模式
tags: [reactor, 异步, human-in-the-loop, webflux, sse]
related: [agent-interface-polymorphism, dual-perspective-architecture, ai-werewolf-game]
created: 2026-06-08
updated: 2026-06-08
sources: ["什么我的狼人杀水平还不如AI.html"]
---
# Sinks.One 异步等待模式

Human in the Loop 的核心工程实现，基于 Reactor `Sinks.One` 将异步用户输入转化为游戏同步等待。

## 数据流

```
游戏线程创建 Sinks.One
    → SSE 通知前端（WAIT_USER_INPUT 事件）
    → Mono 阻塞游戏线程
    → 用户在浏览器输入
    → REST 提交（/api/game/input）
    → submitInput() 调用 tryEmitValue 解除等待
    → 游戏继续
```

## WebUserInput 核心方法

- `waitForInput()`：创建 Sink 并返回 Mono，阻塞游戏流程等待人类输入
- `submitInput()`：用户提交后解除等待，游戏流程恢复

## 设计亮点

- 游戏逻辑保持同步线性编写，无需回调地狱
- WebFlux 的响应式模型天然支持 SSE 推送与异步等待的协调
- 与 [[dual-perspective-architecture|SSE 双视角架构]]无缝集成