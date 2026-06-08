---
type: concept
title: 任务隔离（Task Isolation）
tags: [上下文管理, 任务隔离, agent]
related: [append-only-context, compression-strategy, conversation-driven-vs-task-driven, openclaw]
created: 2026-06-08
updated: 2026-06-08
sources: ["从OpenClaw看Agent架构设计.html"]
---

# 任务隔离（Task Isolation）

**任务隔离** 是一种上下文管理策略：以任务为单位，为不同任务分配**独立上下文窗口**，避免无关话题互相干扰。

## 优势

- 从源头避免信息混杂，无需依赖 [[compression-strategy|压缩]]。
- 每个任务的上下文聚焦，推理质量更高。

## 代价

- 牺牲**跨任务的连贯性**。
- 需要设计任务间信息传递机制（当前为开放问题）。

## 与主循环的关系

任务隔离与 [[conversation-driven-vs-task-driven|任务驱动]] 模式天然契合，是"对话前端 + 任务后端"混合架构的基础。

## 开放问题

- 如何设计跨任务的高效信息传递机制？

## 参见

- [[append-only-context]]
- [[conversation-driven-vs-task-driven]]
- [[architectural-decision-interdependence]]