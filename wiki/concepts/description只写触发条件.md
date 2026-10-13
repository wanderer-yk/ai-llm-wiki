---
type: concept
title: description 只写触发条件
created: 2026-10-13
updated: 2026-10-13
tags: [skill, description, writing-skills, 最佳实践]
related: [模型驱动触发逻辑, 操作型skill触发失效, skill渐进式加载机制, skillmd格式规范]
sources: ["[202605120830]AgentSkill规范构建与设计模式.html"]
---
# description 只写触发条件

"description 只写触发条件"是 Writing-Skills（[[superpowers插件]]）的方法论发现，被来源作者标注为"**writing-skills 中最重要的发现之一**"：description 字段只应描述**何时使用该 Skill**（触发条件），**绝不要总结工作流程**——否则 Agent 会按 description 的摘要走捷径，跳过 Skill 正文直接执行，直接削弱渐进式加载的 L2/L3 层。

## 好/坏对照

```yaml
# ❌ 总结了工作流 → Agent 可能走捷径，跳过 Skill 正文
description: Use when executing plans - dispatches subagent per task
  with code review between tasks

# ✅ 只有触发条件 → Agent 会完整阅读 Skill
description: Use when executing implementation plans with independent
  tasks in the current session
```

## 总结层升级

来源总结章进一步将该原则升格为："写好 description **比写好指令主体更重要**"。依据是双重作用：description 既决定触发召回（[[模型驱动触发逻辑]]），又决定触发后正文阅读完整度。

## 待裁决张力

该声明与 Issue #514"操作型 Skill recall=0% 与 description 质量无关"（[[操作型skill触发失效]]）构成双向约束张力：对需要领域知识的任务，description 质量决定召回与阅读深度；对无需知识的操作型任务，description 优化完全无效。两说的适用边界待 wiki 侧裁决。
