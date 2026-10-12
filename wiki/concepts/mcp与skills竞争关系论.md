---
type: concept
title: MCP 与 Skills 竞争关系论
tags: [mcp, skills, 竞争, agent]
related: [skills与mcp是互补还是竞争, mcp薄协议转换层, skills-sub-agent包装论, 三大武器库, skill-command-mcp三层架构, 生产级skill, function-calling-mcp-skills本质差异对比, 沈询]
created: 2026-10-12
updated: 2026-10-12
sources: ["[202602260830]AIAgent系列深入解析FunctionCallingMCP和Skills的本质差异与最佳实践.html"]
---
# MCP 与 Skills 竞争关系论

MCP 与 Skills 竞争大于互补，是[[沈询]]在其 AI Agent 系列第三篇中提出并完整论证的核心主张，与流行的"互补"看法相左——作者明确反驳"很多人认为互补"的观点。结论在"一句话总结"部分预告（两者是竞合关系，"竞争关系更大一些"），论证在正文完成。

## 论证结构

1. **同一问题**：两者解决的都是"整合多既有系统、实现复杂多步任务"
2. **不同方法**：
   - MCP：协议转换 Server + 代码型流程——在 MCP Server 内以代码串接 API（[[mcp薄协议转换层]]）
   - Skills：直接命令行 + 文字化流程——SKILL.md 描述流程，bash/curl 直接调用（[[skills-sub-agent包装论]]）
3. **选此失彼**：选 Skills 则无需在 MCP Server 写串接代码；反之若依赖 MCP Server 代码串接，Skills 的价值降低

## 跨源张力（待裁决）

本文竞争论与腾讯 CodeBuddy 实践的互补框架构成实质冲突：

- [[三大武器库]]：知识库（知道）+ MCP（连接）+ Skills（掌握）的互补三层赋能
- [[skill-command-mcp三层架构]]：Skill（核心逻辑）→ Command（薄壳路由）→ MCP Server（外部 API）的分层协作模型
- [[生产级skill]]：Skills 作为可复用 SOP 封装层

双方证据已汇集于查询页 [[skills与mcp是互补还是竞争]]，语境差异（编码工具内能力配置 vs 外部系统集成流程编排）有待裁决。

## 文内并存的张力表述

Skills"长流程任务成功率更高"与"不可能 100% 准确"两种表述同为作者立场，但适用条件未界定——非严格矛盾，但影响竞争论的证据强度。
