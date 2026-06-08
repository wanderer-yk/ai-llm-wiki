---
type: concept
title: "call/observe 双模式"
tags: [消息交互, 解耦, agentscope, msg-hub]
related: [msg-hub, agentscope, ai-werewolf-game]
created: 2026-06-08
updated: 2026-06-08
sources: ["什么我的狼人杀水平还不如AI.html"]
---
# call/observe 双模式

[[msg-hub|MsgHub]] 中的两种消息交互模式，将"接收信息"与"生成响应"解耦。

## 机制

- **`call()`**：主动发言。触发 LLM 推理，生成响应并返回给频道。用于讨论阶段的轮流发言。
- **`observe()`**：被动接收。将频道消息写入 Agent 的 Memory，但不触发 LLM 推理、不生成回复。用于让 Agent "听"到他人发言。

## 设计意图

这种分离使得 Agent 可以"倾听"所有同频道发言（更新上下文）而不必每次都做出响应，既节省 Token 又符合轮流发言的游戏规则。