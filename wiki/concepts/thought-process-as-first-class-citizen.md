---
type: concept
title: 思考过程作为一等公民
tags: [可观测性, 任务驱动, 思考链]
related: [perceive-think-act-loop, conversation-driven-vs-task-driven, openclaw]
created: 2026-06-08
updated: 2026-06-08
sources: ["从OpenClaw看Agent架构设计.html"]
---

# 思考过程作为一等公民

**思考过程作为一等公民** 是任务驱动 Agent 的一项核心理念：将模型的思考链（reasoning trace）提升为**核心输出**，而非隐藏的中间状态。

## 价值

- **可观测性**：Agent 的决策过程对外可见。
- **可追溯性**：错误可定位到具体思考步骤，便于调试。
- **解决黑盒问题**：从"不可解释的输出"转变为"可审查的推理"。

## 在 OpenClaw 中的体现

[[openclaw]] 的任务驱动模式将思考链作为一等输出，与行动记录并列保存。

## 参见

- [[perceive-think-act-loop]]
- [[conversation-driven-vs-task-driven]]
- [[agent-architecture-design]]