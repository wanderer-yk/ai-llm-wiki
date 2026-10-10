---
type: concept
title: 动态 Skill 生成
tags: [skill, 自进化, 复盘, hermes-agent]
related: [hermes-agent, 内外双路径自进化, 后台审查agent, 外挂式进化vs权重内化, 生产级skill, skill-for-skill元技能自举, openclaw]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604240830]深度解析HermesAgent如何实现自进化及其PromptContextHarness的设计实践.html"]
---
# 动态 Skill 生成

动态 Skill 生成是 [[hermes-agent]] “外路径”自进化机制：任务结束后启动“复盘”流程，将执行轨迹中的踩坑、纠错手段与人工验证过的最佳实践抽象为结构化 Skill 文件包，实现 Skill 的**自动生成、持续优化、持续积累**——Skill 从“静态调用”变为“动态生成”。

## 与 OpenClaw 静态 Skill 的对比

来源文章批评 [[openclaw]] 的执行过程“无状态”：试错/自我纠正/人工引导经验不自动沉淀，Memory 只记简要重点与用户习惯，智能上限锁定于“基座模型 + 静态提示词 + 静态 Skill”。Hermes 以动态 Skill 生成 + [[SQLite全量对话持久化]] 的轨迹资产对此补齐。

## 实现细节（`run_agent.py`）

| 标识符 | 作用 |
|---|---|
| `_iters_since_skill` | 距上次使用 `skill_manage` 工具的轮数计数器（技能催促计数器） |
| `_skill_nudge_interval = 10` | 连续 10 轮未创建/修改技能时“提醒”Agent 整理经验 |
| `_spawn_background_review` | 回复完成后异步启动后台审查 Agent（见 [[后台审查agent]]） |

## 关联

- 与 [[生产级skill]]（腾讯）的差异：后者是人工 SOP 封装，前者是运行时自动沉淀。
- 与 [[skill-for-skill元技能自举]] 的关系：都把“生成/改进 Skill”作为 Agent 的元能力，Hermes 额外叠加了催促计数器与后台审查的工程化触发机制。
- Skill 外挂式进化的深度局限见 [[外挂式进化vs权重内化]]。