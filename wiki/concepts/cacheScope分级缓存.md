---
type: concept
title: cacheScope分级缓存
created: 2026-10-10
updated: 2026-10-10
tags: [claude-code, kv-cache, prompt-caching, 性能优化]
related: [cacheScope分级缓存, system-prompt动态组装机制, 快照冻结与前缀缓存, 三层上下文压缩, microcompact工具白名单]
sources: ["[202604200830]深度解析ClaudeCode在PromptContextHarness的设计与实践.html"]
---
# cacheScope分级缓存

**cacheScope 分级缓存**是 [[claude-code]] 为提升 KV Cache 命中率而对 System Prompt 数组做的显式分块策略：由 `constants/systemPromptSections.ts` 的 `splitSysPromptPrefix()` 将 Prompt 拆为缓存友好块，按 `cacheScope` 字段标记各级缓存归属。

## 分块结构（原文还原）

```javascript
[
  { text: "x-anthropic-billing-header: .", cacheScope: null },    // 归属头（永不缓存）
  { text: "You are Claude Code.",          cacheScope: 'org' },   // 前缀
  { text: "静态内容（边界前）",                cacheScope: 'global' }, // 全局缓存
  { text: "动态内容（边界后）",                cacheScope: null },    // 不缓存
]
```

`__SYSTEM_PROMPT_DYNAMIC_BOUNDARY__` 边界线是分块依据：归属头永不缓存、静态边界前全局缓存、动态边界后不缓存。该设计与 MicroCompact 的「仅 KV Cache 边界外压缩」路径（[[microcompact工具白名单]]）及 [[快照冻结与前缀缓存]]（HermesAgent 侧同类机制）直接相关，共同构成「缓存感知的上下文工程」：上下文变更必须尊重缓存边界，否则成本与延迟显著上升。
