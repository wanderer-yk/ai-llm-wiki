---
type: entity
title: Anthropic
tags: [厂商, 大模型, api]
related: [claude-code, prompt-cache-mechanism, opus]
created: 2026-06-08
updated: 2026-06-08
sources: ["从OpenClaw看Agent架构设计.html"]
---

# Anthropic

**Anthropic** 是一家大模型厂商，提供 Claude 系列模型及其 API 服务。

## 在 Agent 架构中的关键贡献

### Prompt 缓存机制

Anthropic API 提供严格的 [[prompt-cache-mechanism|前缀缓存]]，按 `system → tools → messages` 顺序构建缓存前缀。一旦 `tools` 字段发生变化，缓存即失效。

这一机制直接塑造了 [[claude-code|Claude Code]] 与 [[openclaw|OpenClaw]] 的工具加载策略：

- Claude Code 固定 `tools` 列表换取 92% 缓存命中。
- OpenClaw 通过 [[prompt-based-tool-injection|Prompt 级工具注入]] 与 [[progressive-tool-loading|渐进式加载]] 寻求折中。

### 代表模型

- **Opus**：高端模型，本文以其定价举例（80K Token 约 $0.30）说明追加式上下文的成本问题。

## 参见

- [[prompt-cache-mechanism]]
- [[claude-code]]
- [[append-only-context]]