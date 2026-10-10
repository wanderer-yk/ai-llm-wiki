---
type: entity
title: stitch-loop（Skill）
tags: [skill, 接力棒循环, 跨session, 文件即状态]
related: [google-labs-code-stitch-skills, skill接力棒循环模式, file-as-progress状态持久化, skill循环迭代模式]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604270830]工作流的Skill怎么写从7个顶级Skill中提炼的模式与最佳实践.html"]
---
# stitch-loop（Skill）

`stitch-loop` 是 [[google-labs-code-stitch-skills|google-labs-code/stitch-skills]] 仓库中的一个 203 行 Skill，是 [[青斧]] 七个分析对象中的第 5 个、接力棒循环模式（见 [[skill接力棒循环模式]]）的代表。核心思想是"文件即状态，跨 session 持久化"：以 `next-prompt.md` 作为接力棒文件，LLM 无需记住"上次做到哪了"，状态全部外置到文件系统。

结构为：标题 → Overview → The Baton System → Execution Protocol（6 步）→ File Structure Reference → Orchestration Options。关键设计包括：Step 6（写接力棒）标记为 Critical + MUST 的"续命机制"——忘记写接力棒则循环断裂；以及"编排无关"特性——同一 Skill 可由 CI/CD、人在回路或 Agent 链任一方式驱动。与 [[skill循环迭代模式]] 的本质区别在状态存储位置：对话上下文（单次会话，分钟~小时）vs 外部文件（长期项目，天~周）。"文件即状态"与 [[file-as-progress状态持久化]] 强互证。
