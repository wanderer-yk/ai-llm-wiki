---
type: entity
title: Claude Code
tags: [工具, anthropic, cli, 编码agent]
related: [zhiyuanfu, 24h打工人, codex, gemini-cli, cursor]
created: 2026-06-12
updated: 2026-10-08
sources: ["[202605071734]十年老技术开发的AIAgent探索之路.html", "[202601261830]AI编程实践从ClaudeCode实践到团队协作的优化思考得物技术.html"]
---
# Claude Code

Anthropic 推出的 Claude CLI 工具，具备读文件、改代码、跑命令能力。区别于 Claude 本身（用于文档生成），Claude Code 是完整的 CLI Agent。在 [[zhiyuanfu]] 的 [[24h打工人]] 系统中作为调度层调用的终端工具之一。

## 来自得物技术篇的证据（2026-01）

- **得物团队级方法论实证（2026-01，最丰富单来源）**：在 Claude Code 中构建协调者+四核心角色子代理系统（技术方案架构师/代码审查专家/代码实现专家/前端页面生成器，见 [[子代理协作模式]]），并沉淀系统提示词护栏论、[[约束衰减]]（第 1/5/10 轮衰减）、[[三阶段对话模型]]、[[上下文管理四技巧]]、[[AI编程局限性四认知]] 等团队级方法论。
- 区别于既有索引中 zhiyuanfu「纯 grep 方案、Token 消耗多」的工具级记录，本源为团队协作视角的深度实践。
