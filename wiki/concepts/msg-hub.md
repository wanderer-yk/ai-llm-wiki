---
type: concept
title: MsgHub 消息频道机制
tags: [消息广播, 发布订阅, 信息隔离, 频道, agentscope]
related: [agentscope, ai-werewolf-game, call-and-observe-pattern, message-channel-isolation]
created: 2026-06-08
updated: 2026-06-08
sources: ["什么我的狼人杀水平还不如AI.html"]
---
# MsgHub 消息频道机制

[[agentscope|AgentScope]] 核心能力之一，基于发布-订阅模式实现多智能体间消息自动广播与信息隔离。

## 频道隔离

不同场景使用不同的 MsgHub 频道，参与者列表不同，天然实现信息隔离：
- 狼人夜谈频道：仅狼人参与
- 白天讨论频道：全员参与

## 广播模式

| 模式 | 场景 | 机制 |
|------|------|------|
| 自动广播 | 讨论阶段 | 实时推送，`call()` 触发言论 |
| 手动广播 | 投票阶段 | 延迟公布，收集后统一 `broadcast()` |

手动广播设计防止跟票，体现对游戏公平性的工程保障。

## call/observe 双模式

- `call()`：主动发言，触发 LLM 推理并返回响应
- `observe()`：被动接收，仅写入 Memory 不触发回复

这种分离设计将"接收信息"和"生成响应"解耦。详见 [[call-and-observe-pattern|call/observe 双模式]]。