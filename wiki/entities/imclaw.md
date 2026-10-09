---
type: entity
title: imclaw
tags: [imclaw, agent蜂群, 微信, 飞书, agent操控层]
related: [鸟窝, smallnest-autoresearch, acpx, openclaw, pi-agent, codex, claude-code, gemini-cli]
created: 2026-10-09
updated: 2026-10-09
sources: ["[202604201800]我把Karpathy的AutoResearch搬到了软件开发领域效果炸了.html"]
---
# imclaw

imclaw（写作 IMClaw）是鸟窝（GitHub 账号 smallnest）的开源工具（`github.com/smallnest/imclaw`），据本文推荐阅读栏目的文章标题描述，其功能定位为"通过微信/飞书操控 ClaudeCode/Codex/GeminiCLI/Pi Agent 蜂群"——即以 IM（微信/飞书）为通道的编码 Agent 蜂群控制工具。

## 与本文的关系

- **设计灵感来源之一**：smallnest/autoresearch 的设计灵感节将 imclaw 列为三项灵感之一，但表述为"本项目和 autoresearch 文件"，与 IMClaw 的实际功能（IM 蜂群控制）如何对应仍未解释，属本文遗留疑点
- **演示载体**：本文的自动化实现演示 Issue #21 链接（`https://github.com/smallnest/imclaw/issues/21`）挂在 imclaw 仓库；三个实战案例（#21/#15/#6）的代码路径（`internal/job/`、`internal/gateway/server.go`）提示案例可能属该仓库，但未获正文确认
- **被操控对象**：按推荐阅读标题，imclaw 可操控 [[codex]]、[[claude-code]]、GeminiCLI 与 [[pi-agent]] 蜂群，覆盖 [[openclaw]] 生态

## 操控层谱系定位

imclaw 与 [[acpx]] 同为 smallnest 的 Agent 操控层作品：acpx 走命令行通道，imclaw 走 IM 通道，均属"人经由中间层调度编码 Agent"的实例。同账号另有《IMClaw》专题文章与 GoClaw（OpenClaw 的 Go 重写版）可作延伸采集线索。
