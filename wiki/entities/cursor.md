---
type: entity
title: Cursor
tags: [ai编程, ide, 代码编辑器]
related: [specflow, ai编程幻觉, 规格驱动ai开发]
created: 2026-06-12
updated: 2026-06-12
sources: ["[202603261200]治愈CursorAI编程的幻觉用它就够了.html"]
---
# Cursor

Cursor 是一款 AI 驱动的代码编辑器/IDE，具备 AI 对话、代码生成、Agent 模式等功能。在本文语境中，Cursor 是 [[specflow|Specflow]] 深度适配且**唯一支持**的 IDE 平台。

## 与 Specflow 的关系

- Specflow 的 Commands 和 Templates 通过注入项目 `.cursor` 目录实现集成
- Specflow 的运行环境要求为 **Cursor Chat (Agent Mode)**
- 天玑前端团队在一年的 Cursor 使用实践中发现，Cursor 的 AI 编程存在[[ai编程幻觉|幻觉]]问题——在复杂需求下因上下文断层和需求共识缺失导致效率波动
- Cursor Rules 与 Specflow 职责互补：前者管代码质量，后者管流程标准化