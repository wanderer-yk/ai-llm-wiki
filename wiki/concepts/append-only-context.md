---
type: concept
title: 追加式上下文（Append-Only Context）
tags: [上下文管理, agent, 缓存]
related: [compression-strategy, task-isolation, prompt-cache-mechanism, claude-code, openclaw]
created: 2026-06-08
updated: 2026-06-08
sources: ["从OpenClaw看Agent架构设计.html"]
---

# 追加式上下文（Append-Only Context）

**追加式上下文** 是一种 Agent 上下文管理模式：Agent 维护持续增长的对话历史，每次调用 LLM 时发送**全量上下文**，只增不减。

## 优势

- **缓存利用率高**：历史前缀不变，天然契合 [[prompt-cache-mechanism|Prompt 缓存]]。
- **实现简单**：无需复杂的状态管理。
- **上下文连贯**：完整保留对话线索。

## 问题

- **成本与请求复杂度脱钩**：即使当前请求极简单（如"你好"），仍需发送全部历史。文章以 Opus 定价为例，80K Token 历史的单次请求约花费 $0.30。
- **无关信息干扰推理**：历史信息无差别塞入，可能干扰当前任务的推理质量。

## 采用者

- [[openclaw]]
- [[claude-code]]

## 缓解措施

- [[compression-strategy|压缩策略]]：对膨胀的上下文进行摘要/落盘
- [[task-isolation|任务隔离]]：从源头避免信息混杂

## 参见

- [[prompt-cache-mechanism]]
- [[agent-architecture-design]]