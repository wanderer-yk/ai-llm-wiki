---
type: entity
title: DashScopeMultiAgentFormatter
tags: [agentscope, 格式化器, dashscope, 通义千问]
related: [agentscope, bailian, qwen3-plus, multi-agent-formatter]
created: 2026-06-08
updated: 2026-06-08
sources: ["什么我的狼人杀水平还不如AI.html"]
---
# DashScopeMultiAgentFormatter

[[agentscope|AgentScope]] 为阿里云 [[bailian|百炼]]（DashScope）平台提供的多人对话格式化器实现类。详见 [[multi-agent-formatter|多智能体格式化器]]。

## 接入方式

仅需一行代码：`.formatter(new DashScopeMultiAgentFormatter())`

## 原理

通过 `Msg.name` 字段标记说话者身份（消息标记），再将多条独立消息合并为一条带 `<history>` 标签的 user 消息（消息合并），巧妙绕开 LLM API 仅支持 system/user/assistant 三种角色的限制。

## 效果

9人2轮讨论从 18 条 user 消息压缩为 1 条带 `<history>` 的 user 消息。