---
type: concept
title: 模型训练倾向（RLHF 导致的直接回复偏好）
tags: [rlhf, 训练, agent]
related: [conversation-driven-vs-task-driven, perceive-think-act-loop, anthropic]
created: 2026-06-08
updated: 2026-06-08
sources: ["从OpenClaw看Agent架构设计.html"]
---

# 模型训练倾向

**模型训练倾向** 指当前大语言模型经过 RLHF（基于人类反馈的强化学习）训练后，天然倾向于**直接对话回复**，而非调用工具或进入任务执行流程。

## 对 Agent 架构的影响

这是任务驱动模式落地的核心障碍：

- 模型"本能"是直接回答用户问题。
- 在任务驱动场景下，模型应先感知、思考、规划，再行动。
- 训练偏好与架构愿景产生冲突。

## 应对策略

- 采用"对话转任务"混合模式（参见 [[conversation-driven-vs-task-driven]]）。
- 通过 Prompt 设计引导模型进入任务模式。

## 开放问题

- 模型厂商应如何针对 Agent 模式优化底层训练？

## 参见

- [[conversation-driven-vs-task-driven]]
- [[perceive-think-act-loop]]
- [[anthropic]]