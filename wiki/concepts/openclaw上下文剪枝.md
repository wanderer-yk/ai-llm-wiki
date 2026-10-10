---
type: concept
title: OpenClaw 上下文剪枝
tags: [openclaw, 上下文管理, pruning, cache-ttl]
related: [openclaw, 工具结果头尾修剪, kv-cache时间窗优化, openclaw上下文压缩流水线]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202603190830]深入理解OpenClaw技术架构与实现原理上.html"]
---

# OpenClaw 上下文剪枝

OpenClaw 上下文剪枝（Pruning）是 [[openclaw]] 针对工具结果消息（toolResult）的内存内清理机制，实现位于 `src/agents/pi-extensions/context-pruning/`（settings.ts + pruner.ts）。与压缩（Compaction）正交：剪枝不生成摘要、不持久化，仅在每次请求前按 TTL 与占用比例对过期的工具结果做软修剪或硬清除。来源：[[sources/[202603190830]深入理解OpenClaw技术架构与实现原理上|深入理解OpenClaw技术架构与实现原理（上）]] 3.6.4 节。

## Compaction vs Pruning 四维对比（源文原表）

| 特性 | Compaction | Pruning |
|------|-----------|---------|
| 作用范围 | 整个历史 | 仅 toolResult 消息 |
| 持久化 | ✓ 写入 JSONL | ✗ 仅内存 |
| 触发时机 | 接近窗口上限 | 每次请求前 (TTL 过期时) |
| 内容变更 | 生成摘要 | 软修剪/硬清除 |

## 默认剪枝配置（verbatim）

```javascript
DEFAULT_CONTEXT_PRUNING_SETTINGS = {
  mode: "cache-ttl",
  ttlMs: 5 * 60 * 1000,        // 5 分钟 TTL
  keepLastAssistants: 3,       // 保护最后 3 条助手消息
  softTrimRatio: 0.3,          // 上下文占用 > 30% 触发软修剪
  hardClearRatio: 0.5,         // 上下文占用 > 50% 触发硬清除
  minPrunableToolChars: 50_000,
  softTrim: {
    maxChars: 4_000,           // > 4K 字符触发软修剪
    headChars: 1_500,          // 保留头部 1500 字符
    tailChars: 1_500,          // 保留尾部 1500 字符
  },
  hardClear: {
    enabled: true,
    placeholder: "[Old tool result content cleared]",
  },
}
```

## 剪枝执行流程（pruner.ts）

```text
1. 检查 TTL 是否过期
   ↓ 过期
2. 计算上下文占用比例
   ↓ 超过 softTrimRatio
3. 软修剪：对可修剪工具结果截取 head + tail
   ↓ 仍超过 hardClearRatio
4. 硬清除：替换为占位符
```

## 四重保护机制

1. 不修改用户/助手消息，只处理工具结果
2. 跳过含图片的 toolResult
3. 保护 bootstrap 阶段消息（第一条用户消息之前）
4. 保护最后 N 条（默认 3）助手消息之后的工具结果

## 关联

- [[工具结果头尾修剪]]：softTrim 保留头尾各 1500 字符是该概念的直接源码证实（双源互证）
- [[kv-cache时间窗优化]]：剪枝模式命名 `cache-ttl` 直接表明该机制以保留 KV cache 前缀为导向，5 分钟 TTL 可与既有概念对照
- [[openclaw上下文压缩流水线]]：双机制分工的另一半
- [[autocompact水位线机制]]：Claude Code 的水位线自动压缩与 OpenClaw 的比例阈值剪枝同为阈值化守卫，可对照

## 开放问题

- `minPrunableToolChars: 50_000` 与 `softTrim.maxChars: 4_000` 的确切关系
- "每次请求前"与"TTL 过期"双条件的精确组合逻辑
