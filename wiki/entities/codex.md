---
type: entity
title: Codex
tags: [工具, openai, 编码agent]
related: [zhiyuanfu, 24h打工人, gemini-cli, claude-code, cursor]
created: 2026-06-12
updated: 2026-10-09
sources: ["[202605071734]十年老技术开发的AIAgent探索之路.html", "[202604201800]我把Karpathy的AutoResearch搬到了软件开发领域效果炸了.html"]
---
# Codex

OpenAI 推出的编码 Agent 工具。在 [[zhiyuanfu]] 的 [[24h打工人]] 系统中，Codex 是调度层轮询调用的主要 CLI 工具之一，具备完整的代码读写和执行能力。当配额耗尽时，[[24h打工人]] 的 ToolProber 组件会自动切换到备用工具（[[gemini-cli]]）并冷却 5 分钟。

## autoresearch 双 Agent 终端角色（2026-04）

- 在 smallnest/autoresearch 中 Codex 任**实现者**角色：`agents/codex.md` 承载实现者指令+代码规范+自检清单，经 [[acpx]] 在命令行与 Claude Code 组成双 Agent 终端，[[奇偶轮角色互换]] 调度。
- 案例证据：Issue #21 Codex 首轮仅 1.0 分（只读代码未实现），经 Claude 审核指出不足后迭代改善。
