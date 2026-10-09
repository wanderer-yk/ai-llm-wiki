---
type: entity
title: Claude Code
tags: [工具, anthropic, cli, 编码agent]
related: [zhiyuanfu, 24h打工人, codex, gemini-cli, cursor]
created: 2026-06-12
updated: 2026-10-09
sources: ["[202605071734]十年老技术开发的AIAgent探索之路.html", "[202601261830]AI编程实践从ClaudeCode实践到团队协作的优化思考得物技术.html", "[202605090830]Harness实践让Agent自动制作知识讲解视频.html", "[202604151800]读完ClaudeCode源码才发现SkillsMCPRules的区别远没有你想的那么大.html"]
---
# Claude Code

Anthropic 推出的 Claude CLI 工具，具备读文件、改代码、跑命令能力。区别于 Claude 本身（用于文档生成），Claude Code 是完整的 CLI Agent。在 [[zhiyuanfu]] 的 [[24h打工人]] 系统中作为调度层调用的终端工具之一。

## 来自得物技术篇的证据（2026-01）

- **得物团队级方法论实证（2026-01，最丰富单来源）**：在 Claude Code 中构建协调者+四核心角色子代理系统（技术方案架构师/代码审查专家/代码实现专家/前端页面生成器，见 [[子代理协作模式]]），并沉淀系统提示词护栏论、[[约束衰减]]（第 1/5/10 轮衰减）、[[三阶段对话模型]]、[[上下文管理四技巧]]、[[AI编程局限性四认知]] 等团队级方法论。
- 区别于既有索引中 zhiyuanfu「纯 grep 方案、Token 消耗多」的工具级记录，本源为团队协作视角的深度实践。

## 来自 ConardLi Harness 实践篇的证据（2026-05）

- **SubAgent 与 Agent Teams 双模式**（实验特性）：子代理模式成熟稳定；Agent Teams 支持多 Agent 并行开发，实测并行章节开发**最大 3**（经验值，超过易冲突）。
- 工具链适配：`tmux` 多窗格并行会话（Agent Teams 适配配置仅见截图，键值存疑保留）、`.claude/skills` 承载 Skill 级 Harness、`--dangerously-skip-permissions` 跳过权限确认的边界=**仅限可信目录**（原文：「不要在陌生仓库里随便开」）。
- 实验开关：Agent Teams 需手动开启实验特性，尚非默认能力。

## 源码级注入机制证据（百度 Cheer，2026-04）

- **三通道注入机制**（与既有「纯 grep 代码匹配方案」结论并存，分属不同机制层）：Rules 走 `prependUserContext()` 被动注入 messages 最前部（`<system-reminder>` 包裹，不走 tool_use）；MCP 走 `tools[]` 注册+真实 JSON-RPC；Skills 走 `skill_listing` attachment 常驻+`tool_use` 触发注入 SKILL.md（本质提示词注入）（证据等级：v2.1.88 泄漏源码、版本特定、非官方口径，引用链未独立核实）。
- **附件与记忆机制**：`nested_memory` 按需加载（目录级 CLAUDE.md 发现经 `getMemoryFiles`/`processMemoryFile` 流水线）；`/skill-name` 手动触发链路与 `@file` FileAttachment （证据等级：v2.1.88 泄漏源码、版本特定、非官方口径，引用链未独立核实）。
- **Skill 双执行模式**：Inline（默认，指令注入当前上下文）与 Fork（独立上下文隔离执行），见 [[skill双执行模式]]。
- 泄漏源码出处待核（文中链接指向官方仓库但官方不发布源码），已登记 [[sources/[202604151800]读完ClaudeCode源码才发现SkillsMCPRules的区别远没有你想的那么大|源页]] 开放问题。
