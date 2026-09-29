---
type: entity
title: knot
tags: [mcp-server, 知识库, 语义搜索]
related: [mcp, superpowers插件, claude-code]
created: 2026-06-12
updated: 2026-06-12
sources: ["[202604171736]从VibeCoding到AgenticEngineering重构后台开发全流程.html"]
---
# knot

知识库 MCP Server，用于内部 Wiki 文档检索和语义搜索，辅助设计决策。

## 功能

- 内部 Wiki 文档的语义搜索
- 为 [[superpowers插件|brainstorming]] 阶段提供技术背景知识
- 辅助 AI 在需求澄清阶段做出更符合项目现状的设计决策

## 在[[十一阶段后台开发流程]]中的角色

阶段②（交互式需求澄清）中，`superpowers:brainstorming` Skill 组合调用 `wiki-doc`（Skill）+ `knot`（MCP）进行知识检索。

## MCP 映射

在 MCP Server 配置表中，knot 归属于 KnowledgeBase MCP，为 brainstorming 阶段补充技术背景。

## 关联来源

- [[sources/[202604171736]从VibeCoding到AgenticEngineering重构后台开发全流程|[202604171736]从 Vibe Coding 到 Agentic Engineering：重构后台开发全流程]]