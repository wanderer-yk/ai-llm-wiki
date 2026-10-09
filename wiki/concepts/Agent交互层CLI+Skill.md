---
type: concept
title: Agent 交互层 CLI+Skill
tags: [agent接入, CLI, skill, MCP替代, 场景化]
related: [code-wiki, 意图化子命令设计, token预算优化输出格式, agent-skill知识包, skills真正价值三场景, skill-command-mcp三层架构, mcp]
created: 2026-10-09
updated: 2026-10-09
sources: ["[202604231830]从可观测到可理解用UModel构建Agent原生的代码知识图谱.html"]
---
# Agent 交互层 CLI+Skill

"Agent 交互层 CLI+Skill"是《[202604231830]从可观测到可理解用UModel构建Agent原生的代码知识图谱》确认的 Agent 查询侧接入形态：**不采用 MCP，而是 [[code-wiki]] CLI（query/check/ingest/status 四组命令）+ 场景化 Skill**。这一设计回答了文章中段的开放问题——Agent 如何接入 UModel 图谱。

## 设计逻辑

- **渐进式推理匹配**：search→context→impact 的推理链天然匹配 CLI 命令组合，与 [[渐进式工具加载]] 策略同构。
- **场景化 Skill**：Skill 按 RCA 排障/日常开发/架构治理三场景组织查询指南，使 Agent 免学 SPL 语法——是 [[agent-skill知识包]] 与 [[skills真正价值三场景]]（按场景组织能力）主张的又一实例。
- **token 预算控制**：CLI 默认 `--format brief`（单次 query context < 500 tokens），详见 [[token预算优化输出格式]]。

## 接入形态三方对照

| 接入形态 | 代表 | 特点 |
|----------|------|------|
| CLI + Skill | 本文 [[code-wiki]] | 渐进式推理、免学查询语言、token 优化输出 |
| MCP 工具 | [[deepwiki|DeepWiki]]、[[augment-code|Augment Code]] | 标准化协议、跨工具复用 |
| Skill-Command-MCP 三层 | seanguo [[skill-command-mcp三层架构]] | 编排串联外部 API |

三种路线的选择差异（何时 MCP 何时 CLI）尚无系统裁决，是本 Wiki 的开放对照点。