---
type: query
title: "Skills 与 MCP 是互补还是竞争"
tags: [mcp, skills, 争议, agent]
related: [mcp与skills竞争关系论, 三大武器库, skill-command-mcp三层架构, 生产级skill, function-calling-mcp-skills本质差异对比, lynxe, 沈询, codebuddy]
created: 2026-10-12
updated: 2026-10-12
sources: ["[202602260830]AIAgent系列深入解析FunctionCallingMCP和Skills的本质差异与最佳实践.html"]
---
# Skills 与 MCP 是互补还是竞争

**状态**：开放。双方证据材料已齐备，语境差异假设待验证。

## 问题陈述

[[沈询]]（千问AI平台 AI Agent 系列第三篇）主张 MCP 与 Skills **竞争大于互补**——两者是同一问题（整合多既有系统完成复杂多步任务）的两种解法，选此失彼；而腾讯 CodeBuddy 实践将知识库/MCP/Skills 并列为**互补**的三层赋能。两种框架哪个更接近普遍规律，还是各自成立于不同语境？

## 竞争论证据（本文，[[mcp与skills竞争关系论]]）

- 论证结构：同一问题 + 不同方法（协议转换 Server + 代码型流程 vs 直接命令行 + 文字化流程）+ 选此失彼
- 作者明确反驳"很多人认为互补"的流行观点
- 技术依据：Skills 用 bash/curl 直接调用即可替代 JSON-RPC 封装；选 Skills 则无需在 MCP Server 写串接代码
- Skills 的两大未解问题（[[skills两大未解问题]]）由 Func-Agent 函数化路线回应，而非由 MCP 补足

## 互补论证据（腾讯 CodeBuddy）

- [[三大武器库]]：知识库（知道）+ MCP（连接）+ Skills（掌握）三层赋能架构
- [[skill-command-mcp三层架构]]：Skill（核心逻辑）→ Command（薄壳路由）→ MCP Server（外部 API）的分层模型，各层职责不同、协同工作
- [[生产级skill]]：Skills 作为可复用 SOP 封装层，与 MCP 工具接入并存

## 语境差异假设（待裁决）

- **竞争论语境**：外部系统集成与多步任务流程编排（通用 Agent 能力栈视角）——Skills 与 MCP 在"怎么让 Agent 完成多步集成任务"上正面竞争
- **互补论语境**：编码工具内面向开发者的能力配置——知识库解决"知道"、MCP 解决"连接"、Skills 解决"掌握"，三者解决不同问题
- **可能的调和**：在编码助手场景三者互补（各司其职），在系统集成场景 Skills 与 MCP 竞争（同一问题的两种解法）；裁决需回到各源的具体使用场景对照

## 下一步

- 在比较页 [[function-calling-mcp-skills本质差异对比]] 中呈现双方语境差异
- 关注 AI Agent 系列第 4/5 篇（上下文管理、并行执行）是否延续竞争论
- 若有新的 Skills/MCP 共存实践案例（尤其是与 [[lynxe]] 或 [[codebuddy]] 相关的），回填本页证据
