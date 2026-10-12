---
type: concept
title: ReAct 双引擎场景适配
tags: [执行引擎, 选型框架, ReAct]
related: [ReAct推理模块, 问题排查场景方案, workflow优先于agent, reflection模式]
created: 2026-10-12
updated: 2026-10-12
sources: ["[202603230830]拒绝重复造轮子抽象80工作场景打造可复用的AI助手工厂.html"]
---
# ReAct 双引擎场景适配

ReAct 双引擎场景适配是问题排查场景方案（[[问题排查场景方案]]）的执行引擎选型框架：提供简化版与完整版两个执行引擎，并按场景特征给出推荐组合。

## 双引擎

- **ReActExecutor（简化版）**：Thought→Action，少一步 LLM 调用、响应快，但复杂逻辑效果可能不好。
- **ReActObversationExecutor（完整版）**：Thought→Action→Observation，职责清晰、支持任务规划协调重试/错误恢复，但耗时较长。（注：原文拼写 ReActObversationExecutor 疑为 ReActObservationExecutor 笔误，此处保留原文拼写并注记。）

## 场景适配表（原文保真）

| 场景类型 | 推荐引擎 | 原因 |
|---------|---------|------|
| 简单排查（答疑） | ReActExecutor | 快速响应（少一步LLM调用），无需复杂评估。 |
| 多步骤业务流程 | ReActObversationExecutor | 需动态评估每一步结果，确保流程正确性。 |
| 需要错误恢复 | ReActObversationExecutor | 观察者可触发重试或回退策略。 |

## 关联

- "简单场景用确定性快速路径、复杂场景才上完整推理循环"与 [[workflow优先于agent]] 的分阶段策略同频；带 Observation 反思重试的完整版与 [[reflection模式]] 呼应。
