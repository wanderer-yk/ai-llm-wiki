---
type: concept
title: Skill/Command/MCP 三层架构
tags: [工具架构, claude-code, skill, mcp, agent工程化]
related: [agentic-engineering, claude-code, mcp, superpowers插件, 三大武器库, 生产级skill]
created: 2026-06-12
updated: 2026-06-12
sources: ["[202604171736]从VibeCoding到AgenticEngineering重构后台开发全流程.html"]
---
# Skill/Command/MCP 三层架构

腾讯开发者 [[seanguo]] 提出的 [[agentic-engineering]] 工具分层模型，基于 [[claude-code|Claude Code]] 构建后台开发全流程工具链。

## 三层结构

### Skill 层（核心）
- 核心业务逻辑载体，含独立工具权限白名单和执行流程
- 8 个 Skill：pm-dev、git-workflow、code-review、dtools、galileo-log-query、git-context、wiki-doc、service-analyzer
- Skill 间可链式调用（组合复用原则），`git-context` 被多模块复用为前置准备

### Command 层（薄壳）
- 轻量级路由入口，每个 `/xxx` Command 仅一行代码委托给 Skill
- 5 个 Command：/commit、/create-mr、/review-mr、/fix-mr、/analyze-codebase
- 自然语言（如"帮我提交代码"）与显式命令效果等价

### MCP Server 层（外部连接）
- 通过 [[mcp|MCP]] 协议连接外部平台 API，配置一次全局生效
- 5 个 MCP Server：GitPlatform、PM、Galileo、KnowledgeBase、InternalWiki
- 对用户透明（MCP 透明化原则），Skill 自动封装所有平台 API 调用

### Superpowers 子类
[[superpowers插件|Superpowers]] 提供的 brainstorming/writing-plans/executing-plans 被归为独立类型，构成"理解→计划→执行"强制纪律链。

## 四大设计决策

1. **Command 薄壳**：Command 仅是轻量级路由，核心逻辑全在 Skill 层
2. **Skill 可组合**：Skill 间链式调用，组合优于重复实现
3. **Superpowers 管纪律**：强制流程链防止 AI 跳过关键步骤自由发挥
4. **MCP 透明化**：用户无需感知 MCP 调用细节

## 与 [[三大武器库]] 的对比

| 维度 | Skill/Command/MCP 三层架构 | 三大武器库 |
|------|---------------------------|-----------|
| Skill 层 | 核心业务逻辑+工具权限白名单 | Skills（掌握） |
| Command 层 | 薄壳路由（新增层） | 无独立层 |
| MCP 层 | 外部平台 API 连接 | MCP（连接） |
| 知识层 | 通过 knot MCP 实现 | 知识库（知道） |
| 纪律层 | Superpowers 强制流程 | 无独立纪律层 |

## 物理实现

配置目录 `~/.claude-internal/` 下 `commands/` 对应 Command 层、`skills/` 对应 Skill 层、`settings.json` 管理权限。配置仓库为 [[dot-agents]]。

## 关联来源

- [[sources/[202604171736]从VibeCoding到AgenticEngineering重构后台开发全流程|[202604171736]从 Vibe Coding 到 Agentic Engineering：重构后台开发全流程]]