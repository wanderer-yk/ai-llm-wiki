---
type: concept
title: skill 稳定性决定论
created: 2026-10-09
updated: 2026-10-09
tags: [skill, 稳定性, 三层架构]
related: [三层架构管住AI输出, checklist驱动skill, skill错误倒逼生成, skill质量等式, 提取校验修复三skill拆分]
sources: ["[202605201800]网盘存量代码迁移实战我们如何用三层架构管住AI的输出.html"]
---
# skill 稳定性决定论

**skill 稳定性决定论**是本文对三层架构中各层地位的判断：整个迁移方案的稳定性最终由 Skill 的规范质量决定，SubAgent 与 Agent Team 只是调度骨架。调度层决定"任务怎么分"，但每个节点"做得稳不稳"完全取决于 Skill 写得好不好。

## 两个关键表述

- Skill 要"**约束执行过程**"而非"**描述执行目标**"：AI 输出不稳定的根源是每次都在"重新理解"任务，不是模型问题，而是没把"怎么做"固化下来。
- Skill 的价值在于"**同一件事能做多稳**"而非"**能做多难**"——这与 wiki 既有 [[skill质量等式]] 的"做多稳 vs 做做多难"框架直接呼应。

## 配套手段

- Checklist 逐项打勾是写 Skill 最有效的约束（[[checklist驱动skill]]）。
- 核心规则主文件 + references 目录的分层文件管理（[[skill分层文件管理]]）。
- Skill 本身靠真实错误倒逼迭代产生（[[skill错误倒逼生成]]），并通过提取/校验/修复的角色拆分避免自审放水（[[提取校验修复三skill拆分]]）。