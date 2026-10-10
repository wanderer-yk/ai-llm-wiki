---
type: concept
title: memory-flush 机制
tags: [openclaw, 长期记忆, 上下文压缩]
related: [openclaw, 记忆写入双路径, autocompact水位线机制, 三层上下文压缩, 函数结果清理机制]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604151800]OpenClaw长期记忆优秀管线与玄学效果.html"]
---
# memory-flush 机制

Memory Flush 是 [[openclaw]] 原生记忆系统的 Compaction（上下文压缩）前安全网：会话临近自动压缩时强制进行一次记忆提取落盘，是短对话丢失信息的"最后一次救济机会"。

## 触发条件（双阈值）

| 参数 | 默认值 | 含义 |
|---|---|---|
| `softThresholdTokens` | 4000 | 距 Compaction 的 token 距离阈值 |
| `forceFlushTranscriptBytes` | 2MB | 转录文件大小阈值（防止 token 计数器过时） |

## 提取指令（原文）

```
Pre-compaction memory flush turn.
The session is near auto-compaction; capture durable memories to disk.
Store durable memories only in memory/YYYY-MM-DD.md.
Treat MEMORY.md, DREAMS.md, SOUL.md, TOOLS.md, AGENTS.md as read-only during this flush.
If nothing to store, reply with NO_REPLY.
```

## 权限收紧

Flush 期间 `write` 工具被包装为 `appendOnly` 仅追加模式：只允许写当天日记忆文件，不能覆盖已有内容；其余五个记忆文件（MEMORY.md/DREAMS.md/SOUL.md/TOOLS.md/AGENTS.md）只读；无内容可存则回复 NO_REPLY。

## 局限与对照

- 局限：仅在接近压缩阈值时触发，短对话无安全网（[[原生记忆不确定性链路]] 第二环）；[[RDSClaw]] 以 `agent_end` 钩子每轮触发补强。
- 跨来源对照：同为压缩前的记忆抢救 ↔ Claude Code 的 [[autocompact水位线机制]]、[[三层上下文压缩]]、[[函数结果清理机制]]。
