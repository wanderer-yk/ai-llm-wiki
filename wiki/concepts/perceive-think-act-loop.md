---
type: concept
title: 感知-思考-行动循环
tags: [主循环, 任务驱动, agent]
related: [conversation-driven-vs-task-driven, thought-process-as-first-class-citizen, task-isolation]
created: 2026-06-08
updated: 2026-06-08
sources: ["从OpenClaw看Agent架构设计.html"]
---

# 感知-思考-行动循环

**感知-思考-行动循环** 是任务驱动 Agent 的核心运行机制，构成了 Agent 的主循环。

## 三阶段

1. **感知（Perceive）**：从输入源（聊天、Cron、Webhook 等）接收任务或事件。
2. **思考（Think）**：进行推理规划，生成思考链（参见 [[thought-process-as-first-class-citizen]]）。
3. **行动（Act）**：调用工具执行，产生结果并反馈。

## 与对话驱动的区别

- 对话驱动：输入局限于对话框，输出为对话回复。
- 任务驱动：输入源多元，输出包含结构化的思考过程与行动记录。

## 价值

- 打破对话框作为唯一容器的局限。
- 使 Agent 行为可观测、错误可追溯。

## 参见

- [[conversation-driven-vs-task-driven]]
- [[thought-process-as-first-class-citizen]]
- [[agent-architecture-design]]