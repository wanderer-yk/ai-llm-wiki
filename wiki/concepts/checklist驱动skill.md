---
type: concept
title: checklist 驱动 skill
created: 2026-10-09
updated: 2026-10-09
tags: [skill, checklist, prompt工程]
related: [skill稳定性决定论, skill分层文件管理, 提取校验修复三skill拆分, skill步骤间校验]
sources: ["[202605201800]网盘存量代码迁移实战我们如何用三层架构管住AI的输出.html"]
---
# checklist 驱动 skill

**checklist 驱动 skill** 是本文总结的写 Skill 最有效约束手段：在 Skill 中加入 Checklist，让 AI 逐项打勾执行。其针对的失效模式是多步骤任务中的**跳步遗漏**——AI 在长指令中容易跳过中间步骤，逐项打勾把"理解式执行"变成"核对式执行"。

## 在本文中的位置

Checklist 与另外两项手段共同构成 Skill 层的稳定性方案（[[skill稳定性决定论]]）：

1. Checklist：解决多步骤任务跳步（本文认定"最有用的动作"）。
2. 分层文件管理：核心规则主文件 + references 目录（[[skill分层文件管理]]）。
3. 角色拆分：extractor → validator → fixer，避免自审放水（[[提取校验修复三skill拆分]]）。

## 与既有概念的关系

与 [[skill步骤间校验]]（在步骤之间设置校验点）思路同向：都是把"事后发现遗漏"前移为"过程内强制核对"；区别在于 Checklist 是 Skill 文本内的逐项核对机制，步骤间校验是流程编排层面的校验点设计。