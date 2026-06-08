---
type: concept
title: Prompt 缓存机制
tags: [缓存, 成本控制, api]
related: [anthropic, claude-code, prompt-based-tool-injection, progressive-tool-loading, mcp]
created: 2026-06-08
updated: 2026-06-08
sources: ["从OpenClaw看Agent架构设计.html"]
---

# Prompt 缓存机制

**Prompt 缓存机制** 是 [[anthropic|Anthropic]] 等厂商提供的 API 特性，通过**严格前缀匹配**避免重复计算，大幅降低成本与延迟。

## 工作原理

缓存按 `system → tools → messages` 顺序构建前缀。只要前缀保持不变，后续请求即可命中缓存。

## 关键约束

- **严格前缀匹配**：任何前缀部分的变动都会导致缓存失效。
- `tools` 字段位于前缀中部，因此**工具列表的任何变化都会破坏整个 messages 的缓存**。

## 与动态工具加载的矛盾

这是 Agent 工具加载设计的核心矛盾：

- [[claude-code]] 通过**永不改变 `tools` 列表**实现 92% 缓存命中。
- [[mcp]] 的动态工具发现会破坏缓存前缀。

## 折中方案

- [[prompt-based-tool-injection|Prompt 级工具注入]]：工具描述移入 messages
- [[progressive-tool-loading|渐进式加载]]：只追加不修改
- [[console-vs-mcp-strategy|控制台 + MCP 混合]]

## 参见

- [[anthropic]]
- [[claude-code]]
- [[agent-architecture-design]]