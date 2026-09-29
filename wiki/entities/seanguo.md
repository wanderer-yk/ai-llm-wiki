---
type: entity
title: seanguo
tags: [腾讯, 开发者, claude-code, agentic-engineering]
related: [腾讯技术工程, 腾讯程序员, claude-code, agentic-engineering, skill-command-mcp三层架构]
created: 2026-06-12
updated: 2026-06-12
sources: ["[202604171736]从VibeCoding到AgenticEngineering重构后台开发全流程.html"]
---
# seanguo

腾讯开发者，[[腾讯技术工程]]公众号文章《从 Vibe Coding 到 Agentic Engineering：重构后台开发全流程》的作者，署名于正文首行。

## 核心贡献

- 提出[[agentic-engineering]]（智能体工程）方法论：人在关键节点审核、AI 在结构化流程中自主执行
- 设计[[skill-command-mcp三层架构]]工具分层模型
- 构建[[十一阶段后台开发流程]]：从需求创建到合入发布的完整后台开发全流程实践
- 概括[[prompt-and-pray]]（"提示即祈祷"）一词精准描述 Vibe Coding 的本质

## 工具链体系

基于 [[claude-code]] 构建了包含 8 个 Skill、5 个 Command、5 个 MCP Server 的完整工具链，配置仓库为 [[dot-agents]]（git.example.com/alice/dot-agents）。

## 关键理念

- 开发者角色从"亲自执行"转变为"审核确认"
- 自定义 Skill 的核心价值是编排和串联现有能力，而非从零实现（"不要重复造轮子"）
- [[superpowers插件|Superpowers]] 的三个 Skill 构成"理解→计划→执行"的强制纪律链
- 发布环节是唯一明确不交给 AI 的阶段（灰度策略和线上风险）

## 关联来源

- [[sources/[202604171736]从VibeCoding到AgenticEngineering重构后台开发全流程|[202604171736]从 Vibe Coding 到 Agentic Engineering：重构后台开发全流程]]