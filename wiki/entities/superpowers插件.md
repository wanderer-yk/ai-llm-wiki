---
type: entity
title: superpowers 插件
tags: [claude-code, 插件, skill, 结构化工作流]
related: [claude-code, agentic-engineering, skill-command-mcp三层架构, 结构化工作流skill]
created: 2026-06-12
updated: 2026-06-12
sources: ["[202604171736]从VibeCoding到AgenticEngineering重构后台开发全流程.html"]
---
# superpowers 插件

[[claude-code|Claude Code]] 插件，提供结构化工作流 Skill，是 [[seanguo]] [[agentic-engineering]] 体系中的核心纪律执行组件。

## 提供的 Skill

1. **brainstorming** — 交互式需求澄清，AI 先探索代码库了解现状，再通过提问-回答逐步明确需求边界
2. **writing-plans** — 制定结构化实施计划，生成含精确文件路径、代码变更描述、验证标准的可审核计划
3. **executing-plans** — 执行计划，支持 Sequential（顺序）和 Subagent-Driven（子 Agent 并行）两种模式
4. **subagent-driven-development** — 为每个 Task 派独立子 Agent 并行执行，完成后自动运行 spec review + code quality review
5. **verification-before-completion** — 完成前验证

## 核心定位

三个核心 Skill 构成"理解→计划→执行"的强制纪律链，确保 AI 先理解再动手、先计划再执行、按步骤不跳跃。这些不是"可选好习惯"，而是写入 Skill 定义的强制流程。

## 与 Skill 层的关系

在 [[skill-command-mcp三层架构]] 中，superpowers 提供的 Skill 被归为"Superpowers"类型而非普通"Skill"，暗示架构中存在 Skill 的子分类体系。

## 开放问题

- 开源还是商业产品？与 Claude Code 的官方关系？
- 是否支持用户自定义新的"纪律型 Skill"？

## 关联来源

- [[sources/[202604171736]从VibeCoding到AgenticEngineering重构后台开发全流程|[202604171736]从 Vibe Coding 到 Agentic Engineering：重构后台开发全流程]]