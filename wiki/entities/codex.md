---
type: entity
title: Codex
tags: [工具, openai, 编码agent]
related: [zhiyuanfu, 24h打工人, gemini-cli, claude-code, cursor]
created: 2026-06-12
updated: 2026-06-12
sources: ["[202605071734]十年老技术开发的AIAgent探索之路.html"]
---
# Codex

OpenAI 推出的编码 Agent 工具。在 [[zhiyuanfu]] 的 [[24h打工人]] 系统中，Codex 是调度层轮询调用的主要 CLI 工具之一，具备完整的代码读写和执行能力。当配额耗尽时，[[24h打工人]] 的 ToolProber 组件会自动切换到备用工具（[[gemini-cli]]）并冷却 5 分钟。