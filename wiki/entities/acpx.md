---
type: entity
title: acpx
tags: [acpx, agent操控层, 命令行工具, codex, claude-code]
related: [smallnest-autoresearch, imclaw, codex, claude-code, 自动化决策层级, 24h打工人]
created: 2026-10-09
updated: 2026-10-09
sources: ["[202604201800]我把Karpathy的AutoResearch搬到了软件开发领域效果炸了.html"]
---
# acpx

acpx 是一个 Agent 控制工具，据本文描述其作用是"让 Codex/Claude 在命令行中协作"，即在命令行中操控 Claude Code 和 Codex 等编码 Agent。本文未给出其代码仓库地址与维护方，身份描述仅来自正文与设计灵感节。

## 在 smallnest/autoresearch 中的三重角色

1. **运行前置条件**：环境准备阶段以 `which acpx` 检查可用性（与 gh、go 并列三件套）
2. **会话建立**：四阶段循环 Phase 1 中"创建分支 + acpx session"，为双 Agent 迭代提供命令行会话载体
3. **设计灵感来源**：作者在设计灵感节明确致谢"acpx —— Agent 命令行协作控制"

## 定位与对照

acpx 属 Agent 操控层的命令行（CLI）通道实例，与同作者（[[鸟窝]]/smallnest）的 [[imclaw]]（IM 通道：通过微信/飞书操控编码 Agent 蜂群）构成操控层对照组。二者与 zhiyuanfu 的 [[24h打工人]] 终端调度、ConardLi 体系的 tmux 驱动同构，均是 [[自动化决策层级]] 中"CLI 层调度编码 Agent"的具体实现；区别在于 acpx 服务于单任务内的双 Agent 协作会话，imclaw 服务于跨会话的蜂群控制。
