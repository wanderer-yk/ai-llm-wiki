---
type: entity
title: 百炼
tags: [阿里云, 大模型平台, dashscope, llm]
related: [agentscope, agentscope-java, qwen3-plus, dashscope-multi-agent-formatter]
created: 2026-06-08
updated: 2026-06-08
sources: ["什么我的狼人杀水平还不如AI.html"]
---
# 百炼

阿里云大模型服务平台，通过 `bailian.console.aliyun.com` 获取 API Key。AgentScope Java 版使用 DashScope API 作为 LLM 后端，环境变量为 `DASHSCOPE_API_KEY`。

## 在 AgentScope 狼人杀中的角色

- 提供 [[qwen3-plus|qwen3-plus]] 模型作为 Agent 推理引擎
- [[dashscope-multi-agent-formatter|DashScopeMultiAgentFormatter]] 为百炼平台定制的多人对话格式化器
- Function Calling 结构化输出基于百炼 API 的工具调用能力