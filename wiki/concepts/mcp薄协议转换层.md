---
type: concept
title: MCP 薄协议转换层
tags: [mcp, 协议, json-rpc, function-calling]
related: [mcp, function-calling桥梁论, 任务流程定义困难问题, mcp与skills竞争关系论, skills-sub-agent包装论, 沈询]
created: 2026-10-12
updated: 2026-10-12
sources: ["[202602260830]AIAgent系列深入解析FunctionCallingMCP和Skills的本质差异与最佳实践.html"]
---
# MCP 薄协议转换层

MCP 薄协议转换层是[[沈询]]对 MCP 本质的定位：MCP 的核心仍基于 Function Calling，本质是在 FC 之上加的一层"很薄的协议转换"——FC + JSON-RPC 转换 + description 打包发布。客户端把 Function Calling 调用转换成 JSON+HTTP 请求发往 MCP Server，Server 调用既有系统并返回 JSON 响应。

## 产生原因

在纯 FC 框架下，每个既有系统都需要单独集成，工具集成成本太高；MCP 以标准化接口（JSON-RPC + 标准化工具描述）降低接驳成本，为 LLM 应用提供连接外部数据源和工具的统一方式。

## 调用链路（原文 code-snippet，原样保留）

```javascript
LLM -> Function Calling -> MCP Client -> JSON+HTTP请求 -> MCP Server -> 既有系统（GitHub/Slack/数据库等）
                                                                                    ↓
LLM <- Function Calling结果 <- MCP Client <- JSON响应 <- MCP Server <- 既有系统返回结果
```

## 定位的两个限定

- **接驳标准而非唯一接驳方式**：Skills 场景下用 bash/curl 直接本地调用即可替代 JSON-RPC 封装，说明 MCP 的协议封装并非必需（见 [[skills-sub-agent包装论]]）
- **未解决流程编排问题**：MCP 与 FC 共有"任务流程定义困难"两难（[[任务流程定义困难问题]]）——协议层解决了"怎么调"，没有解决"按什么顺序调"

## 治理新事实

本文记录：MCP 由 Anthropic 推动，现已捐赠给 Linux 基金会。该事实及本概念页的薄转换层定位、竞争论视角，可回填至 [[mcp]] 实体页。
