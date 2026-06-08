---
type: concept
title: 对话驱动 vs 任务驱动
tags: [主循环, agent, 架构]
related: [perceive-think-act-loop, thought-process-as-first-class-citizen, task-isolation, model-training-bias, architectural-decision-interdependence]
created: 2026-06-08
updated: 2026-06-08
sources: ["从OpenClaw看Agent架构设计.html"]
---

# 对话驱动 vs 任务驱动

**对话驱动 vs 任务驱动** 是 Agent 主循环设计的两种根本架构模式。

## 对话驱动

- 以**对话框**为核心容器。
- 用户输入 → 模型回复 → 循环。
- 优势：门槛低，符合用户习惯。
- 局限：对话框是唯一容器，难以接入 Cron、Webhook 等其他任务源。

## 任务驱动

- 以 [[perceive-think-act-loop|感知-思考-行动]] 循环为核心。
- 聊天框**降级为众多工具之一**（与 Cron、Webhook 并列）。
- 将[[thought-process-as-first-class-citizen|思考过程]]作为核心输出，增强可观测性与可追溯性。

## 现实矛盾

任务驱动的愿景与当前模型受 [[model-training-bias|RLHF]] 影响而偏好"直接对话回复"的现实冲突，导致纯任务驱动落地受限。

## 最佳实践：混合模式

> **对话作为前端收集需求，任务作为后端隔离执行。**

- 前端：对话驱动，低门槛接入用户。
- 后端：任务驱动，隔离执行、可观测、可追溯。

## 参见

- [[task-isolation]]
- [[perceive-think-act-loop]]
- [[architectural-decision-interdependence]]