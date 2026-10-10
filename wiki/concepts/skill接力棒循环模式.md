---
type: concept
title: skill 接力棒循环模式（模式 4）
tags: [skill, 设计模式, 接力棒, 跨session, 状态持久化]
related: [stitch-loop, google-labs-code-stitch-skills, skill循环迭代模式, file-as-progress状态持久化, 模式选择决策树, agent可观测性六维度]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604270830]工作流的Skill怎么写从7个顶级Skill中提炼的模式与最佳实践.html"]
---
# skill 接力棒循环模式（模式 4）

接力棒循环模式（跨 Session 持久化）是 [[青斧]] 归纳的 5 种核心设计模式之四，适用于跨多个 session 持续推进长期项目、或多 Agent 协作的场景。代表案例为 [[google-labs-code-stitch-skills|google-labs-code/stitch-skills]] 的 [[stitch-loop]]（203 行），一句话精髓"文件即状态，跨 session 持久化"。

**结构**：标题 → Overview → The Baton System → Execution Protocol（6 步）→ File Structure Reference → Orchestration Options。

**关键技巧**：文件即状态——以 `next-prompt.md` 作为接力棒文件，LLM 无需记住"上次做到哪了"，状态全部外置到文件系统（与 [[file-as-progress状态持久化]] 强互证）；续命机制——Step 6（写接力棒）标记 Critical + MUST，忘记写则循环断裂，是整个循环的"续命点"；文件协议——用文件约定交接内容；编排无关——同一 Skill 可由 CI/CD、人在回路、Agent 链任一方式驱动。

**与模式 3（[[skill循环迭代模式]]）的四维对比**：状态存储（对话上下文 vs 外部文件）；跨 session（否 vs 是）；循环退出（Checklist 打勾 vs 路线图清空）；适用时长（分钟~小时 vs 天~周）。
