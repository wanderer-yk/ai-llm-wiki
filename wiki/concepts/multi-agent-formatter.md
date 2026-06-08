---
type: concept
title: 多智能体格式化器
tags: [格式化器, 多说话者, 消息合并, llm-api限制]
related: [agentscope, dashscope-multi-agent-formatter, msg-hub, ai-werewolf-game]
created: 2026-06-08
updated: 2026-06-08
sources: ["什么我的狼人杀水平还不如AI.html"]
---
# 多智能体格式化器

[[agentscope|AgentScope]] 核心能力之一，解决 LLM API 仅支持 system/user/assistant 三种角色、无法区分多说话者的限制。

## 问题

在 9 人狼人杀中，"3 号玩家说…""5 号玩家说…"这类多人对话，LLM API 无法通过 role 区分。

## 解决方案：两步法

### 第一步：消息标记

框架自动将 Agent 的 `name` 绑定到 `Msg.name` 字段。

### 第二步：消息合并

将多条独立消息合并为一条带 `<history>` 标签的 user 消息：

```
<history>
3号玩家: 我觉得5号有问题...
5号玩家: 我是好人，3号在血口喷人...
</history>
```

## 效果

9 人 2 轮讨论从 18 条 user 消息压缩为 1 条，显著减少 API 调用开销。

## 多模型支持

AgentScope 为通义千问（[[dashscope-multi-agent-formatter|DashScopeMultiAgentFormatter]]）、GPT、Claude、Gemini 等均提供对应 Formatter 实现。