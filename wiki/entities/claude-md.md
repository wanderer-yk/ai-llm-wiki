---
type: entity
title: CLAUDE.md
created: 2026-06-22
updated: 2026-06-22
tags: [Claude Code, 声明式配置, Skill定义]
related: [claude-code, agent-skills, skill-command-mcp三层架构]
sources: ["[202602271818]AgentSkills与MCP一场被误解的替代战争.html"]
---
# CLAUDE.md

CLAUDE.md 是 [[claude-code]] 项目根目录的 Skill 声明文件，采用 Markdown 格式定义业务流程。

## 结构

CLAUDE.md 包含三段式结构：
1. **触发条件** — 定义何时激活该 Skill
2. **执行流程** — 以自然语言描述步骤，每步引用 [[mcp]] 工具名完成调用
3. **安全规则** — 定义操作约束（如部署安全规则四原则：未经测试不部署/破坏性操作需确认/部署前必须备份/生产部署需批准）

## 工程意义

CLAUDE.md 是 [[agent-skills]] "Skills Markdown 声明式配置"概念的实际载体，证明了 Skills 层可以使用 Markdown 而非代码定义业务流程，使非工程师（产品经理、QA）也能理解并参与流程优化。这与 [[skill-command-mcp三层架构]] 中 Skill 作为核心逻辑编排层的定位一致。