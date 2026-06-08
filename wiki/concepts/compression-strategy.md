---
type: concept
title: 压缩策略（Compression Strategy）
tags: [上下文管理, 压缩, agent]
related: [append-only-context, task-isolation, claude-code, openclaw]
created: 2026-06-08
updated: 2026-06-08
sources: ["从OpenClaw看Agent架构设计.html"]
---

# 压缩策略

**压缩策略** 是 [[append-only-context|追加式上下文]] 模式下，当上下文逼近窗口上限时，减小上下文体积的机制。它只是对膨胀的**缓解**而非**解决**，因为会引入额外 LLM 调用成本并造成摘要信息损失。

## 两种代表做法

| 维度 | [[claude-code]] | [[openclaw]] |
|------|-----------------|--------------|
| 触发时机 | 逼近窗口上限 | 逼近窗口上限 |
| 方式 | 摘要前置 | 记忆落盘 + 渐进压缩 |
| 跨会话保留 | 否 | 是 |

## 局限

- 引入额外 LLM 调用成本。
- 摘要有损，可能丢失关键信息。
- 本质上是"治标"，源头治理需依赖 [[task-isolation|任务隔离]]。

## 参见

- [[append-only-context]]
- [[task-isolation]]
- [[agent-architecture-design]]