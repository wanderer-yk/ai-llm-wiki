---
type: concept
title: Prompt 级工具注入
tags: [工具加载, 缓存, prompt]
related: [prompt-cache-mechanism, progressive-tool-loading, console-vs-mcp-strategy]
created: 2026-06-08
updated: 2026-06-08
sources: ["从OpenClaw看Agent架构设计.html"]
---

# Prompt 级工具注入

**Prompt 级工具注入** 是一种缓解 [[prompt-cache-mechanism|缓存]] 与动态工具加载冲突的策略：将工具描述放入 `messages` 字段而非 `tools` 字段，避免破坏前缀缓存。

## 优势

- 兼容 [[append-only-context|追加式上下文]] 模式。
- 不破坏已有的缓存前缀。
- 与 [[progressive-tool-loading|渐进式加载]] 天然契合（只追加）。

## 代价

- 失去 `tools` 字段的结构化保证。
- 可能降低自定义 API 的解析可靠性。
- 模型对 Prompt 内工具的调用准确率可能低于结构化 `tools`。

## 适用场景

适合低频动态工具；高频核心工具仍建议放在 `tools` 字段（参见 [[console-vs-mcp-strategy]]）。

## 参见

- [[prompt-cache-mechanism]]
- [[progressive-tool-loading]]
- [[agent-architecture-design]]