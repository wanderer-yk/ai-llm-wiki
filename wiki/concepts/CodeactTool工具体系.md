---
type: concept
title: CodeactTool 工具体系（Dynamic Tools）
tags: [codeacttool, dynamic-tools, mcp, openapi, AssistantAgent, 工具扩展]
related: [AssistantAgent, code-as-action, mcp, mcp薄协议转换层, openclaw工具系统六层架构, function-calling-mcp-skills本质差异对比, 触发器三类型与自我调度, 多数据源统一检索, 回复渠道路由]
created: 2026-10-13
updated: 2026-10-13
sources: ["[202601150830]阿里AssistantAgent开源助力开发者快速构建答疑诊断智能助手.html"]
---
# CodeactTool 工具体系（Dynamic Tools）

CodeactTool 是 [[AssistantAgent]] 动态工具扩展（Dynamic Tools）模块定义的工具体系，以五类工具覆盖外部系统接入的主要路径，是 [[code-as-action]] 范式下 Agent 可编排动作的供给侧；内置工具类型含 Search / Reply / Trigger / Learning，与评估、回复、触发器、学习各专节模块互相印证。

## 五类工具

| 工具类别 | 覆盖能力 |
|------|------|
| MCP Tools | 一键接入任意 MCP Server，复用 MCP 工具生态 |
| HTTP API Tools | 按 OpenAPI 规范接入 REST API |
| Search Tools | 知识检索、项目搜索 |
| Trigger Tools | 定时任务、事件回调 |
| 自定义 Tools | 实现 `CodeactTool` 接口 |

## 要点

- **MCP 是工具接入的一等公民**：任意 MCP Server 一键接入（关联 [[mcp]]）；REST API 次之（OpenAPI 规范声明式接入）。这一优先级与 [[mcp薄协议转换层]]、[[function-calling-mcp-skills本质差异对比]] 中"MCP 作为标准化工具协议"的行业趋势一致。
- **触发器经工具创建**：Trigger Tools 的存在印证了 [[触发器三类型与自我调度]]——Agent 通过调用工具自主创建触发器，实现"自我调度"。
- **自定义工具**：实现 `CodeactTool` 接口即可扩展；接口方法签名与注册方式原文未披露。
- 与 [[openclaw工具系统六层架构]]（OpenClaw 的六层工具系统）为跨框架比较候选：前者按接入协议分类（MCP/OpenAPI/SPI），后者按运行时分层，反映两种工具组织哲学。
