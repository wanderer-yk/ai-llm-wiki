---
type: entity
title: Cursor
tags: [ai-coding, ide, 编辑器]
related: [specflow, ai-bian-cheng-huan-jue, spec-driven-development]
created: 2026-06-08
updated: 2026-06-08
sources: ["治愈CursorAI编程的幻觉用它就够了.html"]
---
# Cursor

**Cursor** 是一款 AI 编程工具/编辑器，支持 AI 辅助代码生成、对话式开发和 Agentic Workflow。它是 [[specflow|Specflow]] 深度适配的唯一目标 IDE。

## 核心特性

- 支持 Chat Agent Mode（Specflow 的运行环境）
- 提供 `.cursor` 目录用于注入自定义 commands 和 templates
- 支持 Cursor Rules 作为项目级代码规范（AI 编码的"交通规则"）
- 支持 Agentic Workflow 模式

## 在 Wiki 中的讨论

### 问题：AI 编程"幻觉"
[[ai-bian-cheng-huan-jue|AI 编程幻觉]] 是 Cursor 等 AI 编程工具生成看似合理但实际错误的代码或建议的问题。天玑前端团队发现，早期 [[vibe-coding|Vibe Coding]] 模式在复杂中后台场景下因缺乏标准化约束导致碎片化输出。

### 解决方案：Specflow
[[specflow|Specflow]] 专为 Cursor 定制，通过以下方式深度适配：
- 利用 Cursor 的 `.cursor` 目录注入 commands 和 templates
- 依赖 Cursor Chat Agent Mode 运行
- 与 Cursor Rules 互补——Specflow 管流程路径，Rules 管代码质量
- 将社区 SDD 方案（[[openspec|OpenSpec]]、[[github-spec-kit|GitHub Spec Kit]]、[[bmad-method|BMAD-METHOD]]）在 Cursor 中的"摩擦"（心智负担重、状态易丢失）作为自研的动机