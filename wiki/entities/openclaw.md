---
type: entity
title: OpenClaw
tags: [ai-agent, 产品, 平台]
related: [manus, pi-agent, agent-loop, 文件系统作为上下文]
created: 2026-06-12
updated: 2026-06-12
sources: ["[202604131736]详尽地带你从零开始设计实现一个AIAgent框架.html"]
---
# OpenClaw

AI Agent 平台（openclaw.ai），2026 年初爆火的 Agent 产品。在 [[sources/[202604131736]详尽地带你从零开始设计实现一个AIAgent框架|yabohe 的文章]]中作为开篇引子，引出 AI Agent 商用化趋势的讨论。

## 核心特征

- 使用文件系统保存 Agent 长期记忆：SOUL.md / TOOLS.md / MEMORY.md
- 底层 Agent Core 为 [[pi-agent|Pi Agent]]，工具层仅有 4 个方法：Read、Write、Edit、Shell
- 其余能力通过事件机制与 Skills 扩展实现

## 与本文极简框架的对比验证

yabohe 指出 OpenClaw Pi Agent 的 4 工具设计（Read/Write/Edit/Shell）与本文极简框架的 4 工具（shell_exec/file_read/file_write/python_exec）高度一致，印证了极简工具集在工业级 Agent 产品中的可行性。