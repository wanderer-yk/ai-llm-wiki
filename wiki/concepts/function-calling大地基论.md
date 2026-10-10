---
type: concept
title: Function Calling 大地基论
tags: [function-calling, tool, harness, agent架构]
related: [结构化输出, mcp工具封装模式, agent框架三要素, 受控子Agent机制, harness工程三部曲演进论, SubAgent生命周期工具化, RAG能力MCP服务化, command-vs-skill]
sources: ["[202604220830]AI实践基于SpringAI从0到1构建AIAgent.html"]
created: 2026-10-10
updated: 2026-10-10
---
# Function Calling 大地基论

Function Calling 大地基论是[[觖弦]]提出的 Agent 架构判断：**Agent 开发一般指 Harness 开发，而 Function Calling 是 Harness 的大地基**——LLM 本身不会调用工具，工具调用由 Harness 完成；Skill 本质是一种 Tool，RAG、SubAgent 与外部 MCP 服务等能力在工程实践中也大量被做成 Tool 由 LLM 决策调用。

> 「Skill 本质就是一种 Tool，而 RAG、SubAgent 与外部 MCP 服务等能力在工程实践中也大量被做成一种 Tool 由 LLM 决策调用」

## 论断的机制化落地（贯穿全文）

该论断在文章前言提出（chunk 5），并在各章获得机制级确认：

| 能力 | Tool 化形态 | 出处 |
|------|------|------|
| Skill | 注册为 ToolCallback，LLM 依 description 自主调用 | 第四章 |
| SubAgent | create/chat_with/destroy 三工具暴露全生命周期（[[SubAgent生命周期工具化]]） | 第五章 |
| RAG | `knowledge_search` 工具供 LLM 主动检索（[[RAG工具化双路径]]） | 第二、三章 |
| 外部 MCP 服务 | `{mcp_tool_name}` 从 MCP Server 发现并注册 | 第六章 |
| 外部 AI 应用 | `call_ideas_{name}` 调用 IdeaLab 平台 | 第二章 |

配套的机制表述是「执行位置论」：LLM 只返回调用意向，真实执行发生在 Agent 服务端——与 [[结构化输出]]（Function Calling 确保决策可靠解析）互为表里。

## 跨源印证

- [[mcp工具封装模式]]（转转）：`@mcp.tool()` 将多步流水线封装为 AI 可调用的单一工具——同一「能力 Tool 化」共识；
- [[受控子Agent机制]]（Hermes）：子 Agent 经工具暴露给主 Agent——SubAgent 工具化的另一独立实现；
- [[agent框架三要素]]（腾讯 yabohe）：Tools Call 作为框架三要素之一——地基论在框架分层中的位置；
- [[agent-loop]]：工具调用循环是 Agent Loop 的驱动机制。

## 意义

若一切能力最终都收敛为 Tool，则工具定义的质量（命名、描述、参数 Schema）成为 Agent 能力的决定性因素——这正是 [[工具定义最终论]] 预测「工具定义可能会走到最后」的逻辑基础。