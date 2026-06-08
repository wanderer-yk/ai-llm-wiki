---
type: concept
title: 渐进式工具加载
tags: [工具加载, 缓存, agent]
related: [prompt-cache-mechanism, prompt-based-tool-injection, console-vs-mcp-strategy]
created: 2026-06-08
updated: 2026-06-08
sources: ["从OpenClaw看Agent架构设计.html"]
---

# 渐进式工具加载

**渐进式工具加载** 是一种随对话流按需追加新工具的策略，已有缓存完全保留。

## 原理

- 工具描述放入 `messages`（参见 [[prompt-based-tool-injection]]）。
- 新工具只**追加**到上下文末尾，不修改已有前缀。
- 由于追加不改变已有前缀，缓存得以保留。

## 优势

- 兼顾动态加载的灵活性与 [[prompt-cache-mechanism|缓存]] 的成本控制。
- 与 [[append-only-context|追加式上下文]] 模式天然契合。

## 代价

- 上下文体积随工具增加而增长。
- 依赖 Prompt 级注入，结构化保证较弱。

## 参见

- [[console-vs-mcp-strategy]]
- [[prompt-cache-mechanism]]
- [[agent-architecture-design]]