---
type: entity
title: Claude Code
tags: [agent, anthropic, 对比对象]
related: [openclaw, anthropic, append-only-context, compression-strategy, prompt-cache-mechanism]
created: 2026-06-08
updated: 2026-06-08
sources: ["从OpenClaw看Agent架构设计.html"]
---

# Claude Code

**Claude Code** 是 [[anthropic|Anthropic]] 推出的命令行 Agent 产品，在本文中作为 [[openclaw]] 的主要对比对象。

## 架构特征

### 上下文管理

- 采用 [[append-only-context|追加式上下文]] 模式。
- 压缩策略：**摘要前置**，在上下文逼近窗口上限时触发，会话间不保留。

### 工具加载与缓存

- 报告 **92% 的 Prompt 缓存命中率**，关键原因在于它**永不改变 `tools` 列表**，保证 [[prompt-cache-mechanism|前缀缓存]] 稳定。
- 这种"工具列表固定"的取舍，是 Claude Code 实现高缓存命中、低成本推理的核心。

## 与 OpenClaw 的对比

| 维度 | Claude Code | OpenClaw |
|------|-------------|----------|
| 压缩方式 | 摘要前置 | 记忆落盘 + 渐进压缩 |
| 跨会话保留 | 否 | 是 |
| 工具列表 | 固定（保缓存） | 可动态（牺牲部分缓存） |

## 参见

- [[prompt-cache-mechanism]]
- [[console-vs-mcp-strategy]]
- [[append-only-context]]