---
type: concept
title: OpenClaw 上下文压缩流水线
tags: [openclaw, 上下文管理, compaction, 上下文压缩]
related: [openclaw, 自适应分块压缩, 摘要分层降级策略, 上下文压缩策略, compaction双触发模式]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202603190830]深入理解OpenClaw技术架构与实现原理上.html"]
---

# OpenClaw 上下文压缩流水线

OpenClaw 上下文压缩流水线（Compaction）是 [[openclaw]] 在会话接近/超过上下文窗口时自动执行的摘要式压缩机制，实现位于 `src/agents/compaction.ts`。核心流程为：`旧消息 → LLM 总结 → 紧凑摘要条目 → 持久化到 JSONL`。它是 [[上下文压缩策略]] 中"OpenClaw 记忆落盘+分阶段压缩"路线的源码级落实。来源：[[sources/[202603190830]深入理解OpenClaw技术架构与实现原理上|深入理解OpenClaw技术架构与实现原理（上）]] 3.6.3 节。

## 四步流水线

```text
1. estimateMessagesTokens        // Token 估算
2. chunkMessagesByMaxTokens      // 按 token 限制分块
3. summarizeWithFallback         // 带重试的摘要
4. pruneHistoryForContextShare   // 裁剪旧消息保持预算
```

产物为紧凑摘要条目，并持久化到 JSONL（与仅内存操作的 [[openclaw上下文剪枝]] 形成正交分工）。

## 自适应分块三常量

```javascript
BASE_CHUNK_RATIO = 0.4   // 基础分块比例
MIN_CHUNK_RATIO = 0.15   // 最小分块比例
SAFETY_MARGIN = 1.2      // 20% 缓冲补偿估算误差
// 当消息平均大小 > 上下文 10% 时，自动减小分块比例
```

与既有概念 [[自适应分块压缩]]（源自 [202604130830] 文章）形成罕见的**双源互证**。

## 过大消息三级降级

`isOversizedForSummary`：单条消息超过上下文 50% 即无法安全压缩。降级路径：

1. 尝试完整压缩
2. 失败则只压缩小消息并记录过大消息
3. 最终回退返回消息计数说明

与 [[摘要分层降级策略]] 互证。

## 触发与配置

- 主循环 `runEmbeddedPiAgent` 将"context overflow → 自动压缩"作为一等异常路径处理，与 [[compaction双触发模式]] 互证
- 默认配置：`compaction.mode: "auto"`、`compaction.targetTokens: 0.7`（目标占用率；精确语义——触发水位还是压缩后目标占用——待核证）
- 压缩完成后由 `src/auto-reply/reply/post-compaction-context.ts` 重注入 AGENTS.md 的 `## Session Startup` 与 `## Red Lines` 关键章节，防止规则随压缩丢失（详见 [[openclaw运行时上下文注入]]）

## 开放问题

- `summarizeWithFallback` 的重试/fallback 链细节；压缩摘要调用哪个模型、是否可配置

## 关联

- [[openclaw上下文剪枝]]：Compaction（全历史摘要式持久化）vs Pruning（仅内存清理 toolResult）双机制分工
- [[上下文压缩策略]]：OpenClaw 与 Claude Code 两条压缩路线的对比框架
- [[openclaw]]：上下文管理是系统运行的上下文核心之一
