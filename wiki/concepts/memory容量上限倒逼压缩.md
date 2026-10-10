---
type: concept
title: memory 容量上限倒逼压缩
tags: [hermes-agent, memory, 上下文工程]
related: [hermes-agent, memory-skill-nudge三子系统自进化闭环, 快照冻结与前缀缓存, openclaw, 追加式上下文]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604230830]深入源码HermesAgent如何实现SelfImproving.html"]
---
# memory 容量上限倒逼压缩

memory 容量上限倒逼压缩是 [[hermes-agent]] Memory 子系统的容量治理机制：MEMORY.md 限 2200 字符、USER.md 限 1375 字符；写入超限时不让 `add` 静默成功、也不由系统自动压缩，而是返回失败并附上全部 `current_entries`，把"淘汰/合并"决策交还给模型——从 Agent 视角看，这构成一次被强制触发的自我反思（哪些记忆值得保留）。

## 源码证据

```python
# tools/memory_tool.py:248-259
if new_total > limit:
    current = self._char_count(target)
    return {
        "success": False,
        "error": (
            f"Memory at {current:,}/{limit:,} chars. "
            f"Adding this entry ({len(content)} chars) would exceed the limit. "
            f"Replace or remove existing entries first."
        ),
        "current_entries": entries,
        "usage": f"{current:,}/{limit:,}",
    }
```

## 设计理由（文章设计取舍表第 1 条）

低质量 Memory 注入系统提示词 = 每次 API 调用都带噪声；容量上限迫使 Agent 只挑重要的记。

## 对比与边界

- 文章对照 [[openclaw]]：MEMORY.md 纯追加、数月膨胀成"几万行的怪兽文件"（作者单方定性）。
- ⚠️ 注意：此处 OpenClaw 的"纯追加膨胀"指**持久记忆文件**，与 vivo 文章的 [[追加式上下文]]（会话历史数组 append-only、缓存友好）描述对象不同，引用须区分。