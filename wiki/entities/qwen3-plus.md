---
type: entity
title: qwen3-plus
tags: [通义千问, 模型, 阿里云, llm]
related: [bailian, agentscope, dashscope-multi-agent-formatter]
created: 2026-06-08
updated: 2026-06-08
sources: ["什么我的狼人杀水平还不如AI.html"]
---
# qwen3-plus

通义千问模型版本，在 AgentScope 狼人杀代码示例中作为默认模型使用。通过 [[bailian|阿里云百炼]]（DashScope）平台提供 API 服务。

## 在项目中的使用

- ReActAgent 创建时通过 `model` 参数指定
- [[dashscope-multi-agent-formatter|DashScopeMultiAgentFormatter]] 针对此模型的 API 格式优化消息合并策略
- 支持 Function Calling 用于 [[function-calling-structured-output|结构化输出]]