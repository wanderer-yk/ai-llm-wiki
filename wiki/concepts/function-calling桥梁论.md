---
type: concept
title: Function Calling 桥梁论
tags: [function-calling, 结构化输出, 工具调用]
related: [结构化输出, function-calling大地基论, mcp薄协议转换层, skills四阶段运行流程, 任务流程定义困难问题, 沈询, react-agent, agent框架三要素]
created: 2026-10-12
updated: 2026-10-12
sources: ["[202602260830]AIAgent系列深入解析FunctionCallingMCP和Skills的本质差异与最佳实践.html"]
---
# Function Calling 桥梁论

Function Calling 桥梁论是[[沈询]]对 Function Calling 本质的概括：FC 是在 LLM 的非结构化自然语言需求与既有系统的结构化函数调用之间架设的桥梁——数据库、API、文件系统等既有系统只能处理结构化信息，而 LLM 擅长非结构化文本，FC 完成两者之间的转换。这一桥梁是 MCP 与 Skills 得以存在的前置条件：三者中 FC 是基础能力，后两者均建立其上。

## 机制支撑：结构化输出与工具调用四步流程

1. LLM 接收用户请求和工具描述
2. LLM 决定是否调用工具并生成结构化 JSON（关键字段 `tool_calls`、`function.name`、`function.arguments`、`finish_reason: "tool_calls"`）
3. 系统解析并执行工具调用
4. 将结果返回给 LLM，LLM 据此决定继续调用工具或生成最终回答

作者强调：结构化输出让系统无需复杂文本解析即可稳定解析 LLM 意图，是 Function Calling 让工具调用"可预测和可靠"的核心机制。完整的响应 JSON 示例与系统侧解析代码见[[sources/[202602260830]AIAgent系列深入解析FunctionCallingMCP和Skills的本质差异与最佳实践|来源页]]。

## 跨源互证

- [[function-calling大地基论]]（觖弦/SpringAI 文）：FC 是 Agent 能力的大地基，与本文"后两者存在的基础"论断一致
- [[结构化输出]]：基于 Function Calling 的结构化输出机制，确保 LLM 决策可靠解析
- [[react-agent]]、[[agent框架三要素]]：工具调用机制的语料相通

## 局限

FC 本身未解决"任务流程定义困难"（[[任务流程定义困难问题]]）——多步任务的流程编排单靠 FC 的单次结构化调用无法覆盖，这正是 MCP 与 Skills 两条路线的共同起点。
