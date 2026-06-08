---
type: concept
title: 控制台 + MCP 混合策略
tags: [工具加载, mcp, 控制台]
related: [execute, mcp, prompt-cache-mechanism, progressive-tool-loading]
created: 2026-06-08
updated: 2026-06-08
sources: ["从OpenClaw看Agent架构设计.html"]
---

# 控制台 + MCP 混合策略

**控制台 + MCP 混合策略** 是 [[openclaw|OpenClaw]] 提出的工具加载最佳实践：根据能力的来源与频率，采用不同的加载方式。

## 分工

| 能力类型 | 加载方式 | 理由 |
|----------|----------|------|
| 本地高频能力 | [[execute]] 控制台（单工具） | 大模型已具备世界知识；`tools` 列表不变，缓存命中 100% |
| 远程鉴权服务 | [[mcp]] | 标准化接口与鉴权，动态发现 |

## 核心收益

- 本地能力通过单一 `execute` 工具实现零学习成本与缓存稳定。
- 远程能力通过 MCP 获得标准化与安全性。
- 整体在灵活性与缓存命中之间取得平衡。

## 参见

- [[prompt-cache-mechanism]]
- [[progressive-tool-loading]]
- [[prompt-based-tool-injection]]