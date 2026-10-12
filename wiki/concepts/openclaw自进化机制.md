---
type: concept
title: OpenClaw 自进化机制
tags: [openclaw, 自进化, agent架构]
related: [openclaw, memory-skill-nudge三子系统自进化闭环, openclaw双层记忆系统, openclaw-skills系统]
created: 2026-10-12
updated: 2026-10-12
sources: ["[202603260830]深入理解OpenClaw技术架构与实现原理下.html"]
---
# OpenClaw 自进化机制

OpenClaw 的自进化由**四支柱**构成（3.12 节）：

1. **可修改核心文件**：AGENTS.md / SOUL.md / MEMORY.md / memory/YYYY-MM-DD.md——用户或 Agent 皆可改；
2. **动态系统提示**：每次运行时重建，改动即时生效；
3. **Skills 扩展系统**：能力外挂（[[openclaw-skills系统]]）；
4. **自我修改能力**：agent 用 read/write/edit 修改自身文件。

系统提示内置 "OpenClaw Self-Update" 部分，提供 `config.apply` 与 `update.run` 两条自我更新指令。

## 进化循环

`用户指令/反馈 → Agent 修改文件 → 下次会话加载新内容 → 行为改变 → 持续迭代`

## 跨源对照

与 Hermes 的 [[memory-skill-nudge三子系统自进化闭环]] 同题不同构：OpenClaw 靠**文件系统即真相**（[[文件即真相]]）+ 运行时重建，Hermes 靠**后台审查+量化评分门控**——前者简单直接但质量靠文件纪律，后者可控但复杂。与 [[外挂式进化vs权重内化]] 的外挂路线一致。
